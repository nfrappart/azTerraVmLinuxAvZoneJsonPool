# Linux VMs Based Application Module
This module creates multiple VM, with data disks and dependant resources
- 1 Resource Group
- n Linux Virtual Machines
- VM Extensions (ansible required ssh config)
- 1 NIC per VM
- n Managed Disks per VM
- Keyvault secrets for primary sudo user Password (to be stored in Existing Core Keyvault)
- 1 storage Account for all VMs boot diags


## Required resources :
- existing Keyvault

## Required file :
- vm.json : containing all VMs and their specs

example:
```json
{
    "myVm":{
        "size":"Standard_D2s_v3",
        "vmAdminName":"root-dsi",
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
  source = "https://github.com/nfrappart/azTerraVmLinuxAvZoneJsonPool?ref=v1.0.0""
  configFileName = "vm.json"
  rgName = "test"
  env = "prod"
  keyVaultName   = "myKv"
  keyVaultRg = "rgMyKv"
  vmDiagSta = "mystorageaccount"
  rgVmDiagSta = "rgStorageAccount" 
  vnetName = "myVnet"
  vnetRg = "rgMyNetwork"

}
```