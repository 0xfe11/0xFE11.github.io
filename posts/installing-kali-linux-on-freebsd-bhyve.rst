.. title: Installing Kali Linux on FreeBSD bhyve
.. slug: installing-kali-linux-on-freebsd-bhyve
.. date: 2021-03-13 14:18:50 UTC+08:00
.. tags: 
.. category: 
.. link: 
.. description: 
.. type: text

Currently I wanted to learn some penetration testing and I have heard a fair bit about FreeBSD's hypervisor bhyve and I thought why not 
install one Kali/Parrot on my FreeBSD machine. Bhyve is a a FreeBSD hypervisor built into the base of FreeBSD. One can set up the vm using
just bhyve and the FreeBSD handbook, but I decided to use the tool vm-bhyve to help me with the task. After much tinkering, this is the steps
that I have collected to deploy a Kali on my machine. Note: I have issues with booting Parrot OS somehow. I just got a blinking cursor which
I could not got through. I will need to take a look another time. For now this are the steps that I have done.


.. code-block:: bash

   # Install grub2-bhyve for linux grub bootloader
   # vm-bhyve is wrapper for bhyve
   doas pkg install vm-bhyve grub2-bhyve

   # Install any vnc client you like, for me I am using tigervnc
   doas pkg install tigervnc

   # Load kernel virtualization module
   doas kldload if_bridge if_tap nmdm vmm

   # Make loading of kernel modules persistent
   doas echo 'if_bridge_load="YES"' >> /boot/loader.conf
   doas echo 'if_tap_load="YES"' >> /boot/loader.conf
   doas echo 'nmdm_load="YES"' >> /boot/loader.conf
   doas echo 'vmm_load="YES"' >> /boot/loader.conf

   # Set configuration for byhve-vm
   doas sysrc vm_enable="YES"
   doas sysrc vm_dir="zfs:zroot/vms"
   doas sysrc vm_delay="5"

   # Edit our pf rules
   doas vi /etc/pf.conf

   # Add the following, taken from https://aliov.org/index.php/2020/03/22/bhyve-nat-vm-setup/
   # Note: IPs are taken from reference of https://github.com/churchers/vm-bhyve/wiki/NAT-Configuration
   nat on wlan0 from {192.168.8.0/24} to any -> (wlan0)

   # Enable ip forwarding
   doas sysctl net.inet.ip.forwarding=1

   # Set configuration for NAT (network for our guest)
   gateway_enable="yes"
   pf_enable="yes"

   # Start PF if not started
   doas service pf start

   # Otherwise restart
   doas service pf restart

   # Create filesystem for VMs, all the vms will live inside, managed by vm-bhyve (settings above)
   doas zfs create -o mountpoint=/vms zroot/vms
   doas vm init
   doas cp /usr/local/share/examples/vm-bhyve/* /vms/.templates/

   # Create bridge device, as I am using wifi, I set it to wlan0
   doas vm switch create -a 192.168.8.1/24 public
   doas vm switch add public wlan0

   # To see switch info
   doas vm switch info

   # To see all switches
   doas vm switch list

   # Grab Kali from the net (Note: source could be local filesystem) will be downloaded to /vms/.iso
   doas vm iso https://cdimage.kali.org/kali-2021.1/kali-linux-2021.1-installer-amd64.iso

   # Create the VM (-t stands for template and -s is the size of the vm)
   # More templates can be found at /vms/.templates
   doas vm create -t debian -s 20G mykali

   # Configure things like memory, cpu, uefi, vnc settings
   doas vm configure mykali

   # This are the things I added, refer to https://github.com/churchers/vm-bhyve/wiki/UEFI-Graphics-(VNC)
   cpu=2
   memory=4G
   uefi="yes"
   graphics="yes"
   graphics_res="1600x1200"
   xhci_mouse="yes"

   # To check the iso name in the iso store
   doas vm iso

   # In this case kali-linux-2021.1-installer-amd64.iso is the iso I want
   DATASTORE           FILENAME
   default             kali-linux-2021.1-installer-amd64.iso

   # Start installation from the ISO, this step will boot the vm with the iso
   doas vm install mykali  kali-linux-2021.1-installer-amd64.iso

   # Since I am intending to use with TigerVNC, I put in 127.0.0.1:5900 (default vnc port) for the host
   vncviewer 127.0.0.1:5900

   # Due to the uefi folder not in BOOT, it will not boot properly hence what I did was use the expert install menu
   # and when prompted about installing grub to my removable drive, I click yes and it will show a installing to dummy grub
   # Otherwise if you miss this, you can still remediate

   # After install, you can always configure again if you need to add resource whatsoever
   doas vm configure mykali
   
   # Enable autostart of the newly created VM, if you want
   doas sysrc vm_list="mykali"

   # Otherwise you can manually start with
   doas vm start mykali

   # If you have not done the installing grub fix, you might be faced with an boot failed efi cdrom/dvd message.
   # Wait a while for a shell to appear, type exit the quit the shell and you should see a bhyve boot menu
   # go to Boot Maintainence Manager > Boot From File > YOUR_BOOT_LABEL
   # You should see an EFI entry inside, go ahead into kali and select the grubx64.efi, your OS should not show the
   # bootloader

   # Once inside, do the following to fix the grub issue, Note: this have to be done everytime grub updates
   sudo su
   mkdir /boot/efi/EFI/BOOT/
   cd /boot/efi/EFI/kali
   cp grubx64.efi /boot/efi/EFI/BOOT/bootx64.efi

   # Reboot the machine to see the change
   reboot

   # To stop the vm and power down if there is any issue, other can just power off from the guest
   doas vm stop mykali

   # To see list of all vms and it's state
   doas vm list

   # To set up internet in the kali, right click on the network connection icon on the top bar and click edit connection
   # Go to the ipv4 settings and set the machine ip to be 192.168.8.2/24 and the gateway to be 192.168.8.1
   # Set the DNS to 8.8.8.8 (Google DNS)
   # Try to ping the following and make sure it is not timed out
   ping -c 2 192.168.8.1
   ping -c 2 8.8.8.8

   # If all else succeed, you will have Kali Linux running on your FreeBSD laptop with internet access.

All thanks to (for troubleshooting and guides):
   - `Install Ubuntu on FreeBSD with byhve (Update Ubuntu 18.04)`_
   - `How to install Linux VM on FreeBSD using bhyve and ZFS`_
   - `UEFI Graphics (VNC) vm-bhyve wiki`_
   - `Nat configuration vm-bhhyve wiki`_
   - `FreeNAS 11 “Boot Failed. EFI Misc Device” Fix (Debian)`_
   - `Solving UEFI Boot Problems on Bhyve / FreeNAS VM`_
   - `bhyve wireless NAT vm setup`_

.. _Install Ubuntu on FreeBSD with byhve (Update Ubuntu 18.04): https://www.davd.io/install-ubuntu-on-freebsd-with-bhyve/
.. _How to install Linux VM on FreeBSD using bhyve and ZFS: https://www.cyberciti.biz/faq/how-to-install-linux-vm-on-freebsd-using-bhyve-and-zfs/
.. _UEFI Graphics (VNC) vm-bhyve wiki: https://github.com/churchers/vm-bhyve/wiki/UEFI-Graphics-(VNC)
.. _Nat configuration vm-bhhyve wiki: https://github.com/churchers/vm-bhyve/wiki/NAT-Configuration
.. _FreeNAS 11 “Boot Failed. EFI Misc Device” Fix (Debian): https://blog.tkrn.io/freenas-11-boot-failed-efi-misc-device-fix-debian/
.. _Solving UEFI Boot Problems on Bhyve / FreeNAS VM: https://www.jongibbins.com/solving-uefi-boot-problems-on-bhyve-freenas-vm/
.. _bhyve wireless NAT vm setup: https://aliov.org/index.php/2020/03/22/bhyve-nat-vm-setup/
