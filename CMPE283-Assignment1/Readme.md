# CMPE 283 - Assignment 1 - Discovering VMX Features
#### Name: Shivani Degloorkar
#### Student ID: 015237664







## Questions:

### 1. For each member in your team, provide 1 paragraph detailing what parts of the lab that member implemented / researched.

### Answer: 
I did it on my own

### 2. Describe in detail the steps you used to complete the assignment. Consider your reader to be someone skilled in software development but otherwise unfamiliar with the assignment.

### Answer:
This assignment helps in discovering VMX features present in your processor by writing a Linux kernel module that queries these features.


### Prerequisite:
You will need a machine capable of running Linux, with VMX virtualization features exposed. You may be able to do this inside a VM, or maybe not, depending on your hardware and software configuration. Another option is to do this assignment in Google Cloud (GCP).


### Steps to complete assignment:

1. Fork linux git repository - https://github.com/torvalds/linux

2. Install git - sudo apt-get install git

3. Clone the above forked repository

4. Build the cloned linux kernel and install missing packages such as:

   sudo apt install gcc
   
   sudo apt install flex
   
   sudo apt install bison
   
   sudo apt-get install libssl-dev
   
   sudo apt-get install libelf-dev
   
   sudo apt-get install dwarves
   

5. Reboot the machine

6. Edit the cmpe283-1.c file in order to detect and print required VMX capabilities of the CPU.

7. Build module using - make command

8. Load the above build module into the kernel using - sudo insmod cmpe283-1.ko

9. Verify the output using - dmesg command

10. To edit the .c file, unload the module using - sudo rmmod cmpe283-1.ko command, edit the file and repeat step 7 - 9.


## Results:

![alt text](https://github.com/shivanid22/linux/blob/master/CMPE283-Assignment1/assignment1-1.png)


![alt text](https://github.com/shivanid22/linux/blob/master/CMPE283-Assignment1/assignment1-2.png)




