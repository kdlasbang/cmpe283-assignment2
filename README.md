# cmpe283-assignment2

## Build the kernal
### (at ./linux)make menuconfig
#### update config->
### sudo bash (into root mode)
### uname -a 
#### (then copy the config)
### cp /boot/config-5.4.0-70-generic ./.config
### make oldconfig (press all enter)
### vi .config
#### find the instruction-> 
### CONFIG_SYSTEM_TRUSTED_KEY, then comment it. Otherwise, there is a error in build the kernal
#### build the kernal with this command->
### make -j 3 modules && make -j 3 && sudo make modules_install && sudo make install (3 cpu)

## Change to source code->
#### for CPUID %eax==0x4FFFFFFF
####  return total number of exits in %eax
####  return high 32bits of total time spent in exit of %ebx
####  return high 32 bits of total time spent in exit in %ecx
#### cd linux/arch/x86/kvm
#### vi cpuid.c 
#### at the end: if(eax== 0x4fffffff){}  else
