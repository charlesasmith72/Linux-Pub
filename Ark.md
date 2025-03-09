### **✅ Getting Higher Refresh Rate on Samsung Odyssey Ark (165Hz)**
Your **Samsung Odyssey Ark** supports **up to 165Hz** at **4K (3840x2160)** resolution, but it looks like your system is currently limiting it to **60Hz**. Let's fix that.

---

## **🛠 Step 1: Check Supported Refresh Rates**
Run:
```bash
xrandr | grep -A 1 "Samsung"
```
This will list all supported refresh rates. If you see **165Hz** listed, you can enable it.

---

## **🛠 Step 2: Set 165Hz Refresh Rate**
If **165Hz** is supported, set it manually:
```bash
xrandr --output HDMI-1 --mode 3840x2160 --rate 165
```
If you are using **DisplayPort**, replace `HDMI-1` with `DP-1` or check your display name using:
```bash
xrandr
```

---

## **🛠 Step 3: Ensure Correct Cable and Port**
- **HDMI 2.1 or DisplayPort 1.4** is required for **4K 165Hz**.
- If using **HDMI**, ensure the cable supports **48Gbps bandwidth**.
- Try a different **DisplayPort or HDMI port** on the GPU.

---

## **🛠 Step 4: Add a Custom Mode (If 165Hz Is Not Available)**
If `xrandr` does not list **165Hz**, manually add it:
```bash
cvt 3840 2160 165
```
Copy the output (something like `Modeline "3840x2160_165.00" ...`) and add it:
```bash
xrandr --newmode "3840x2160_165.00" <modeline values>
xrandr --addmode HDMI-1 "3840x2160_165.00"
xrandr --output HDMI-1 --mode "3840x2160_165.00"
```

---

### **🎯 Final Summary**
✔ **Check available refresh rates (`xrandr`)**  
✔ **Set 165Hz manually (`xrandr --output HDMI-1 --mode 3840x2160 --rate 165`)**  
✔ **Use HDMI 2.1 or DisplayPort 1.4 cable for full 165Hz support**  
✔ **Add a custom mode if 165Hz is not listed**  

Try these steps and let me know if you need more help! 🚀
