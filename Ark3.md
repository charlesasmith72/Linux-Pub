Linux typically doesn't require a proprietary "monitor driver" like you might find for Windows. Instead, your display settings—including available refresh rates—are determined by the graphics driver (whether it's open‑source or proprietary), the display server (Xorg or Wayland), and the EDID data your monitor provides.

If you're not seeing 165 Hz as an option on your Samsung Ark, consider the following:

1. **EDID Limitations:**  
   Your monitor sends EDID information to your system, which might not be advertising 165 Hz. This isn’t a driver issue per se, but rather how the monitor is reporting its capabilities.

2. **GPU and Cable Support:**  
   Ensure that your laptop’s GPU supports 165 Hz at your desired resolution and that you’re using a cable/port (e.g., DisplayPort or a high-bandwidth HDMI) that can handle that refresh rate.

3. **Forcing a Custom Mode with xrandr:**  
   You can try manually adding a 165 Hz mode. For example, if you’re using a 2560×1440 resolution, you could generate a modeline with a command like:  
   ```bash
   cvt 2560 1440 165
   ```  
   This might output a modeline similar to:  
   ```
   Modeline "2560x1440_165.00"  446.50  2560 2744 3024 3488  1440 1443 1453 1496 -hsync +vsync
   ```  
   Then add the mode and assign it to your output (replace `HDMI-1` with your actual output identifier):  
   ```bash
   xrandr --newmode "2560x1440_165.00"  446.50  2560 2744 3024 3488  1440 1443 1453 1496 -hsync +vsync
   xrandr --addmode HDMI-1 "2560x1440_165.00"
   xrandr --output HDMI-1 --mode "2560x1440_165.00"
   ```  
   This forces your system to use the custom mode, assuming your hardware supports it.

4. **Driver Updates:**  
   Verify that your GPU drivers are up to date. On Linux, the display capabilities depend largely on the GPU driver (like `nvidia`, `amdgpu`, or `intel` drivers) rather than a monitor-specific driver.

In summary, there isn’t a separate driver for your Samsung Ark on Linux. Instead, you may need to tweak your system’s settings (such as adding a custom mode with xrandr) and ensure all your hardware and drivers support the desired 165 Hz refresh rate.
