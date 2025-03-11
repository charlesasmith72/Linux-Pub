### **🔍 Windows Boot Manager Is Missing**
Your output shows that **`/boot/efi/EFI/` does not contain a `Microsoft` folder**, which means that the **Windows Boot Manager is missing or not mounted correctly**.  

Let's try restoring it.

---

## **✅ Step 1: Check If the Windows EFI Partition Exists**
Run:
```bash
sudo fdisk -l
```
Look for a partition labeled **EFI System Partition (ESP)** (usually **`/dev/nvme0n1p1`**).

If it's there, mount it manually:
```bash
sudo mount /dev/nvme0n1p1 /mnt
ls /mnt/EFI
```
You should see:
```
BOOT  Microsoft  void_grub
```
If `Microsoft` is missing, Windows Boot Manager needs to be **restored**.

---

## **✅ Step 2: Restore Windows Boot Manager**
### **Option 1: Using a Windows USB Drive**
1. **Boot from a Windows installation USB**.
2. Select **Repair your computer** → **Troubleshoot** → **Command Prompt**.
3. Run the following commands:
   ```cmd
   diskpart
   list disk
   select disk 0  (Replace with your actual disk)
   list partition
   select partition 1  (Replace with your EFI partition)
   assign letter=s
   exit
   ```
4. Restore the bootloader:
   ```cmd
   bcdboot C:\Windows /s S: /f UEFI
   ```
5. **Reboot** and run:
   ```bash
   sudo os-prober
   sudo grub-mkconfig -o /boot/grub/grub.cfg
   ```

---

### **Option 2: Manually Reinstall GRUB (If Windows Is Intact)**
If you don’t have a Windows USB, try manually reinstalling GRUB:
```bash
sudo grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=void_grub
sudo grub-mkconfig -o /boot/grub/grub.cfg
```
Reboot and check if Windows appears.

---

### **🎯 Final Summary**
✔ **Check if the Windows EFI partition exists (`fdisk -l`)**  
✔ **Manually mount the EFI partition (`mount /dev/nvme0n1p1 /mnt`)**  
✔ **Restore Windows Boot Manager using a Windows USB (`bcdboot C:\Windows /s S: /f UEFI`)**  
✔ **Reinstall GRUB if necessary (`grub-install` + `grub-mkconfig`)**  

Try these steps and let me know what happens! 🚀
