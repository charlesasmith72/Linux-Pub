### **✅ Windows EFI Partition Exists - Next Steps**
Your `fdisk -l` output confirms that your **Windows EFI partition exists** (`/dev/nvme0n1p1` at 260MB), but **it's not being detected by GRUB**. Let's fix that!

---

## **🛠 Step 1: Manually Mount the Windows EFI Partition**
Try mounting the EFI partition and check its contents:
```bash
sudo mount /dev/nvme0n1p1 /mnt
ls /mnt/EFI/
```
Expected output:
```
BOOT  Microsoft  void_grub
```
If **Microsoft is missing**, the Windows Boot Manager needs to be restored.

---

## **🛠 Step 2: Manually Add Windows to GRUB**
If `os-prober` doesn’t detect Windows, **manually add it**.

1. **Find the EFI partition UUID**:
   ```bash
   blkid /dev/nvme0n1p1
   ```
   Example output:
   ```
   /dev/nvme0n1p1: UUID="XXXX-XXXX" TYPE="vfat"
   ```
   Copy the **UUID**.

2. **Edit the GRUB configuration file**:
   ```bash
   sudo nano /etc/grub.d/40_custom
   ```
   Add this entry at the bottom:
   ```
   menuentry "Windows 10" {
       insmod part_gpt
       insmod fat
       search --no-floppy --fs-uuid --set=root XXXX-XXXX
       chainloader /EFI/Microsoft/Boot/bootmgfw.efi
   }
   ```
   Replace `XXXX-XXXX` with your **EFI partition UUID**.

3. **Save and exit** (`Ctrl + X`, then `Y`, then `Enter`).

4. **Update GRUB**:
   ```bash
   sudo grub-mkconfig -o /boot/grub/grub.cfg
   ```

5. **Reboot and check if Windows appears**:
   ```bash
   reboot
   ```

---

## **🛠 Step 3: Restore Windows Boot Manager (If It’s Missing)**
If `/mnt/EFI/Microsoft/Boot/bootmgfw.efi` does not exist, restore it using a **Windows USB drive**:

1. **Boot from a Windows USB**.
2. Select **Repair your computer** → **Troubleshoot** → **Command Prompt**.
3. Run:
   ```cmd
   diskpart
   list disk
   select disk 0
   list partition
   select partition 1  (Replace with your EFI partition)
   assign letter=S
   exit
   ```
4. Restore the Windows bootloader:
   ```cmd
   bcdboot C:\Windows /s S: /f UEFI
   ```
5. **Reboot into Linux** and update GRUB:
   ```bash
   sudo os-prober
   sudo grub-mkconfig -o /boot/grub/grub.cfg
   ```

---

## **🎯 Final Summary**
✔ **Mount the EFI partition (`mount /dev/nvme0n1p1 /mnt`)**  
✔ **Check if Windows Boot Manager exists (`ls /mnt/EFI/Microsoft/Boot/bootmgfw.efi`)**  
✔ **Manually add Windows to GRUB (`/etc/grub.d/40_custom`)**  
✔ **Use a Windows USB to restore the Boot Manager if missing (`bcdboot C:\Windows /s S: /f UEFI`)**  

Try these steps and let me know if you need further help! 🚀
