# Choosing the best performance counters to activate on Azure Windows VMs

This guide follows the recomendations regarding the performance counters to collect for Azure VMs running [SQL Server](https://docs.microsoft.com/en-us/azure/azure-sql/virtual-machines/windows/performance-guidelines-best-practices-collect-baseline?view=azuresql). This same settings can also be applied to Azure VMs not running SQL Server.

## Step 1. Review and customize Diagnostic configuration (only if needed)

You can review the file `pub-settings.json`and add or remove performance counters if needed. The provided configuration already includes counters addressing 5 areas:
- Compute (cpu)
- Memory
- Disks
- Networking
- SQL Server

## Step 2. Create a Storage account to be used

Collecting performance counters requires a storage account to be available.
Create a new one or reuse an existing one. You need to get these 2 configuration parameters to use later on:
- Storage account name
- Storage account key

## Step 3. Login to Azure in your Powershell session

```
Connect-AzAccount -UseDeviceAuthentication
```

## Step 4. Apply the diagnostic settings to your VMs

Customize to your environment:

```
$vm_resourcegroup = "your resource group where VMs are"
$vm_name = "your VM name"
$diagnosticsconfig_path = "pub-settings.json"
$diagnosticsstorage_name="your storage account name"
$diagnosticsstorage_key="your storage account key"

Set-AzVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name -DiagnosticsConfigurationPath $diagnosticsconfig_path -StorageAccountName $diagnosticsstorage_name -StorageAccountKey $diagnosticsstorage_key
```

## If you need to collect settings for an existing Azure VM already configured

```
Get-AzVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name

$publicsettings = (Get-AzVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name).PublicSettings

$publicsettings | out-file -filepath collected-pub-settings.json
```
