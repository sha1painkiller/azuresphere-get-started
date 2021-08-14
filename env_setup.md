# azuresphere-device-bringup
The demonstration here is using Powershell as the CLI command issuer.
## Set up environment
1. following the [instruction](https://docs.microsoft.com/en-us/azure-sphere/install/install-sdk?pivots=visual-studio) to install Azure Sphere SDK.
1. launch Powershell from Windows
1. after seeing the command prompt, import CLI autocomplete module
    - > Import-Module -name AzsphereCli
1. connect Azure Sphere enabled device (through USB) to PC and chech the connectivity on CLI
    - > azsphere device list-attached
    - you should see similar messages below (ID is unique per device)
    - 1 device attached:
--> Device ID: 488FXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX22E6
  --> Is responsive: yes
  --> IP address: 192.168.35.2
  --> Connection path: 313
1. Do a system recovery to update the device to the latest OS release
    - > azsphere device recover
    - this action will typically take ~2 minutes

## Claim device
1. On the Powershell CLI, sign in to Azure sphere using a Microsoft Account
    - > azsphere login
    - If you've never logged in to Azure Sphere before, register a user with the Azure Sphere Security Service
    - > azsphere register-user --new-user \<email-address>
2. >  azsphere tenant list
    - to choose an existing tenant
    - > azsphere tenant select -t \<tenant ID or tenant name>
3. If you cannot find a tenant in previosu step, create one
    - > azsphere tenant create --name \<tenant-name> --force
4. Claim the device to the selected or created tenant
    - > azsphere device claim
    - you will see similar messages below
    - Claiming device.
Successfully claimed device ID '\<device ID>' into tenant '\<name>' with ID 'd343c263-xxxx-xxxx-xxxx-d3fc34631800'.

## Dvelopment and debug mode
1. for local development and debug, put the device in development mode so it can accept un-signed images
    - > azsphere device enable-development

## Create a cloud deployment (production mode)
1. Upload an image to AS3
    - > azsphere image add --image \<image-ID>
3. Create a product for the device
    - > azsphere product create --name \<product name> --description "\<product description>"
4. Enable cloud based deployment for your device
    - > azsphere device enable-cloud-test --product \<product name>
5. List current available device groups
    - > azsphere device-group list
6. Create a new deployment for a device group
    - > azsphere device-group deployment create --device-group \<device-group-ID> --images \<image-ID>
    - or
    - > azsphere device-group deployment create --device-group '\<product-name>/\<device-group-name>' --images \<image-ID>
7. Trigger the deployment
    - To trigger the download immediately, press the <span style="color:red">**RESET**</span> button on (or power reset) the Azure Sphere device, Or the image will happen when next device attestation occurs (within 24 hours).
    - To verify that the application was installed on your device, use the
        - > azsphere device image list-installed
8. Reenable development and debugging
    - > azsphere device enable-development

