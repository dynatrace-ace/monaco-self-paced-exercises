## Overview

Applying manual changes to monitoring configuration is outdated as it takes a hefty toll on manageability and slows down innovation.

In this self-paced training, you will learn how to:
- Use the Dynatrace Monitoring-as-Code toolset to configure Dynatrace across different environments
- Create reusable and scalable configuration templates
- Backup and rollback your Dynatrace environment configuration
- Accelerate application onboarding with self-service monitoring

## Exercises and Exercise Samples

Training guides as well as exercise samples to download can be found in the below links:

- [Exercises](exercise-instructions/content)
     - [00 An Introduction](exercise-instructions/content/00%20An%20Introduction/index.md) 
     - [00 Install Monaco and Explore](exercise-instructions/content/00%20Install%20Monaco%20and%20Explore/index.md)    
     - [01 Configure Monaco](exercise-instructions/content/01%20Configure%20Monaco/index.md)
     - [02 Download all configuration](exercise-instructions/content/02%20Download%20all%20configuration/index.md)
     - [03 Configuration Variables](exercise-instructions/content/03%20Configuration%20Variables/index.md)
     - [04 Delete and Restore configuration](exercise-instructions/content/04%20Delete%20and%20Restore%20configuration/index.md)
     - [05 Linking configurations](exercise-instructions/content/05%20Linking%20configurations/index.md)
     - [06 Cleanup](exercise-instructions/content/06%20Cleanup/index.md)
     - [07 Learn More](exercise-instructions/content/07%20Learn%20More/index.md)
- [Exercise Samples to Download](exercise-samples-to-download)
    - [exercise-03-to-download](exercise-samples-to-download/exercise-03-to-download)
    - [exercise-05-to-download](exercise-samples-to-download/exercise-05-to-download)

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
