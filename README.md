# ESI-OpenFoam-in-Azure
How to install and run OpenFOAM in Azure 



# Task 2
Download Source or Binaries
a) Linux Source
https://www.openfoam.com/download/install-source.php
b) Linux Binaries
https://www.openfoam.com/download/install-binary-linux.php

# Task 3 (for 2a)

Create VM
F64v2
CentOS HPC 7.4


Version 1806 supported by ESI

F64v2 with Skylake Platinum

This VM is available since October 
https://azure.microsoft.com/en-us/blog/fv2-vms-are-now-available-the-fastest-vms-on-azure/

Please visit the requirements page
https://www.openfoam.com/documentation/system-requirements.php

Usually you should do the following on your VM

sudo yum -y update
sudo yum -y install centos-release-scl
sudo yum -y install devtoolset-4-gcc*
scl enable devtoolset-4 bash  (gcc 5.3.1)
sudo yum -y install boost qt fftw
export LD_PRELOAD="libmpi.so" 

# Task 3b (for 2b)

Create Service Principal

az ad sp create-for-rbac --name dockeree

you will get

thomas@DE-LOANER514:~$ az ad sp create-for-rbac --name dockeree
Retrying role assignment creation: 1/36
Retrying role assignment creation: 2/36
{
  "appId": "d.......................xx",
  "displayName": "dockeree",
  "name": "http://dockeree",
  "password": "2xxxxxxxxxxxxxxxxxxxxxxx6",
  "tenant": "7xxxxxxxxxxxxxxxxxxxxxxxxxxxx7"
}
thomas@DE-LOANER514:~$

You will need these details for creating the VM.

Create VM with Docker EE
Docker EE for Azure (Basic) - [17.06] from Marketplace

Select the type and number of workers














Build Binaries from Source

```
mkdir $HOME/OpenFOAM 

mv ThirdParty-v1806 OpenFOAM-v1806 $HOME/OpenFOAM

tar -xzf OpenFOAM-v1806.tgz 
tar -xzf ThirdParty-v1806.tgz 

source /opt/intel/impi/5.1.3.223/intel64/bin/mpivars.sh
```
Follow the Build Guide 
https://www.openfoam.com/code/build-guide.php, e.g. 
edit etc/bashrc and select INTELMPI for WM_MPLIB
```
export WM_MPLIB=INTELMPI

```

# Task 3 




