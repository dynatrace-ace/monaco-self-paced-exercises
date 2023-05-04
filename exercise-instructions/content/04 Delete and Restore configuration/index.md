## Exercise 4: Delete and restore configuration
In this exercise, we'll use Monaco to delete a specific configuration. Upon deletion, we want to restore all configs from the backup created earlier.

Configurations which aren't needed anymore can also be deleted in an automated fashion. The `delete` command is a convenient way to remove one or more configurations from one or more Dynatrace environments. For example, Monaco can be used to clean up ephemeral configurations in development environments.

The `delete` command takes two arguments as YAML files. A manifest file which contains the list of Dynatrace environments and a delete file where configurations defined are to be removed.

If you don't specify file names, the command tries to find a `manifest.yaml` and a `delete.yaml` file in the current folder.

 ```bash
monaco delete <manifest.yaml> <delete.yaml> [flags]
```

The content of a delete.yaml is as follows:

```yaml
delete:
  - <api/name> OR <schema/config-id>
  - …
```

To delete configs, use <api>/<name> (for example, java-service/My Config Name)
To delete settings objects, use <schema/config-id> (for example, builtin:anomaly-detection.service/my-service-id)

Here's one example of `delete.yaml` with multiple configurations:

```yaml
delete:
  - "auto-tag/app-one"
  - "auto-tag/app-two"
  - "management-zone/app-one"
  - "calculated-metrics-service/simplenode.staging"
```

### Step 1 - Verify existence of target object

Since we'll be deleting the auto tagging rule created in exercise-1, let's make sure the tag still exists.

1. Open the Dynatrace web UI and navigate to `Manage` > `Settings`

2. Open `Tags` > `Automatically applied tags`

3. Confirm that tag `Owner` still exist

    ![Owner tag](../../assets/images/05_owner_tag_ui.png)

### Step 2 - Prepare the delete file

1. Create a new directory for this exercise and copy the contents of the manifest file from Exercise-3.
   Please make sure to locate the correct directory of exercise-3. But don´t worry, below is the content of the manifest file that you can copy and paste.
 
   ```yaml
    ---
    manifestVersion: "1.0"

    projects:
      - name: apps
        path: apps
        type: grouping
      - name: infrastructure
        path: infrastructure

    environmentGroups:
      - name: default
        environments:
          - name: development-environment
            url:
              type: environment
              value: DT_TENANT_URL
            auth:
              token:
                name: DT_API_TOKEN
   ```
  
   ```bash
    mkdir exercise-04
    cd exercise-04
    cp ../exercise-03/manifest.yaml .
   ```

2. Save the changes

3. Still in Gitea, edit file `dt-exercises/06_exercise_six/delete.yaml` to make it look like the snippet below

    ```yaml
    delete:
      - "auto-tag/Owner"
    ```

4. Commit the changes

### Step 3 - Pull changes and run Monaco

1. Open the SSH client that's connected to your VM and navigate into the directory of this exercise

    ```bash
    cd ~/06_exercise_six
    ```

2. Execute the following command to pull down changes made in Gitea

    ```bash
    git pull
    ```

    > **Note:** In case you experience an issue with your git upstream, you can run the following command: `git pull --set-upstream gitea main`

    Confirm that both `manifest.yaml` and `delete.yaml` are pulled into the current directory

    ```text
    From http://****:3000/dt-exercises/06_exercise_six
      dfc521b..dfbbfc3  main       -> gitea/main
    Updating dfc521b..dfbbfc3
    Fast-forward
      delete.yaml   |  3 ++-
      manifest.yaml | 19 ++++++++++++++++++-
      2 files changed, 20 insertions(+), 2 deletions(-)
    ```

3. Verify that the environment variable `DT_API_TOKEN` still exists

    ```bash
    echo $DT_API_TOKEN
    ```

    If not, recreate it from the Kubernetes secret

    ```bash
    export DT_API_TOKEN=$(kubectl -n ace get secret monaco-dt-access-token -o jsonpath='{.data.apiToken}' | base64 -d)
    ```

4. Run Monaco

    ```bash
    monaco delete manifest.yaml delete.yaml
    ```

    Monaco should execute and you shouldn't see any errors

    ```text
    2022/12/21 01:28:41 INFO  Deleting configs for environment `development-environment`
    2022/12/21 01:28:41 INFO  Deleting configs of type auto-tag...
    ```

5. Confirm in your Dynatrace environment that the `Owner` tag doesn't exist anymore (refresh the page if it's already open)

### Step 4 - Restore configuration from backup

Now that we deleted configuration, we want to restore it from the backup we created earlier. In a real scenario this might not only be one configuration (e.g. auto-tag) but multiple configs across different projects and environments. The concept stays the same: If a configuraiton is backed up (= managed) by Monaco, Monaco can that very configuration.

In addition to a backup/restore use case, this workflow also comes in handy when you migrate between environments. Monaco doesn't care whether a configuration is deployed to the environment it was initially downloaded from, a completely separate environment or even multiple environments.

1. Navigate to and explore the directory of the backup we created earlier (exercise 3):

    ```bash
    cd ~/03_exercise_three
    ```

    Your directory should look similar to this:

    ```text
    │── manifest.yaml
    └── backup
        │── builtin:tags.auto-tagging
        │   │── config.yaml
        │   │── aaaaaaaa-bbbb-cccc-eeee-ffffffffffff.json
        │── ...
    ```

2. Verify your deployment with a dry run:

    ```bash
    monaco deploy manifest.yaml -d
    ```

3. And deploy your backup:

    ```bash
    monaco deploy manifest.yaml
    ```

4. Confirm in your Dynatrace environment that the `Owner` tag is restored (refresh the page if it's already open).

#### Congratulations on completing Exercise 6!
