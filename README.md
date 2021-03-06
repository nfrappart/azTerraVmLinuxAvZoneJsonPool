# Linux VMs Module
This module creates multiple VM, with data disks and dependant resources
- 1 Resource Group
- n Linux Virtual Machines
- VM Extensions (azure ad auth)
- 1 NIC per VM
- n Managed Disks per VM
- Keyvault secrets for primary sudo user Password (to be stored in Existing Core Keyvault)
- 1 storage Account for all VMs boot diags


## Required resources :
- existing Keyvault
- existing storage account for diag
- existing Vnet (to specify as input variable)
- existing subnet (to specify in the vm configurations in the json)

## Required file :
- vm.json : containing all VMs and their specs

example:
```json
{
    "myVm":{
        "size":"Standard_D2s_v3",
        "vmAdminName":"localadm",
        "publisher":"Canonical",
        "offer":"0001-com-ubuntu-server-focal",
        "sku":"20_04-lts-gen2",
		"version":"latest",
        "subnet":"mySubnet",
        "zone":"1",
        "osDiskSize":"64",
        "disks":[
            {
            "lunId":"1",
            "size":"128"
            },
            {
            "lunId":"2",
            "size":"128"
            }
        ]
    }
}
```


## Usage Example :

```hcl

module "vm" {
  source = "github.com/nfrappart/azTerraVmLinuxAvZoneJsonPool?ref=v1.0.4"
  configFileName = "vm.json"
  rgName = "test"
  keyVault   = "myKv" # reference existing key vault
  keyVaultRg = "rgMyKv" # reference keyvault resource group name
  vmDiagSta = "mystorageaccount" # reference existing storage account
  rgVmDiagSta = "rgStorageAccount" # reference strage account resource group name 
  vnetName = "myVnet"
  vnetRg = "rgMyNetwork"

}
```
