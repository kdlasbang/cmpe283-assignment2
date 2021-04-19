# cmpe283-assignment2

#### for CPUID %eax==0x4FFFFFFF
####  return total number of exits in %eax
####  return high 32bits of total time spent in exit of %ebx
####  return high 32 bits of total time spent in exit in %ecx
  
#### cd linux/arch/x86/kvm
#### vi cpuid.c 
#### at the end: if(eax== 0x4fffffff){}  else

#### if we want to test this, we need to build a new kernel

#### (at ./linux)make menuconfig

#### update config->
#### uname -a 
#### cp /boot/config-5.4.0-70-generic ./.config
#### vi .config
#### make oldconfig (press all enter)
#### vi .config
#### make -j 3 modules && make -j 3 && sudo make modules_install && sudo make install (3 cpu)
