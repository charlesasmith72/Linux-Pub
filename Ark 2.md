To add support for your Samsung Odyssey Ark monitor and enable 165Hz refresh rate on Linux, follow these steps:

⸻

1. Check Available Refresh Rates

Run the following command to list available resolutions and refresh rates:

xrandr

Look for 3840x2160 resolution and check if 165Hz is listed. If it’s missing, you will need to manually add it.

⸻

2. Add a Custom Mode for 165Hz

If 165Hz is not available, you can manually add it using xrandr and cvt:
	1.	Generate a modeline for 3840x2160 @ 165Hz:

cvt 3840 2160 165

The output will look something like:

# 3840x2160 165.00 Hz (CVT) hsync: 368.10 kHz; pclk: 1417.00 MHz
Modeline "3840x2160_165.00" 1417.00 3840 4008 4400 4960 2160 2163 2173 2222 -hsync +vsync


	2.	Add this mode to xrandr:

xrandr --newmode "3840x2160_165.00" 1417.00 3840 4008 4400 4960 2160 2163 2173 2222 -hsync +vsync


	3.	Attach the mode to your display (replace HDMI-1-1 with your actual display name from xrandr output):

xrandr --addmode HDMI-1-1 3840x2160_165.00


	4.	Apply the new refresh rate:

xrandr --output HDMI-1-1 --mode 3840x2160_165.00 --rate 165



⸻

3. Ensure Proper Cable & Port
	•	If using HDMI, verify that your cable supports 48Gbps bandwidth (HDMI 2.1).
	•	If possible, use DisplayPort 1.4 or a different HDMI port on your GPU for higher refresh rates.

⸻

4. Make It Persistent After Reboot

By default, xrandr settings reset after reboot. To apply the changes permanently:
	1.	Edit your Xorg configuration:

sudo nano /etc/X11/xorg.conf.d/10-monitor.conf


	2.	Add the following (modify HDMI-1-1 if needed):

Section "Monitor"
    Identifier "HDMI-1-1"
    Modeline "3840x2160_165.00" 1417.00 3840 4008 4400 4960 2160 2163 2173 2222 -hsync +vsync
    Option "PreferredMode" "3840x2160_165.00"
EndSection


	3.	Save (Ctrl + X, then Y, then Enter) and reboot.

⸻

Final Summary

✔ Check available refresh rates → xrandr
✔ Set 165Hz manually → xrandr --newmode and --addmode
✔ Ensure HDMI 2.1 or DisplayPort 1.4 is used
✔ Make settings permanent by editing /etc/X11/xorg.conf.d/

After applying these settings, test if your Samsung Odyssey Ark now runs at 165Hz. Let me know if you need further troubleshooting!
