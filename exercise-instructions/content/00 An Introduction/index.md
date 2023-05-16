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
   DataExport, ReadConfig , WriteConfig , DataPrivacy , CaptureRequestData , slo.read , slo.write , ExternalSyntheticIntegration , settings.read , settings.write
   ```
4. Select `Generate token` and copy the generated token to a notepad for future use. 
