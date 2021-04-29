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
####  if(eax == 0x4FFFFFFF) then return total number of exits in %eax, high 32bits of total time spent in exit of %ebx, high 32 bits of total time spent in exit in %ecx. (we get hint from the assignment 1, how to get high32bits and low 32bits from a 64 bits data.)
#### 2. vi linux/arch/x86/kvm/vmx/vmx.c
####   Initialize exit_counters and exit_time. Then find the function -> vmx_handle_exit()
####     count the time for each exit execute(Please view the video 5 1:35:00, professor gives lots of hint to do this part)
#### 3. rebuild again, just view the Build the kernal steps above(use this two command-> "sudo bash" -> "make -j 3 modules && make -j 3 && sudo make modules_install && sudo make install")

## Install nested VM
#### 1. Please follow the instruction here: https://help.ubuntu.com/community/KVM/Installation
#### 2. Make sure whether your hardware support VM. If so, go to next step
#### 3. Install Cosmic(18.10) or later package -> sudo apt-get install qemu-kvm libvirt-daemon-system libvirt-clients bridge-utils
#### 4. Installed the virt-manager-> sudo apt-get install virt-manager
#### 5. To check whether install successfully -> virt-manager
#### 6. Go to ubuntu official website, download ISO, then use virt-manager to open ISO and install it.
![](https://github.com/kdlasbang/cmpe283-assignment2/blob/main/IMG_6042.jpg )
#### 7. Open nested VM terminal -> sudo apt-get install cpuid
#### 8. -> sudo apt-get install make

## Test file (inside nested VM)
#### 1. In command line, view the exit register -> cpuid -l 0x4FFFFFFF
![](https://github.com/kdlasbang/cmpe283-assignment2/blob/main/IMG_6039.jpg)
#### 2. View exit counter and time -> gcc test.c    -> ./a.out
![](https://github.com/kdlasbang/cmpe283-assignment2/blob/main/IMG_6041.jpg)

## Comment on the frequency of exits – does the number of exits increase at a stable rate? Or are there more exits performed during certain VM operations? Approximately how many exits does a full VM boot entail?

### Output file -> (https://github.com/kdlasbang/cmpe283-assignment2/blob/main/IMG_6039.jpg  and https://github.com/kdlasbang/cmpe283-assignment2/blob/main/IMG_6041.jpg)

#### 1.Not sure, the number of exits increase in a very slightly rate. Hard to detect whether it is stable or not. But these Exit may be performed in operations:  EPT_VIOLATION, MSR_WRITE or page fault.
#### 2. Approximately 7500000 exits for a full VM boot entail.




## Reference:
#### Thanks for the instruction from https://github.com/ParvathiRPai/CMPE283-Assignment2  . It gives lots of help in Setup KVM part and testing part. The test file here is the test file from ParvathiRPai's github.
#### https://help.ubuntu.com/community/KVM/Installation
#### https://elixir.bootlin.com/linux/v5.10-rc2/source/include/asm-generic/atomic-instrumented.h
#### https://www.kernel.org/doc/html/v4.12/core-api/atomic_ops.html
#### https://lwn.net/Kernel/
