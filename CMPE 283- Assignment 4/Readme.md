
# CMPE 283 - Assignment 4 - Nested Paging vs Shadow Paging 
#### Names: Shivani Degloorkar and Shervin Suresh 
#### Student IDs: 015237664 011229205







## Questions:

### 1. For each member in your team, provide 1 paragraph detailing what parts of the lab that member implemented / researched.

### Answer: 
Shervin- I had helped in working on fixing the issues both Shivani and I were facing with the corruption of our VMs. Thus in order to fix those issues I had spent time researching the proper configurations of how to operate nested VMs. I had also researched how to make the inner VM function correctly as we were running into issues with crashing and the inner VM not connecting to the internet. Then after researching this issues I had booted up a new VM and worked on running the linux from Shivani.  
  Shivani- Shivani helped in pinpointing the issues each of us were having with our seperate VMs. More importantly Shivani checked the functionality of the code from Assignment 3. This allowed us to re-run assignment 3 completely in the new set up VM, to make sure it was correct. This running also allowed us to remedy some issues we had with assignment 3, those issues were fixed by Shivani.  

### 2. Describe in detail the steps you used to complete the assignment. Consider your reader to be someone skilled in software development but otherwise unfamiliar with the assignment.

### Answer:

Checked the functionality:
1. Viewing the performance of nesting paging.  
2. Viewing the performance of shadow paging.
3. Illustrating the different exit frequencies and exits.  

Prerequisite - Configuration setup from Assignment 3.

#### Steps to implement:

1. Run the updated kernel from this repo
- git clone https://github.com/shivanid22/linux.git  
2. Do the steps to build the kernel
3. Create a test VM inside the VM
- We had chosen an ubuntu VM, which allows us to run CPUID commands  
4. record the exit count after running the command: cpuid --leaf=0x4ffffffe multiple times
5. Shutdown the inner VM PROPERLY
6. edit the kvm-intel.ko with ept=0
- see the picture for the command:
![alt text](https://github.com/shivanid22/linux/blob/master/CMPE%20283-%20Assignment%204/entering%20ept%3D0.png)  
7. Reboot the VM
8. After Reboot open the inner VM
9. run step 4 again and see the changes in the total exit count


### Output:  
#### after running: cpuid --leaf=0x4FFFFFFFE  
after running the command x times
![alt text](https://github.com/shivanid22/linux/blob/master/CMPE%20283-%20Assignment%204/example%20of%20output%20before%20ept%3D0%20(1).png)  
after running the command x+1 times  
![alt text](https://github.com/shivanid22/linux/blob/master/CMPE%20283-%20Assignment%204/example%20of%20output%20before%20ept%3D0%20(2).png)  
#### Total exit Count before setting ept=0:  
![alt text](https://github.com/shivanid22/linux/blob/master/CMPE%20283-%20Assignment%204/Total%20Exit%20Count%20before%20ept%3D0.png)  
  
#### after setting ept=0:  
after running: cpuid --leaf=0x4FFFFFFFE 
after running the command x times
![alt text](https://github.com/shivanid22/linux/blob/master/CMPE%20283-%20Assignment%204/example%20of%20output%20after%20ept%3D0%20(1).png)  
after running the command x+1 times  
![alt text](https://github.com/shivanid22/linux/blob/master/CMPE%20283-%20Assignment%204/eaxmple%20of%20output%20after%20ept%3D0%20(2).png)  
#### Total exit Count after setting ept=0:  
![alt text](https://github.com/shivanid22/linux/blob/master/CMPE%20283-%20Assignment%204/Total%20Exit%20count%20after%20ept%3D0.png)

## Questions 3 and 4:
### 3. What did you learn from the count of exits? Was the count what you expected? If not, why not?  
The Total Exit Count observed when ept was set to 0 was more than when ept was not set to 0. This is what we expected since ept=0 was shadow paging, and ept=1 is nested paging.  
As seen above the difference between 0x4ffffffe commands run in ept=1 was 63311298-62572216= 739,082  
The difference between 0x4ffffffe commands run in ept=0 was 71150674-67157626= 3,993,048  
That difference is 5.4 times
### 4. What changed between the two runs (ept vs no-ept)?  
The biggest change that occured between these runs are running with ept and without ept.  
Without ept (ept=0) the VM was in shadow paging, which degraded the VMs performance due to the increased exits. The degredation was felt in the lesser responsiveness of the VM.  
With ept (ept=1), the VM was in nested paging which was better in terms of observed performance and also had fewer exits as well.  
The change in the responsiveness of the VM in ept vs no-ept mode most likely stems from ept allowing to sync the memeory less, unlike in no-ept where the memory needs to be synced more.  

