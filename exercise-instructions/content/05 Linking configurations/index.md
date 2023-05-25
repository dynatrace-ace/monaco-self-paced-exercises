## Exercise 5: Linking configurations

In this exercise, we'll see how we can link multiple configurations without having to figure out the IDs of those linked configurations. Instead, we'll reference them using Monaco configuration instances and let Monaco figure out the actual IDs, dependencies and priorities!

### Step 1 - Take a look at the projects

Please [download the exercise-5 sample](https://github.com/dynatrace-ace/monaco-self-paced-exercises/blob/main/exercise-samples-to-download/exercise-05-to-download.zip), unzip and copy it into a newly created folder.

```bash
mkdir exercise-05
cd exercise-05
mv <DOWNLOADED-EXERCISE-FOLDER-PATH> .
```

You will find a standard Monaco setup:

```text
├─── apps
│    ├── app-one
│    │    └── _config.yaml
│    └── shared
│         ├── alerting-profile.json
│         ├── management-zone.json
│         └── notification.json
└── infrastructure
│    ├── _config.yaml
│    ├── alerting-profile.json
│    └── management-zone.json
├── manifest.yaml
```

There are two projects, `apps` and `infrastructure`. Both projects contain configurations with dependencies.

The `apps` project has the following dependencies:
    `notification` > `alerting-profile` > `management-zone`

And the `infrastructure` project has the following dependency:
    `alerting-profile` > `management-zone`

If we link these configurations together, Monaco can create all of them in one go. If you take a look at the `manifest.yaml` file, you'll notice that the `apps` project has been commented out. We'll start with the `infrastructure` project and show you how this works and it'll be up to you to implement the `apps` project.

### Step 2 - Link the alerting profile to the management zone

1. Open `exercise-05/infrastructure/_config.yaml`

    ```yaml
    ---
    configs:
      # management-zone 
      - id: management-zone-k8s-nodes
        config:
          name: k8s-nodes
          template: management-zone.json
          skip: false
        type:
          api: management-zone

      # alerting-profile
      - id: alerting-profile-k8s-nodes
        config:
          name: profile-k8s-nodes
          parameters:
            mzId: ""
          template: alerting-profile.json
          skip: false
        type:
          api: alerting-profile
    ```

    We can see that there are two configurations, whose ids are `management-zone-k8s-nodes` and `alerting-profile-k8s-nodes`.

    Configuration `alerting-profile-k8s-nodes` requires a parameter called `mzId`. This is used in the creation of the alerting profile.

2. Open `exercise-05/infrastructure/alerting-profile.json`.

    At the bottom, it contains two variables that point to the management-zone IDs.

    ```json
    "managementZoneId": "{{.mzId}}",
    "mzId": "{{.mzId}}",
    "eventTypeFilters": []
    ```

    No changes are needed in this file.

3. In `infrastructure/_config.yaml`, looking at the definitions `alerting-profile-k8s-nodes`  configuration the `mzId` variable has been created, we need to supply the mapping for this variable.

    ```yaml
     # alerting-profile
     - id: alerting-profile-k8s-nodes
        config:
          name: profile-k8s-nodes
          parameters:
            mzId: ""
          template: alerting-profile.json
          skip: false
        type:
          api: alerting-profile
    ```

    For the `mzId` field, we can now reference the management zones using Monaco references instead of IDs. The structure one of the following:

    ```yaml
    "{parameter}":
        type: reference
        project: "{project}"
        configType: "{configType}"
        configId: "{configId}"
        property: "{property}"
    ```

    Or - using the short form syntax:

    ```yaml
    "{parameter}": [ "{project}", "{configType}", "{configId}", "{property}" ]
    ```

    > **Note:** The curly brackets {} and their contents must be replaced with real values, e.g.: `mzId: ["project-1", "management-zone", "main", "id"]`.

    Where the fields mean the following:
    * `project`: The name of the Monaco project, for us that would be `infrastructure`
    * `configType`: The type of configuration you want to reference, for us that would be `management-zone`
    * `configId`: The name of the configuration instance (**NOT the name of the configuration as it is known in Dynatrace**), for us that would be `management-zone-k8s-nodes`
    * `property`: Which value (name or id) of the target configuration you want to use, for us that would be `id`.

    > **Note:** We will use the short form syntax for the remainder of the training. Feel free to experiment with both options!

    Putting this all together, this would give us:

     ```yaml
    mzId: [ "infrastructure", "management-zone", "management-zone-k8s-nodes", "id" ]
    ```

4. With the information above, we can now edit `infrastructure/_config.yaml` to make it look like the snippet below

    ```yaml
    ---
    configs:
      # management-zone 
      - id: management-zone-k8s-nodes
        config:
          name: k8s-nodes
          template: management-zone.json
          skip: false
        type:
          api: management-zone

      # alerting-profile
      - id: alerting-profile-k8s-nodes
        config:
          name: profile-k8s-nodes
          parameters:
            mzId: [ "infrastructure", "management-zone", "management-zone-k8s-nodes", "id" ]
          template: alerting-profile.json
          skip: false
        type:
          api: alerting-profile
    ```

5. Save the changes

### Step 4 - Run Monaco

1. First run it with a dry-run option

    ```bash
      monaco deploy manifest.yaml --dry-run
    ```

2. If there is no validation failure, continue with the real deployment.

    ```bash
      monaco deploy manifest.yaml
    ```

### Step 5 - View results in Dynatrace

1. Confirm in Dynatrace, that you do indeed have all the configurations and that they are correctly linked to each other in this way:

    `alerting-profile` > `management-zone`

    Alerting profile `profile-k8s-nodes` should now be linked to management zone `k8s-nodes`

**Congratulations!** you have completed the first part of this exercise.

---

## Exercise 5: Linking configurations - Part 2

Now you need to complete the linking for the `apps` project.

Go to `exercise-05/manifest.yaml` and edit the file. Uncomment the `apps` project and save your changes. Ensure that the indentation is correct. The file should look like this:

```yaml
manifestVersion: "1.0"

projects:
  - name: apps
    type: grouping
    path: apps
  - name: infrastructure
    path: infrastructure

environmentGroups:
  - name: default
    environments:
      - name: development-environment
        url:
          type: environment
          value: DT_TENANT_URL
        token:
          name: DT_API_TOKEN
```

Go ahead and link the configurations for `notification` > `alerting-profile` > `management-zone` for the `apps` project.

To help you, the following references need to be created:

* Alerting profile `alerting-profile-app-one` must reference the management zone `management-zone-app-one`.

* Notification `email-team-app-one` must reference the alerting profile `alerting-profile-app-one`.

* Notification `email-team-app-one-k8s-nodes` must reference the alerting profile in the `infrastructure` project.

Once you have made your changes push your changes to Dynatrace by triggering the monaco command.

```bash
  monaco deploy manifest.yaml
```
---

### Exercise 5: Linking configurations - Part 2 - Solution

We will walk through the changes required to link `notification` > `alerting-profile` > `management-zone` for the `apps` project.

1. Open `exercise-05/apps/app-one/_config.yaml`.

    The required references are:

    ```yaml
    mzId: [ "apps.app-one", "management-zone", "management-zone-app-one", "id" ]
    ```

    ```yaml
    alertingProfile: [ "apps.app-one", "alerting-profile", "alerting-profile-app-one", "id" ]
    ```

    ```yaml
    alertingProfile: ["infrastructure", "alerting-profile", "alerting-profile-k8s-nodes", "id" ]
    ```

    The file should look like this:

    ```yaml
    configs:
    - id: management-zone-app-one
        config:
          name: app-one
          parameters:
            environment: app-one
          template: ../shared/management-zone.json
          skip: false
        type:
          api: management-zone
    - id: alerting-profile-app-one
        config:
          name: profile-app-one
          parameters:
            mzId: [ "apps.app-one", "management-zone", "management-zone-app-one", "id" ]
          template: ../shared/alerting-profile.json
          skip: false
        type:
          api: alerting-profile
    - id: email-team-app-one
        config:
          name: email-team-app-one
          parameters:
            alertingProfile: ["alerting-profile", "alerting-profile-app-one", "id" ]
            recipient: team-app-one@example.com
          template: ../shared/notification.json
          skip: false
        type:
          api: notification
    - id: email-team-app-one-k8s-nodes
        config:
          name: email-team-app-one-k8s-nodes
          parameters:
            alertingProfile: ["infrastructure", "alerting-profile", "alerting-profile-k8s-nodes", "id" ]
            recipient: team-app-one-k8s@example.com
          template: ../shared/notification.json
          skip: false
        type:
          api: notification
    ```

    >**Note:** The mentioned project `apps.app-one` is of the type _grouping projects_ (as opposed to _simple projects_). That's why a `.` separator is used. You can leave out the project altogether (like halfway in the snippet above), in which case Monaco will default to the local project, in this case `apps.app-one`.

2. Run Monaco

   ```bash
    monaco deploy manifest.yaml
   ```

3. Confirm in Dynatrace, that you do indeed have all the configurations and that they are correctly linked to each other in this way:

    `notification` > `alerting-profile` > `management-zone`

### This concludes the Exercise 5 and the training!
