# cmpe-283-assignment2
## Modifying instruction behavior in KVM
### Questions

#### 1. For each member in your team, provide 1 paragraph detailing what parts of the lab that member implemented / researched. (You may skip this question if you are doing the lab by yourself).
I did it by myself.

#### 2. Describe in detail the steps you used to complete the assignment.
For this assignment, I wanted to try the option of using Google Cloud even though I have a dual boot setup with macOS and Ubuntu. I followed the steps given in the link provided below to quickly get an instance of Ubuntu running on GCP with nested virtualization enabled.

https://cloud.google.com/compute/docs/instances/enable-nested-virtualization-vm-instances

To summarize, here are the steps to spin an instance in GCP with nested virtualization.

Step: 1

```gcloud compute disks create ubuntu-disk --image-project ubuntu-os-cloud --image-family ubuntu-1604-lts --zone us-central1-a```


Step: 2

```gcloud compute images create nested-vm-image --source-disk ubuntu-disk --source-disk-zone us-central1-a --licenses "https://www.googleapis.com/compute/v1/projects/vm-options/global/licenses/enable-vmx"```


Step: 3

Spin up an instance with the created "nested-vm-image".
<img width="1261" alt="Screen Shot 2020-11-04 at 5 55 36 AM" src="https://user-images.githubusercontent.com/6368257/98067919-aa7e8e80-1e80-11eb-86f3-9841b898607c.png">

SSH into the instance and run the following commands:
This will install the necessary pacakges and dependencies for building the kernel.

```
sudo apt update
sudo apt upgrade
sudo apt install build-essential kernel-package fakeroot libncurses5-dev libssl-dev ccache bison flex libelf-dev
```

Check out the Linux source code.
Also check the kernel version.

<img width="1150" alt="Screen Shot 2020-11-04 at 6 35 48 AM" src="https://user-images.githubusercontent.com/6368257/98068091-1cef6e80-1e81-11eb-9099-362c021e67e3.png">

Change directory to the linux source code and copy the config file as shown in the screenshot.
<img width="1277" alt="Screen Shot 2020-11-04 at 6 43 05 AM" src="https://user-images.githubusercontent.com/6368257/98068135-38f31000-1e81-11eb-9248-053d668dd684.png">

Run "make oldconfig" and press the Return key for all the prompts so that the default options are enabled.
<img width="1277" alt="Screen Shot 2020-11-04 at 6 43 36 AM" src="https://user-images.githubusercontent.com/6368257/98068150-4c05e000-1e81-11eb-89f0-46ab0a2ee40e.png">

<img width="1276" alt="Screen Shot 2020-11-04 at 6 46 40 AM" src="https://user-images.githubusercontent.com/6368257/98068165-545e1b00-1e81-11eb-8a45-adce5f7b4b57.png">

Run this command to compile and build the kernel from source. Since my instance has 4 cores I have given the option "-j 4" with the make command to speed up the process.

```
sudo make -j 4 && sudo make modules -j 4 && sudo make install -j 4 && sudo make modules_install -j 4
```

Once the build is successful reboot the instance.
<img width="1246" alt="Screen Shot 2020-11-04 at 8 02 28 AM" src="https://user-images.githubusercontent.com/6368257/98069119-0a2a6900-1e84-11eb-8183-1d23aef64a29.png">

After reboot, verify that the kernel version has been upgraded.

<img width="900" alt="Screen Shot 2020-11-04 at 8 16 58 AM" src="https://user-images.githubusercontent.com/6368257/98069303-72794a80-1e84-11eb-9478-d45c7a50f452.png">

Once changes are made to any Linux kernel module, they can be loaded and removed using the insmod and rmmod command as described in the assignment 1 and only those modules which have been modified need to be built again.

Install the packages necessary to create a nested VM

<img width="1275" alt="Screen Shot 2020-11-04 at 9 18 31 AM" src="https://user-images.githubusercontent.com/6368257/98069143-18788500-1e84-11eb-9f52-9179d1acd743.png">

#### 3. Comment on the frequency of exits – does the number of exits increase at a stable rate? Or are there more exits performed during certain VM operations? Approximately how many exits does a full VM boot entail?
