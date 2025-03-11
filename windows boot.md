Since Windows is booting manually from your UEFI boot menu but not appearing in GRUB, you likely need to properly detect and add the Windows Boot Manager entry to GRUB. Follow these steps:

### **1️⃣ Ensure `os-prober` is Enabled**
Run:
```sh
echo 'GRUB_DISABLE_OS_PROBER=false' | sudo tee -a /etc/default/grub
```
Then update GRUB:
```sh
sudo update-grub
```
or on Void Linux:
```sh
sudo xbps-reconfigure -f grub
```

### **2️⃣ Check for Windows Boot Manager**
Run:
```sh
sudo os-prober
```
It should detect Windows and output something like:
```
/dev/nvme0n1p3:Windows Boot Manager:Windows:efi
```
If it does, run:
```sh
sudo update-grub
```
or
```sh
sudo grub-mkconfig -o /boot/grub/grub.cfg
```

### **3️⃣ Manually Add Windows Boot Entry (If Needed)**
If `os-prober` doesn’t detect Windows, create a manual GRUB entry.

Run:
```sh
sudo nano /etc/grub.d/40_custom
```
Add the following:
```sh
menuentry "Windows 11" {
    insmod part_gpt
    insmod fat
    search --no-floppy --set=root --fs-uuid <UUID of Windows EFI partition>
    chainloader /EFI/Microsoft/Boot/bootmgfw.efi
}
```
Find your Windows EFI partition UUID with:
```sh
lsblk -o NAME,FSTYPE,UUID | grep vfat
```
Replace `<UUID of Windows EFI partition>` in the script.

Save and exit, then update GRUB:
```sh
sudo update-grub
```

### **4️⃣ Verify UEFI Boot Order**
Run:
```sh
sudo efibootmgr
```
If GRUB is set as default but doesn’t detect Windows, you may need to manually add it using:
```sh
sudo efibootmgr --create --disk /dev/nvme0n1 --part 1 --label "Windows Boot Manager" --loader "\EFI\Microsoft\Boot\bootmgfw.efi"
```
Adjust the disk (`/dev/nvme0n1`) and partition (`--part 1`) accordingly.

### **5️⃣ Restart & Test**
Reboot and check if Windows appears in GRUB:
```sh
sudo reboot
```

If Windows still doesn’t show up, let me know what happens after each step.
