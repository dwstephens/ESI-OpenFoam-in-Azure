# OpenFoam+ -in-Azure
This lab describes a fast way how to install and run OpenFOAM-v1806 in Azure on a single VM. The method can also be applied for older versions. The list of versions is available on the ESI/OpenCFD Ltd website https://www.openfoam.com/code/repositories.php


## Task 1 - Create VM

Prepare a virtual machine in the Azure Portal. E.g. A8, A9 or H16r with CentOS7.4 HPC and connect to the machine.

## Option A Download the binaries for H16r with CentOS 7.4, unzip the file and start immediately with Task 4

wget https://bigcompbasestore.blob.core.windows.net/public/openfoam-v1806.zip

## Option B Build your own binaries

## Task 2 - Install basic packages and tools

The basic packages below are not mandatory for OpenFOAM, but as a good practice it is recommended to follow the steps below.

```
source /opt/intel/compilers_and_libraries/linux/bin/compilervars.sh -arch intel64 -platform linux
source /opt/intel/impi/2018.1.163/intel64/bin/mpivars.sh

yum -y update
#
# Allow sudo without password for all users in group wheel
#
sed -e 's|^%wheel|# %wheel|g' -i /etc/sudoers
echo -e "\n%wheel        ALL=(ALL)       NOPASSWD: ALL" >> /etc/sudoers
#
# Install some required repositories
#
if ! yum -y install epel-release > /dev/null 2>&1 ; then
        wget http://dl.fedoraproject.org/pub/epel/epel-release-latest-${major_version}.noarch.rpm
        yum -y localinstall epel-release-latest-${major_version}.noarch.rpm
fi
yum -y install joe multitail wget nmap nfs-utils cifs-utils xterm hwloc hwloc-gui bmon iftop iptraf-ng perf sysstat iperf iperf3 fping htop screen tmux pciutils tcsh most clustershell azure-cli hyperv-tools zlib-devel

yum -y install dapl-static dapl-devel libibverbs libibverbs-utils infiniband-diags rdma-core rdma-core-devel kernel-deve
l kernel-headers tcl-devel numactl numactl-devel numactl-libs

yum -y groupinstall "Development Tools" "X Window System"
```

Install Intel Compiler tar -xzf parallel_studio_xe_2018_update1_cluster_edition_online.tgz
An Evaluation License can be downloaded from http://registrationcenter-download.intel.com


## Task 3 - Install OpenFOAM v1806 on the VM


You can get the source directly from https://www.openfoam.com/releases/openfoam-v1806/ . This version (OpenFOAM plus) is maintained and supported by OpenCFD limited. 
Or you can download from Azure Blob Storage :
```
wget https://hpccenth2lts.blob.core.windows.net/openfoam/OpenFOAM-v1806.tgz
wget  https://hpccenth2lts.blob.core.windows.net/openfoam/ThirdParty-v1806.tgz

mkdir OpenFOAM
cd OpenFOAM

change MPI Version in etc/bashrc to INTELMPI and Icc

export WM_NCOMPPROCS=16
source ~/OpenFOAM/OpenFOAM-v1806/etc/bashrc 
```
Check the readiness of the installation

```
foamSystemCheck 
foam

export WM_NCOMPPROCS=12
./Allwmake

cd ~/OpenFOAM-v1712/tutorials/incompressible/simpleFoam/motorBike

```
At the end there should be this output on the screen

```
2018-07-23 12:41:08 +0000
========================================
  Finished compile of cfMesh with OpenFOAM-v1806
  Icc system compiler
  linux64IccDPInt32Opt, with INTELMPI mpi


2018-07-23 12:41:08 +0000
========================================
  OpenFOAM-v1806
  Icc system compiler
  linux64IccDPInt32Opt, with INTELMPI mpi

12:41:08 [~/OpenFOAM/OpenFOAM-v1806]
```
## Task 4 - Run the motobike case from the tutorial

Run the motoBike case from the tutorial. Default is running with 6 cores. For different core counts one has to modify the system/decomposeParDict.x file. In the Allrun file line 6 has to replaced by pointing to this file.

```
cd ~/OpenFOAM/OpenFOAM-v1806/tutorials/incompressible/simpleFoam/motorBike

```
Example for a run with 16 cores.

```
numberOfSubdomains 16;

method          hierarchical;
// method          ptscotch;

coeffs
{
    n           (8 2 1);
```
For a full list of other entries to the file please look here for more details https://cfd.direct/openfoam/user-guide/v6-running-applications-parallel/

```    
./Allclean
./Allrun
```

Check the file log.simpleFoam.

If you run the MotorBike simpleFoam example on 4, 8 and 16 cores , you should receive similar results:

<img src="https://github.com/schoenemeyer/ESI-OpenFoam-in-Azure/blob/master/openfoam-a9.png" width="432">


