# Separation and isolation 

Isolating processes can reduce the likelihood of a compromised process affecting the entire environment. Process isolation can be achieved by spreading services across several physical servers, virtual machines, or within a single environment using chroot or Linux Containers.

  * [Chroot jails](Chroot-jails.md)
  * [FreeBSD jails](FreeBSD-jails.md)
  * [KVM](KVM.md)
  * [Docker](Docker.md)
  * [Communication](Communication.md)
  * [Services audit](Services-audit.md)

## Notes

* FreeBSD jails and chroot jails use operating-system-level virtualisation and are often used instead of virtual machines to create multiple isolated instances of a host OS. It is a kernel level virtualisation and has practically no overhead as compared to Virtual Machines, which provide virtualisation on an application layer level. chroot is useful for creating multiple isolated instances on the same hardware.
* A Virtual Machine (VM) is a whole other guest computer running on top of a host computer (sitting on top of a layer of virtualisation).
    * If you want to install a binary Linux distribution as a guest, use KVM. It's faster and its' drivers are included in the official kernel tree. 
    * If your guest involves lots of compiling and needs some more advanced features, and/or isn't a Linux system, go with VirtualBox
* A Container is an isolated portion of the host computer, sharing the host kernel (OS) and even its bin/libraries if needed. Introduced in 2008, LXC adopted much of its functionality from Solaris Containers (or Solaris Zones) and FreeBSD jails. With Docker application tiers and services can be separated from one another, and from data. Docker is developed in the Go language and relies on the operating system's own functionality as provided by the underlying infrastructure.

