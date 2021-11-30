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
1. To return the number of exits for the exit number provided (on input) in %ecx - 0x4FFFFFFFD.
2. To return the time spent processing the exit number provided (on input) in %ecx - 0x4FFFFFFFC.

Prerequisite - Configuration setup from Assignmnet 1.

#### Steps to implement:

1. Declare variable in vmx.c such that it can be accessed from cpuid.c
	For part 1:
		extern int exit_reason_arr[70];

	For part 2:
		extern atomic_long_t exit_time[70];


2. Write the logic for both inside vmx_handle_exit function

	For part 1:
		To get the exit count for particular exit increment the array in that specific vm exit function :
		exit_reason_arr[19]++;


	For part 2:
		To get the exit time for particular exit in that specific vm exit function, initialise the begin cycle in each vm exit function and this begin cycle records the 		 time when particular exit happens:

		u64 begin_cycle = rdtsc();

		And call the following function from that specific vm exit function:

		static void atomic_exit_time_cal(int index, u64 begin_cycle) {
		
		u64 end_cycle = rdtsc();

		u64 total_cycles = atomic_long_read(&exit_time[index]);

		total_cycles = total_cycles + (end_cycle - begin_cycle);

		atomic_long_set(&exit_time[index], total_cycles);

		}



3. Import the variables from step 1 in cpuid.c
	int exit_reason_arr[70]= {};
	
	EXPORT_SYMBOL(exit_reason_arr);
	
	atomic_long_t exit_time[70] = ATOMIC_INIT(0);
	
	EXPORT_SYMBOL(exit_time);
	

4. In cpuid.c, in kvm_emulate_cpuid function, 

		if(eax == 0x4ffffffd) {
	
		switch(ecx) {
		
			case 0: eax = exit_reason_arr[0];
			
				break;
				
			case 1: eax = exit_reason_arr[1];
			
				break;
				
			case 2: eax = exit_reason_arr[2];
			
				break;
				
       			 .......
	
		 }
 
 
		For eax==0x4ffffffc, assign high 32 bits of the total time spent for that exit in %ebx and low 32 bits of the total time spent for that exit in %ecx for the exit 		  number provided (on input) in %ecx.


		if(eax == 0x4ffffffc) {
 			u64 t_exit_time = 0;
			
  			switch(ecx) {
			
  			case 0: t_exit_time = atomic_long_read(&exit_time[0]);
			
	   		eax = 0;
			
	   		ebx = t_exit_time >>32 & 0xffffffff;
			
     			ecx = t_exit_time & 0xffffffff;
			
     			edx = 0;
			
    			break;
			
  			}
			
		}
		


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

	For exit types present in SDM:
	
		cpuid -leaf=0x4FFFFFFFD -s 11
		
		cpuid -leaf 0x4FFFFFFFC -s 11


	For exit types absent in SDM:
	
		cpuid -leaf 0x4FFFFFFFC -s 38
		
		cpuid -leaf 0x4FFFFFFFD -s 38
		


## Questions

#### Comment on the frequency of exits – does the number of exits increase at a stable rate? Or are there more exits performed during certain VM operations? Approximately how many exits does a full VM boot entail?
Answer:

Number of frequency exits increases but not with the stable rate. When the VM is idle or not performing any operation on VM then number of exits are stable. But when you perform any operation such as booting or external interrupts then number of VM exit increases significantly.

Boot operation increases the number of exits significantly. Exit count is approximately 56k.

#### Of the exit types defined in the SDM, which are the most frequent? Least?
Answer:
The most frequent exit type is exit number 48. The exit count is approximately 532152.
The least frequent exit type is exit number 55. The exit count is approximately 9.

