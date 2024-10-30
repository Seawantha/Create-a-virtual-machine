# Create-a-virtual-machine- PowerShell

<!-- Define variables for the resource group, VM name, location, and image type. -->

$RESOURCE_GROUP = "myResourceGroup"
$VM_NAME = "myVM"
$LOCATION = "eastus"            # Choose your preferred location
$IMAGE = "UbuntuLTS"             # This example uses an Ubuntu image; choose another if desired

<!-- You need a resource group to hold your VM and other associated resources. -->

az group create --name $RESOURCE_GROUP --location $LOCATION

<!-- Use the az vm create command to create the VM, which will set up the network and other necessary resources. -->

az vm create `
  --resource-group $RESOURCE_GROUP `
  --name $VM_NAME `
  --image $IMAGE `
  --admin-username azureuser `
  --generate-ssh-keys

<!-- By default, SSH (port 22) is open for Linux VMs. To open additional ports (e.g., for HTTP or other services), use: -->

az vm open-port --port 80 --resource-group $RESOURCE_GROUP --name $VM_NAME

<!-- To connect to the VM, get its public IP address with this command: -->

$publicIp = az vm show -d -g $RESOURCE_GROUP -n $VM_NAME --query publicIps -o tsv
Write-Output "Public IP Address: $publicIp"

<!-- Use SSH to connect to the VM. Run this in a local terminal (outside of Azure Cloud Shell) if needed: -->

ssh azureuser@$publicIp

