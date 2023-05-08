## Cleanup

Before closing this self-paced training, let’s make sure we clean up all the resources we created.

Configurations which aren't needed anymore can also be deleted in an automated fashion via Monaco. The `delete` command is a convenient way to remove multiple configurations from one or more Dynatrace environments. Therefore let us utilize Monaco to clean up the Dynatrace environment.

### Step 1 - Prepare the manifest and delete files

1. Create a new directory for this exercise and create `manifest.yaml` and `delete.yaml` files on the same directory.
 
   ```bash
    mkdir cleanup
    cd cleanup
    touch manifest.yaml
    touch delete.yaml
   ```

   Copy the following content into the ´manifest.yaml´ file:
 
    ```yaml
    ---
    manifestVersion: "1.0"

    projects:
      - name: cleanup
        path: cleanup

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
 
2. Save the changes

3. Copy the following content into the `delete.yaml` file.
    
    ```yaml
    delete:
      # From Exercise 1
      - "auto-tag/Owner"
      # From Exercise 3 - infrastructure project
      - "auto-tag/environment-tag"
      - "auto-tag/app-tag"
      - "synthetic-location/synthetic-location-ace-box"
      - "request-attributes/ltn-request-attribute"
      - "request-attributes/tsn-request-attribute"
      - "request-attributes/lsn-request-attribute"
      # From Exercise 3 - apps.shared project
      - "calculated-metrics-service/simplenode.staging"
      # From Exercise 3 - apps.app-one project
      - "auto-tag/tagging-app-one-type-api"
      - "builtin:tags.auto-tagging/tagging-app-one-type-settings"
      - "application-web/application-app-one"
      - "app-detection-rule/application-detect-app-one"
      - "management-zone/management-zone-app-one"
      - "dashboard/dashboard-app-one"
      - "synthetic-monitor/health-app-one"
      # From Exercise 3 - apps.app-two project
      - "auto-tag/tagging-app-two"
      - "application-web/application-app-two"
      - "app-detection-rule/application-detect-app-two"
      - "management-zone/management-zone-app-two"
      - "dashboard/dashboard-app-two"
      - "synthetic-monitor/health-app-two"
      # From Exercise 5 - infrastructure project
      - "management-zone/management-zone-k8s-nodes"
      - "alerting-profile/alerting-profile-k8s-nodes"
      # From Exercise 5 - apps.app-one project
      - "management-zone/management-zone-app-one"
      - "alerting-profile/alerting-profile-app-one"
      - "notification/email-team-app-one"
      - "notification/email-team-app-one-k8s-nodes"
    ```
    
4. Save the changes. Confirm that both `manifest.yaml` and `delete.yaml` exist in the current directory

### Step 2 - Run Monaco

1. Verify that the environment variable `DT_API_TOKEN` still exists

    ```bash
    echo $DT_API_TOKEN
    ```

    If not, recreate it from the token you created in the previous exercises.

    ```bash
    export DT_API_TOKEN=PASTE-YOUR-API-TOKEN-HERE
    ```
2. Verify that the environment variable `DT_TENANT_URL` still exists

    ```bash
    echo $DT_TENANT_URL
    ```

    If not, recreate it with your Dynatrace environment URL. Include `https://` but ensure there is no trailing `/` at the end of the URL.

    ```bash
    export DT_TENANT_URL=PASTE_YOUR_TENANT_URL_HERE
    ```

3. Run Monaco

    ```bash
    monaco delete
    ```

    Monaco should execute and you shouldn't see any errors

4. Confirm in your Dynatrace environment that the configurations created during the exercises do not exist anymore.

