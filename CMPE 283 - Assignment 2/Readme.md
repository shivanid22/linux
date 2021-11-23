# CMPE 283 - Assignment 2 - Instrumentation via hypercall
#### Name: Shivani Degloorkar
#### Student ID: 015237664







## Questions:

### 1. For each member in your team, provide 1 paragraph detailing what parts of the lab that member implemented / researched.

### Answer: 
I did it on my own

### 2. Describe in detail the steps you used to complete the assignment. Consider your reader to be someone skilled in software development but otherwise unfamiliar with the assignment.

### Answer:

I have implemented the functionality:
1. To return the total number of exits (all types) - 0x4FFFFFFFF.
2. To calculate the total time spent processing all exits - 0x4FFFFFFFE.

Prerequisite - Configuration setup from Assignmnet 1.

#### Steps to implement:

1. Declare variable in vmx.c such that it can be accessed from cpuid.c
For part 1:
extern u32 total_exits;

For part 2:
extern u64 total_time_all_exits;

2. Write the logic for both inside vmx_handle_exit function
For part 1:
total_exits++;

For part 2:
u64 total_time;

u64 begin_time = rdtsc();
u64 end_time = rdtsc();
u64 delta = end_time - begin_time;
total_time = total_time + delta;

3. Import the variables from step 1 in cpuid.c
u32 total_exits = 0;
EXPORT_SYMBOL(total_exits);
u64 total_time_all_exits = 0;
EXPORT_SYMBOL(total_time_all_exits);

4. In cpuid.c, in kvm_emulate_cpuid function, if eax == 0x4FFFFFFFF, write total_exits to eax and if eax == 0x4FFFFFFFE, assign the high 32 bits of the total time spent processing all exits to %ebx and low 32 bits to %ecx

5. Save the above programs and run following commands:

make modules

make -j 8 modules

sudo bash

make INSTALL_MOD_STRIP=1 modules_install && make install

rmmod kvm_intel
rmmod kvm

modprobe kvm
modprobe kvm_intel

sudo apt install cpu-checker


sudo apt-get install virt-manager

sudo apt-get install libvirt-bin libvirt-doc

sudo apt-get install qemu-system

sudo virt-manager

6. Setup VM inside virtual machine manager using iso image 

7. Install cpuid package inside VM by downloading debian package

8. Run commands:

cpuid --leaf=0x4FFFFFFFF
cpuid --leaf=0x4FFFFFFFE





