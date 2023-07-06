# Check you still have ui container image from earlier task
docker images

# Connect to Azure
az login

# Login to ACR
az acr login --name acr17708

# Get ACR server address
az acr list --resource-group labs-bverma-use-RG --query "[].{acrLoginServer:loginServer}" --output table

# Tag the local copy of the UI Container Image
docker tag ui:v1.0 acr17708.azurecr.io/ui:v1.0

# Confirm new UI image with acr tag exist
docker images | grep ui -w

# Push image to ACR
docker push acr17708.azurecr.io/ui:v1.0

# Verify the image is in ACR
az acr repository list --name acr17708 --output table

# Connect to AKS
az aks get-credentials --resource-group labs-bverma-use-RG --name aks-cluster-01

# Verify that you are connected
kubectl get nodes

# Apply UI Manifest to the AKS cluster
kubectl apply -f demo-ui.yaml

# Verify pod is deployed for UI
kubectl get pods -n ns-posthub--env

# See every related pod/service/deployment etc in this namespace
kubectl get all -n ns-posthub--env

# Describe a UI pod
kubectl describe pod ui--env-75bbf666b6-x5wsk -n ns-posthub--env

