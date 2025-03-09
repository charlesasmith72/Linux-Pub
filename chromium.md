### **✅ Step 1: Install Chromium on VX Linux**
To install **Chromium**, run:
```bash
sudo xbps-install -S chromium
```
Once the installation is complete, launch Chromium with:
```bash
chromium
```

---

### **✅ Step 2: (Optional) Set Chromium as Default Browser**
If you want to set Chromium as the default browser, run:
```bash
xdg-settings set default-web-browser chromium.desktop
```
This will make Chromium the default browser when opening links.

---

### **✅ Step 3: (Optional) Enable Hardware Acceleration for Faster Performance**
For better **GPU-accelerated browsing**, run Chromium with these flags:
```bash
chromium --use-gl=egl --enable-features=VaapiVideoDecoder
```
If you have an **AMD GPU**, you can also enable VAAPI hardware acceleration:
```bash
chromium --use-gl=egl --enable-features=VaapiVideoDecoder --disable-features=UseChromeOSDirectVideoDecoder
```

---

### **✅ Step 4: Fix GRUB After Installing Chromium**
After installing Chromium, continue with fixing **GRUB to detect Windows**:

1. **Enable Windows detection**:
   ```bash
   echo 'GRUB_DISABLE_OS_PROBER=false' | sudo tee -a /etc/default/grub
   ```

2. **Detect Windows bootloader**:
   ```bash
   sudo os-prober
   ```

3. **Update GRUB**:
   ```bash
   sudo grub-mkconfig -o /boot/grub/grub.cfg
   ```

4. **Reboot and check if Windows appears in the boot menu**:
   ```bash
   reboot
   ```

---

### **🎯 Final Summary**
✔ **Install Chromium (`sudo xbps-install -S chromium`)**.  
✔ **Run Chromium (`chromium`) and optimize it for performance**.  
✔ **Fix GRUB to detect Windows (`os-prober` + `grub-mkconfig`)**.  
✔ **Reboot and verify the GRUB menu**.  

Try these steps and let me know if you need more help! 🚀
