### **✅ Fix Windows Not Showing in GRUB on VX Linux**
Since Windows is missing from GRUB, follow these steps to **restore it**.

---

## **🛠 Step 1: Ensure `os-prober` Is Installed**
First, check if `os-prober` is installed:
```bash
xbps-query -Rs os-prober
```
If it’s not installed, install it:
```bash
sudo xbps-install -S os-prober
```

Now, enable Windows detection inside GRUB:
```bash
echo 'GRUB_DISABLE_OS_PROBER=false' | sudo tee -a /etc/default/grub
```

---

## **🛠 Step 2: Detect Windows Bootloader**
Run:
```bash
sudo os-prober
```
If it detects Windows, you’ll see an output like:
```
/dev/nvme0n1p3:Windows 10:Windows:chain
```
Now, update GRUB:
```bash
sudo grub-mkconfig -o /boot/grub/grub.cfg
```
Reboot and check if Windows appears:
```bash
reboot
```

---

## **🛠 Step 3: Manually Add Windows to GRUB (If Needed)**
If `os-prober` does not detect Windows, **manually add it**.

1. **Find the Windows Bootloader**
   ```bash
   ls /boot/efi/EFI/
   ```
   If Windows is installed, you should see:
   ```
   Microsoft
   ```
   Check for the bootloader file:
   ```bash
   ls /boot/efi/EFI/Microsoft/Boot/bootmgfw.efi
   ```
   If this file exists, we can manually add it to GRUB.

2. **Manually Add Windows to GRUB**
   Open the GRUB configuration file:
   ```bash
   sudo nano /etc/grub.d/40_custom
   ```
   Add the following at the bottom:
   ```
   menuentry "Windows 10" {
       insmod part_gpt
       insmod fat
       search --no-floppy --fs-uuid --set=root <EFI-UUID>
       chainloader /EFI/Microsoft/Boot/bootmgfw.efi
   }
   ```
   - Replace `<EFI-UUID>` with the **UUID** of `/dev/nvme0n1p1`. Get it using:
     ```bash
     blkid /dev/nvme0n1p1
     ```
   - Copy the **UUID** from the output and replace `<EFI-UUID>` in the script.

3. **Save and Exit (`Ctrl + X`, then `Y`, then `Enter`)**.

4. **Update GRUB**:
   ```bash
   sudo grub-mkconfig -o /boot/grub/grub.cfg
   ```

5. **Reboot and Check GRUB Menu**:
   ```bash
   reboot
   ```

---

## **🛠 Step 4: Boot Windows Using `efibootmgr` (If GRUB Fails)**
If GRUB still doesn’t detect Windows, you can **manually boot into Windows**:

1. **Find the Windows Boot Entry**:
   ```bash
   sudo efibootmgr
   ```
   Look for a line like:
   ```
   Boot0001* Windows Boot Manager
   ```
   If found, boot into Windows manually:
   ```bash
   sudo efibootmgr --bootnext 0001
   reboot
   ```
   (Replace `0001` with the actual **Boot ID** for Windows.)

---

### **🎯 Final Summary**
✔ **Check if `os-prober` is installed and enable it (`GRUB_DISABLE_OS_PROBER=false`)**  
✔ **Run `os-prober` and `grub-mkconfig` to detect Windows automatically**  
✔ **If Windows is missing, manually add it to `/etc/grub.d/40_custom`**  
✔ **Use `efibootmgr` to manually boot Windows if GRUB still doesn’t work**  

Try these steps and let me know if you need more help! 🚀
