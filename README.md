# cmpe283-assignment2

##Member-> just myself

## Build the kernal
#### 1.(at ./linux)make menuconfig
### update config->
#### 2.sudo bash (into root mode)
#### 3.uname -a 
### (then copy the config)
#### 4.cp /boot/config-5.4.0-70-generic ./.config
#### 5.make oldconfig (press all enter)
#### 6.vi .config
### find the instruction-> 
#### 7.CONFIG_SYSTEM_TRUSTED_KEY, then comment it. Otherwise, there is a error in build the kernal
### build the kernal with this command->
#### 8.make -j 3 modules && make -j 3 && sudo make modules_install && sudo make install (3 cpu)(approximately need to wait 3 hrs)
#### 9. verify version -> 
#### cmpe283@cmpe283-virtual-machine:~$ uname -a
#### Linux cmpe283-virtual-machine 5.12.0-rc7+ #1 SMP Tue Apr 27 22:31:53 PDT 2021 x86_64 x86_64 x86_64 GNU/Linux
 

## Change to source code->
#### 1.vi linux/arch/x86/kvm/cpuid.c
####     Initial exit_counters and exit_time at the top. Then in the function kvm_emulate_cpuid()->
####  if(eax == 0x4FFFFFFF) then return total number of exits in %eax, high 32bits of total time spent in exit of %ebx, high 32 bits of total time spent in exit in %ecx.
#### 2. vi linux/arch/x86/kvm/vmx/vmx.c
####   Initialize exit_counters and exit_time. Then find the function -> vmx_handle_exit()
####     count the time for each exit execute
#### 3. rebuild again, just view the Build the kernal steps above

## Install nested VM
#### 1. cat /sys/module/kvm_intel/parameters/nested
#### 2. sudo apt install libvirt-clients libvirt-daemon-system ovmf virt-manager qemu-system-x86
#### 3. setup nested VM and Ubuntu 20.0.4 ISO
#### 4. sudo apt-get install cpuid

## Test file
#### 1. test in command line -> cpuid -l 0x4FFFFFFF
#### or 2. test by test file
#### Ouput: CPUID 0x4fffffff, number of exits: 5829019, Cycles spent in exit:  1629102911

## Comment on the frequency of exits – does the number of exits increase at a stable rate? Or are there more exits performed during certain VM operations? Approximately how many exits does a full VM boot entail?

## 1.No, the number of exits increase not in stable rate. Exit reason like EPT_VIOLATION, MSR_WRITE or page fault exits may perform.
## 2. Approximately 1000000 exits for a full VM boot entail.

