### **⚠ Windows Bootloader Is Missing - Recovery Needed**
Your screenshot confirms:
1. **The EFI partition (`/dev/nvme0n1p1`) is mounted**.
2. **Only `void_grub` exists, but `Microsoft` is missing**.
3. **Windows (`/dev/nvme0n1p3`) still exists, but it cannot boot without its EFI entry**.

This means that the **Windows Boot Manager has been deleted or overwritten**, and you must restore it.

---

## **✅ Step 1: Manually Check for Windows Bootloader**
Run:
```bash
ls /mnt/EFI/
```
Expected output:
```
BOOT  Microsoft  void_grub
```
Since `Microsoft` is missing, let's check if Windows still has its bootloader elsewhere:
```bash
find / -name "bootmgfw.efi" 2>/dev/null
```
If the file exists somewhere, we can restore it. If not, we must **reinstall the Windows bootloader**.

---

## **✅ Step 2: Restore Windows Boot Manager from a Windows USB**
If `bootmgfw.efi` is missing, you need a **Windows installation USB** to fix it.

### **1️⃣ Boot into Windows Recovery Mode**
1. **Insert a Windows USB drive** and boot from it.
2. Select **Repair your computer** → **Troubleshoot** → **Command Prompt**.

### **2️⃣ Rebuild the Windows Boot Manager**
Run the following commands:
```cmd
diskpart
list disk
select disk 0
list partition
select partition 1   (Replace 1 with your EFI partition)
assign letter=S
exit
bcdboot C:\Windows /s S: /f UEFI
```
This will **reinstall Windows Boot Manager** onto your EFI partition.

### **3️⃣ Reboot into Linux and Update GRUB**
After restoring the Windows Boot Manager, boot into **Linux** and run:
```bash
sudo os-prober
sudo grub-mkconfig -o /boot/grub/grub.cfg
```
Then **reboot** and check if Windows appears in the GRUB menu.

---

## **🛠 Alternative: Boot Windows Without GRUB**
If you **can't restore the bootloader**, you can try booting Windows manually:

1. **Check available boot entries**:
   ```bash
   sudo efibootmgr
   ```
   If Windows Boot Manager exists, boot into it manually:
   ```bash
   sudo efibootmgr --bootnext 0001
   reboot
   ```
   *(Replace `0001` with the actual Boot ID for Windows.)*

---

## **🎯 Final Summary**
✔ **Check if `Microsoft` exists on EFI (`ls /mnt/EFI/`)**  
✔ **Look for `bootmgfw.efi` (`find / -name "bootmgfw.efi"`)**  
✔ **Use a Windows USB to restore Boot Manager (`bcdboot C:\Windows /s S: /f UEFI`)**  
✔ **Reinstall GRUB (`os-prober` + `grub-mkconfig`)**  
✔ **Use `efibootmgr` to boot Windows manually if needed**  

Try these steps and let me know what happens! 🚀
