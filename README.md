# How to Interact and Manage New Pools Created by the UniswapV3Factory Contract Using Tenderly Alerts and Web3 Actions

This guide walks you through how to manage and interact with new Pools using a Tenderly alerts and Web3Actions on Ethereum Mainnet. We'll also cover how to set up a Web3 Action with Tenderly Alerts using Tenderly's CLI to make this possible.

## Prerequisites

- A Tenderly account
- Access to the Tenderly dashboard
- A project within Tenderly
- Tenderly CLI installed on your machine

## Intro

The alert you create will serve as the trigger for your Web3 Action. Whenever the specified event (PoolCreated) is emitted by the UniswapV3Factory contract, Tenderly will detect it through the alert system. This alert then automatically triggers the execution of your Web3 Action, allowing you to respond to new pool creations in real-time without manual intervention.
This setup provides a powerful way to automate interactions with the Uniswap V3 ecosystem, enabling you to stay updated on new pool creations and perform custom actions as needed.

## Step 1: Creating an Alert in Tenderly

1. Log into your Tenderly Dashboard
2. Navigate to the project where you want to set the alert
3. Go to the **Alerts** tab and click on **New Alert**
4. Set the Alert Type to **Event Emitted**
5. Configure the alert:
   - **Contract Address:** `0x1f98431c8ad98523631ae4a59f267346ea31f984` (UniswapV3Factory contract)
   - **Event Signature:** `PoolCreated(address,indexed address,uint24)`
   - Name your alert appropriately for easy reference
6. Save the alert and note down the `<ALERT_ID>` for later use

## Step 2: Setting Up Your Web3 Action

Navigate to the folder on your system where you want to place your Web3 Action code and the following command to initialize a new Web3 Actions project
```bash
tenderly actions init
````
By default, the project will be configured to use TypeScript. If you prefer plain JavaScript, add the language flag and specify javascript:
```bash
tenderly actions init --language javascript
```
When prompted, select an existing Tenderly project from your account or create a new one.

Next, input the name of the root directory where your code will be stored. If you want to leave the default actions, press Enter, or input a custom directory name.

Newly created `tenderly.yaml` file modify with the following configuration:

```yaml
account_id: "<YOUR_ACCOUNT_ID>"
actions:
  <YOUR_ACCOUNT_ID>/<YOUR_PROJECT_SLUG>:
    runtime: v2
    sources: actions
    specs:
      uniswapV3:
        description: "Monitoring UniswapV3 Factory Contract and adding Child/Pool to Contracts page with proper tag"
        function: example:actionFn
        trigger:
          type: alert
          alert: { <ALERT_ID> }
        execution_type: parallel
project_slug: "<YOUR_PROJECT_SLUG>"
```
Replace the placeholders:

`<YOUR_ACCOUNT_ID>`: Your Tenderly account ID

`<YOUR_PROJECT_SLUG>`: Your Tenderly project slug

`<ALERT_ID>`: The ID of the alert you created in Step 1

## Step 3: Adding the ACCESS-KEY to Secrets

- Navigate to the Web3 Actions page on your Tenderly dashboard
- Go to the `Secrets` tab
- Add a new secret with the key `ACCESS-KEY` and the value of your actual access key

## Step 4: Deploying Web3 Action

- Open a terminal and navigate to your project directory
- Run the following command to deploy your actions:
```bash
tenderly actions deploy
```

This command deploys your Web3 Action as configured in the `tenderly.yaml` file.

## Step 5: Monitoring

This Web3 Action will automatically trigger whenever the `PoolCreated` event is emitted by the UniswapV3Factory contract. The action will execute the actionFn function defined in your code, and will perform the following:

- Logging details about the newly created Pool
- Adding the new Pool contract to your Tenderly project
- Tagging the new Pool contract for easier management
- Performing additional analysis or actions based on the pool creation
