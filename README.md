# Azure Disaster Recovery between avaialbility zones in a single region based on ASR-zone-to-zone with Proximity Placement Groups

This repository provides a code example how to implement an **Azure zone to zone** Disaster Recovery solution. This example shows a failover and failback of a virtual machine between 2 availability zones in a single region including the use of Proximity Placement Groups (PPG) so the Virtual Machine also failovers to another PPG and failbacks to its original PPG. The Virtual Machine uses a static ip address to show that the virtual machines keeps it's own ip address during the failover and failback and therefore it's also a good scenario for applications that cannot handle an ip address change during a DR situation.

This repository provides a couple of Powershell scripts:
1. a script that creates the test environment as a starting point (ASR_CreateTestEnvironment.ps1)
2. a script to configure the ASR protection of the virtual machine to another availability zone (ASR_replicate.ps1)
3. a script to initiate a failover (ASR_failover.ps1)
4. a script to re-protect the virtual machine so it replicates back to the original VM (ASR_re-protect.ps1)
5. and a script failback the virtual machine to its original zone and ppg (ASR_failback.ps1) 

All scripts are Powershell based because PPG is only supported through Powershell currently (July 2020). The re-protection script is using the ASR REST API because there is a bug (July 2020) in the Powershell cmdlet to configure the re-protection to the other zone correctly.  

**Notes**
1. Powershell cmdlets are changing over time. It could be that some of the commands will fail over these changes.
2. Check if the VM you want to protect had resource locks enabled. If the source VM has resource locks remove these first before running the scripts. Otherwise the scripts will fail. 
3. Be carefull using the latest version of an marketplace image because the ASR agent doesn't support always the latest images. See https://docs.microsoft.com/en-us/azure/site-recovery/azure-to-azure-support-matrix to check if your image version is supported 

![Picture of test setup](/images/ASR_zone_to_zone.png)
