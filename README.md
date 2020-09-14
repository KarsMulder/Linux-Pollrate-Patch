This repository contains a highly experimental patch for the Linux kernel which provides a new `usbcore.interrupt_interval_override` module parameter that can be used to forcefully override the interrupt polling interval of an USB device.

# Usage

This parameter will override the polling interval on all interrupt-type endpoints on a device identified by their vendor and product ID. An example usage of this parameter would be:

```usbcore.interrupt_interval_override=045e:00db:16,1bcf:0005:2```

This gives all USB devices with id `045e:00db` an interrupt interval for 16ms, and all USB devices with id `1bcf:0005` an interrupt interval of 2ms. You can discover the id of your devices using the `lsusb` tool.

This parameter can be set at boot time by adding a parameter to your linux command line in your bootloader, or at runtime by writing to the file `/sys/module/usbcore/parameters/interrupt_interval_override`. For example:

```echo "045e:00db:16,1bcf:0005:2" | sudo tee /sys/module/usbcore/parameters/interrupt_interval_override > /dev/null```

If this parameter is modified at runtime, you may need to unplug&replug the affected USB device.

Not all USB devices support all polling intervals. Using a polling interval other than the one requested by the device may have unintended consequences. Use at your own risk. This program is provided WITHOUT ANY WARRANTY of any kind.

# Applying the patch

At the time of writing, this patch should be applicable to the latest mainline kernel, and probably every kernel since 5.9-rc1. The patch will NOT directly apply to the 5.8.* kernels or older. To create a patched version of the Linux source code, do something like:

```git clone https://github.com/torvalds/linux
git clone https://github.com/KarsMulder/Linux-Pollrate-Patch
cd linux
git apply ../Linux-Pollrate-Patch/pollrate.patch```

The way to compile and install the kernel is distribution specific.

# Legalese

This program is provided under the terms and condiditions of the GNU General Public License version 2. This program is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the GNU General Public License for more details.

