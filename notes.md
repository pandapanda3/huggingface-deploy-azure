To view existing environments, use the following Azure CLI command:
```
az containerapp env list --resource-group demo-ml --query "[].name" -o table
```

Here, `demo-ml` is the name of your resource group.

### Check Environment Status

To check the status of the environment, use the following command:

```bash
az containerapp env show --name managedEnvironment-demoml-a2cc --resource-group demo-ml --query "properties.provisioningState"
```

#### If the Environment Status is Failed or Configuration is Incomplete
If the environment status shows Failed or the configuration has not completed, delete the environment:
```
az containerapp env delete --name managedEnvironment-demoml-a2cc --resource-group demo-ml --yes
```
#### Recreate the Environment

```
az containerapp env create \
  --name managedEnvironment-demoml-a2cc \
  --resource-group demo-ml \
  --location "UK South"
```

# Create  Container App
```
az containerapp create \
  --name azure-container-app-demo \
  --resource-group demo-ml \
  --environment managedEnvironment-demoml-a2cc \
  --image ghcr.io/pandapanda3/huggingface-deploy-azure:6f087fa939c498066062a1ebf6de4eabb93f233f \
  --ingress external \
  --target-port 80
```

# To get the PAT
The access token will need to be added as an Action secret. Create one with enough permissions to write to packages:  
[Create a new token with write access to packages](https://github.com/settings/tokens/new?description=Azure+Container+Apps+access&scopes=write:packages)

#  list container apps in the resource group and see the Azure Container App name:
az containerapp list --resource-group demo-ml --query "[].name" -o table

```
az containerapp list --resource-group ${{ env.AZURE_GROUP_NAME }} --query "[].name" -o table
```

 ${{ env.AZURE_GROUP_NAME }}: resource group name