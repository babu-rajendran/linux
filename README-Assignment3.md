##### 1. For each member in your team, provide 1 paragraph detailing what parts of the lab that member implemented / researched. (You may skip this ques!on if you are doing the lab by yourself).
I did this by myself.

##### 2. Describe in detail the steps you used to complete the assignment. Consider your reader to be someone skilled in so"ware development but otherwise unfamiliar with the assignment. Good answers to this ques!on will be recipes that someone can follow to reproduce your development steps.
For assignment 2, I had done it using Google Cloud. But I ran out of credits. So for Assignment 3, I decided to use my dual boot Mac and ubuntu setup. But in Ubuntu, after I built the source, I ended up in an "initramfs" screen and I was stuck there. I could not even login to my Mac. I had to boot to my Mac in recovery mode and then I was able to login to my Mac but I lost my Ubuntu partition. So I decided to use VirtualBox in Mac to create a Linux environment.

I created a Ubuntu VM on VirtualBox and did the steps needed to build the linux code. Prior to launching the VM, I enabled nested virtualization on the VM with the command:

```
VBoxManage modifyvm linux --nested-hw-virt on
```

<img width="643" alt="Screen Shot 2020-12-14 at 2 56 51 AM" src="https://user-images.githubusercontent.com/6368257/102055748-55d01b00-3e11-11eb-93a3-f321e00fffcc.png">
<img width="643" alt="Screen Shot 2020-12-14 at 6 17 04 AM" src="https://user-images.githubusercontent.com/6368257/102055769-5f598300-3e11-11eb-96f5-f98fe4605e68.png">
<img width="930" alt="Screen Shot 2020-12-14 at 10 22 42 AM" src="https://user-images.githubusercontent.com/6368257/102055780-641e3700-3e11-11eb-8f94-5031096770ce.png">

##### 3. Comment on the frequency of exits – does the number of exits increase at a stable rate? Or are there more exits performed during certain VM opera!ons? Approximately how many exits does a full VM boot entail?

##### 4. Of the exit types defined in the SDM, which are the most frequent? Least?
