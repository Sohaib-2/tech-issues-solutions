# Complete Guide to GNOME Boxes: Installation, Configuration, and Storage Management

This guide addresses common issues with GNOME Boxes and provides step-by-step solutions to get your virtual machines running smoothly with custom storage locations.

## Table of Contents
1. [Installation Options](#installation-options)
2. [Enabling KVM](#enabling-kvm)
3. [Changing Default VM Storage Location](#changing-default-vm-storage-location)
4. [Troubleshooting](#troubleshooting)

## Installation Options

### Apt vs. Snap: Which to Choose

When installing GNOME Boxes, you have two main options:

**Option 1: APT Installation (Recommended)**
```bash
sudo apt update
sudo apt install gnome-boxes
```

**Option 2: Snap Installation**
```bash
sudo snap install gnome-boxes
```

**Important Note**: The Snap version has limitations accessing removable media due to strict confinement. If you need to access external drives (such as when your OS is installed on a smaller SSD and you want to use a larger external drive for VMs), use the APT version instead.

### Why Choose APT over Snap for GNOME Boxes
- Full access to all mounted drives and removable media
- No permission issues with external storage
- Better integration with the system
- Easier to customize storage locations

## Enabling KVM

If you encounter a "No KVM found" error when starting GNOME Boxes, you need to enable virtualization in your BIOS.

### Steps to Enable KVM:

1. Restart your computer
2. Enter BIOS/UEFI (usually by pressing F2, F10, DEL, or ESC during boot)
3. Look for virtualization settings (may be under different names):
   - Intel VT-x or Intel Virtualization Technology
   - AMD-V or SVM Mode
4. Enable the virtualization option
5. Save changes and exit BIOS
6. Boot back into Ubuntu

### Verify KVM is Enabled:
```bash
kvm-ok
```

If enabled correctly, you should see:
```
INFO: /dev/kvm exists
KVM acceleration can be used
```

## Changing Default VM Storage Location

By default, GNOME Boxes stores VM images in `~/.local/share/gnome-boxes/images/`. To change this to a custom location, follow these steps:

### Method: Using Symbolic Links

1. **Close GNOME Boxes** if it's running.

2. **Create your desired directory** where you want to store VM images:
   ```bash
   mkdir -p /path/to/your/custom/storage/location
   ```

3. **If you have existing VMs**, move them to the new location:
   ```bash
   mv ~/.local/share/gnome-boxes/* /path/to/your/custom/storage/location/
   ```

4. **Remove the original directory** (after confirming files are moved):
   ```bash
   rm -r ~/.local/share/gnome-boxes
   ```

5. **Create a symbolic link** from the default location to your new one:
   ```bash
   ln -s /path/to/your/custom/storage/location ~/.local/share/gnome-boxes
   ```

6. **Set proper permissions** for your new directory:
   ```bash
   chmod -R 755 /path/to/your/custom/storage/location
   ```

7. **Start GNOME Boxes** again.

### Verifying Your New Storage Location

After changing the storage location and starting GNOME Boxes, verify it's working by:

1. Creating a new virtual machine
2. Checking if files appear in your new location:
   ```bash
   ls -la /path/to/your/custom/storage/location/images/
   ```

## Troubleshooting

### "Permission denied" when trying to access removable media
- Solution: Use the APT version of GNOME Boxes instead of Snap

### "No KVM found" or "KVM is not available"
- Solution: Enable virtualization in BIOS (see [Enabling KVM](#enabling-kvm) section)

### Cannot change VM storage location through GNOME Boxes settings
- Solution: GNOME Boxes doesn't have a built-in option to change the storage location. Use the symbolic link method described above.

### VM image files not showing in the new location
- Check if symbolic link was created correctly:
  ```bash
  ls -la ~/.local/share/
  ```
- Ensure proper permissions on the new directory:
  ```bash
  chmod -R 755 /path/to/your/custom/storage/location
  ```

### GNOME Boxes not showing existing VMs after changing location
- Check if all necessary files were moved:
  ```bash
  ls -la /path/to/your/custom/storage/location/images/
  ls -la /path/to/your/custom/storage/location/sources/
  ```
- Ensure the symbolic link is correctly pointing to the new location

## Additional Tips

- **Back up your VMs regularly**: Keep copies of important VM images on separate storage
- **Allocate enough space**: Ensure your storage drive has sufficient free space for VMs
- **Monitor VM size**: Virtual machines can grow in size over time, especially with dynamic allocation

---
