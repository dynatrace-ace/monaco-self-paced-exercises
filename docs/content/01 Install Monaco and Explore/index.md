## Install Dynatrace Configuration as Code CLI

**Important Note:** Installation steps vary depending on your operating system. This guide provides installation instructions for Linux 64-bit systems. For MacOS and Windows,    please refer to the [installation guide](https://www.dynatrace.com/support/help/manage/configuration-as-code/installation).
   
1. Download the latest version of the Dynatrace Configuration as Code CLI tool

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
5. Execute the command below to ensure Monaco is properly installed on your VM.

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

6. From the `Environments` tab, use the `View environment` button to open up your Dynatrace environment in a new window and sign in with the provided credentials.

### We're now ready to kick off the lab!
