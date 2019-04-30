# Reading BIOS Information of a Hyper-V Machine

A few weeks ago I had to find the BIOS serial number and the GUID of a Hyper-V
machine. I quickly found a PowerShell query which would print me the desired
information:

```powershell
Get-WmiObject -ComputerName 'localhost' -Namespace 'root\virtualization\v2' -Class 'Msvm_VirtualSystemSettingData' |
    Where-Object { $_.BIOSSerialNumber } |
    Select-Object ElementName, BIOSSerialNumber, BIOSGUID
```

But running this command in PowerShell gave me an empty result. It took me
several minutes to figure out that I had to run the query in an
**Administrator** PowerShell session.
