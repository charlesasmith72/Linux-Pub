This terminal prompt style is created using **Powerlevel10k** with **Oh My Zsh** or **Zsh**, and a **patched font with powerline icons and color gradients**.

---

## **1️⃣ Components of This Terminal Look**
### **🖌️ Theme: Powerlevel10k (p10k)**
- This **customizes the shell prompt** and provides **icons, colors, and status indicators**.
- The **rainbow segment style** is a signature of Powerlevel10k.

### **💡 Shell: Zsh (with Oh My Zsh or a Custom Setup)**
- Zsh is a feature-rich shell with **plugin support**.
- Oh My Zsh helps manage themes and plugins.

### **🔡 Font: Nerd Font (Powerline Patched)**
- The **rounded segments and icons** come from a **patched Nerd Font** (e.g., MesloLGS NF, Fira Code NF, Hack NF).
- The time icon (`⏲️`) and arrow segments require a font with **powerline glyphs**.

### **🎨 Colors: Custom Powerlevel10k Configuration**
- Uses a **pastel gradient theme** for the separators.
- The `~` symbol represents the **home directory**.

---

## **2️⃣ How to Get This Look in Your Terminal**
### **🛠️ Step 1: Install Zsh**
For Void Linux:
```sh
sudo xbps-install -S zsh
```
For Arch/Velocity Linux:
```sh
sudo pacman -S zsh
```
Then, **make Zsh the default shell**:
```sh
chsh -s $(which zsh)
```
---

### **🛠️ Step 2: Install Oh My Zsh**
Run:
```sh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```
---

### **🛠️ Step 3: Install Powerlevel10k**
```sh
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ~/.oh-my-zsh/custom/themes/powerlevel10k
```
Set **Powerlevel10k** as your Zsh theme by editing `~/.zshrc`:
```sh
ZSH_THEME="powerlevel10k/powerlevel10k"
```
Then, restart Zsh:
```sh
exec zsh
```
---

### **🛠️ Step 4: Install a Nerd Font**
To display **icons and the color separator bar**, install a **patched font** like:
- **MesloLGS NF** (Recommended)
- **FiraCode Nerd Font**
- **Hack Nerd Font**

For Void Linux:
```sh
sudo xbps-install -S font-hack-ttf
```
For Arch/Velocity Linux:
```sh
sudo pacman -S ttf-meslo-nerd
```
Then, **set the font in your terminal emulator (Alacritty, Kitty, Ghostty, etc.).**
---

### **🛠️ Step 5: Configure Powerlevel10k**
Run the configuration wizard:
```sh
p10k configure
```
Select:
✅ **Rainbow style prompt**  
✅ **Rounded separators**  
✅ **Icons for user, time, and path**  

---

## **🔥 Final Result**
After restarting your terminal, it should look like the image you uploaded! 🎨🚀  

Would you like a pre-configured `.zshrc` file with this exact setup? 😊
