# ESI-OpenFoam-in-Azure
How to install and run OpenFOAM in Azure 


Version 1806 supported by ESI

F64v2 with Skylake Platinum

This VM is available since October 
https://azure.microsoft.com/en-us/blog/fv2-vms-are-now-available-the-fastest-vms-on-azure/

# Task 1
Download Source or Binaries
a) Linux Source
https://www.openfoam.com/download/install-source.php
b) Linux Binaries
https://www.openfoam.com/download/install-binary-linux.php

# Task 2 (for a)
Build Binaries from Source

mkdir $HOME/OpenFOAM 

mv ThirdParty-v1806 OpenFOAM-v1806 $HOME/OpenFOAM

tar -xzf OpenFOAM-v1806.tgz 
tar -xzf ThirdParty-v1806.tgz 



# Task 3 




