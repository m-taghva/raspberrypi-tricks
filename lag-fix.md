
Ubuntu 22.04 is the first release of Ubuntu that works well on the Raspberry Pi 4 without any lag or stutter.
Most of the credit for this goes to the work done on the Mesa drivers over the last year by Igalia. The 2D graphics support and Vulkan 3D support for the Pi 4 are currently in great shape and continue to pick up improvements.

- Disable Zswap and Enable Zram:
   Ubuntu 22.04 enables the use of Zswap by default. Zswap works similarly to Zram but requires a swap file stored on disk. If you have the 4GB or 8GB model of Pi 4 you can disable Zswap, and enable Zram instead, to get slightly better performance.
   Zswap is built into the Linux kernel while Zram is available as an optional kernel module. Run the following command to install the Zram kernel module.
          
      #sudo apt install -y linux-modules-extra-rasp

      To configure Zram we have 2 packages to choose from: 
      zram-config Developed by Canonical. Simple to use but lacks configuration options.
      zram-tools Similar to Zram-config. Provides a configuration file to select the compression algorithm, threshold, size and priority.
      Use either zram-config or zram-tools. Don’t install both.
      Run the following command to install zram-tools (and remove zram-config if installed)

      #sudo apt install -y zram-tools
      #sudo apt autoremove --purge -y zram-config
 
      Edit the configuration file as shown below:

      #sudo nano /etc/default/zramswap
 
      The Zram priority is set to 100. The system will always use Zram first before using the swap file.
      Zswap and Zram should not be used together. Run the command below to disable Zswap:
	
      #sudo sed -i -e 's/zswap.enabled=1/zswap.enabled=0/' /boot/firmware/cmdline.txt

      Reboot the system for the changes to take effect.
      After reboot, you can check the Zram usage with the following command:
	
      #zramctl
