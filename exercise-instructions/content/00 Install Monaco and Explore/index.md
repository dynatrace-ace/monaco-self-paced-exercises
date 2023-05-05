## Overview

Applying manual changes to monitoring configuration is outdated as it takes a hefty toll on manageability and slows down innovation.

In this self-paced training, you will learn how to:
- Use the Dynatrace Monitoring-as-Code toolset to configure Dynatrace across different environments
- Create reusable and scalable configuration templates
- Backup and rollback your Dynatrace environment configuration
- Accelerate application onboarding with self-service monitoring

## Prerequisites
- Dynatrace environment (a.k.a. Dynatrace tenant) - SaaS or Managed
- Basic knowledge of configuring Dynatrace

## Prepare your Dynatrace environment

1. Access to your Dynatrace environment, in the Dynatrace menu, select `Access tokens`
2. Select `Generate new token`. Enter a name for your token : 
    ```text
    Monaco-API-Token
    ```
3. Select the following scopes: 
   ```text
   DataExport, ReadConfig , WriteConfig , DataPrivacy , CaptureRequestData , slo.read , slo.write , , ExternalSyntheticIntegration , settings.read , settings.write
   ```
4. Select `Generate token` and copy the generated token to a notepad for future use. 

## Install Dynatrace Configuration as Code CLI

**Important Note:** Installation steps vary depending on your operating system. This guide provides installation instructions for **Linux 64-bit** systems. For MacOS and Windows, please refer to the [installation guide](https://www.dynatrace.com/support/help/manage/configuration-as-code/installation). All the exercises in this guide will include the terminal commands that are based on **Linux 64-bit** systems.
   
1. Open up your terminal and download the latest version of the Dynatrace Configuration as Code CLI tool.

    ```bash
    curl -L https://github.com/Dynatrace/dynatrace-configuration-as-code/releases/latest/download/monaco-linux-amd64 -o monaco-linux-amd64
    ```

2. (Optional) Verify the downloaded binary using the checksum.
    
    To verify that the downloaded binary is valid, download its checksum file:
    ```bash
    curl -L https://github.com/Dynatrace/dynatrace-configuration-as-code/releases/latest/download/monaco-linux-amd64.sha256 -o monaco.sha256
    ```
    
    Verify the downloaded binary using the checksum:
    ```bash
    shasum -c monaco.sha256
    ```
    You should see the following output:
    `monaco: OK`
    
3. Rename the specific executable to monaco
   ```bash
   mv monaco-linux-amd64 monaco
   ```
4. Make the binary executable
   ```bash
   chmod +x monaco
   ```
5. Install Dynatrace Configuration as Code CLI to a central location in your PATH.
   ```bash
   sudo mv monaco /usr/local/bin/
   ```

6. Execute the command below to ensure Monaco is properly installed on your machine.

    ```bash
    monaco
    ```

    The expected output for this command will be the Monaco help page that explains usage and command options.

    ```text
    Tool used to deploy dynatrace configurations via the cli

    Examples:
    Deploy configuration defined in a manifest
        monaco deploy service.yaml
    Deploy a specific environment within an manifest
        monaco deploy service.yaml -e dev

    Usage:
    monaco <command> [flags]
    monaco [command]

    Available Commands:
    completion  Generate the autocompletion script for the specified shell
    convert     Convert v1 monaco configuration into v2 format
    delete      Delete configurations defined in delete.yaml from the environments defined in the manifest
    deploy      Deploy configurations to Dynatrace environments
    download    Download configuration from Dynatrace
    help        Help about any command
    version     Prints out the version of the monaco cli

    Flags:
    -h, --help      help for monaco
    -v, --verbose   Enable debug logging

    Use "monaco [command] --help" for more information about a command.
    ```
