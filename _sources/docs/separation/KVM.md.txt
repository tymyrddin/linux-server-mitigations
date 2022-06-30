# KVM

* The kernel module `kvm.ko` is a loadable kernel, giving VMs direct access to the hardware.
* KVM comes with processor-specific kernels `kvm-amd.ko` and `kvm-intel.ko`.
* It uses a combination of security-enhanced Linux (SELinux) and secure virtualization (sVirt) for VM security and isolation. 
  * `SELinux` sets up security boundaries around VMs. 
  * `sVirt` allows for applying Mandatory Access Control (MAC) security to guest VMs and prevents manual labelling errors.



