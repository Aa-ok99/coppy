# coppy
Below the coppy lay. 🦋💯 

# 🚀 Termux Full Setup Script

สคริปต์ติดตั้ง **Termux** ให้พร้อมใช้งานแบบ All-in-One  
ประกอบด้วย:

- 🐚 **Zsh** + **Powerlevel10k** (สวยงาม ใช้งานง่าย)
- 🔌 **zsh-autosuggestions**, **zsh-syntax-highlighting**
- 📝 **Neovim** + **NvChad** (Mason only mode)
- 📂 **Alias** สำหรับเข้าถึง Storage ได้สะดวก
- 🔧 **Mason fix script** สำหรับ Termux

---

## 📥 วิธีใช้งาน

### 1. ติดตั้ง Termux (ถ้ายังไม่มี)
ดาวน์โหลดจาก [F-Droid](https://f-droid.org/repo/com.termux_118.apk) หรือ Google Play

### 2. วางสคริปต์ลงใน Termux

bash
#!/data/data/com.termux/files/usr/bin/bash

# ============================================
# Termux Full Setup Script
# ZSH + Powerlevel10k + Auto-suggestions + NvChad (Mason only)
# ============================================

echo "🚀 เริ่มต้นติดตั้ง Termux Environment..."

# 1. อัปเดตระบบและติดตั้ง packages พื้นฐาน
echo "📦 อัปเดตระบบและติดตั้ง packages..."
pkg update -y && pkg upgrade -y
pkg install -y git zsh neovim which termux-api termux-services

# 2. ตั้งค่า storage permission
echo "📂 ขออนุญาตเข้าถึง Storage..."
termux-setup-storage
sleep 2

# 3. ตั้ง Zsh เป็น shell หลัก
echo "🐚 ตั้งค่า Zsh เป็น Default Shell..."
chsh -s zsh

# 4. ดาวน์โหลดและติดตั้ง Oh My Zsh
echo "📥 ติดตั้ง Oh My Zsh..."
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)" "" --unattended

# 5. ติดตั้ง Powerlevel10k theme
echo "🎨 ติดตั้ง Powerlevel10k theme..."
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k

# 6. ติดตั้ง Zsh plugins (auto-suggestions + syntax-highlighting)
echo "🔌 ติดตั้ง Zsh plugins..."
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting

# 7. แก้ไข .zshrc ให้ใช้ theme และ plugins
echo "⚙️ กำหนดค่า .zshrc..."
sed -i 's/ZSH_THEME="robbyrussell"/ZSH_THEME="powerlevel10k\/powerlevel10k"/' ~/.zshrc
sed -i 's/plugins=(git)/plugins=(git zsh-autosuggestions zsh-syntax-highlighting)/' ~/.zshrc

# 8. เพิ่ม alias ให้ git clone ไปที่ shared folder
echo "📁 สร้าง Project folder และเพิ่ม alias..."
mkdir -p ~/storage/shared/Projects
echo 'alias my="cd ~/storage/shared/Projects"' >> ~/.zshrc
echo 'alias gclonemyp="cd ~/storage/shared/Projects && git clone"' >> ~/.zshrc

# 9. ติดตั้ง NvChad (เฉพาะ Mason โดยไม่สร้าง custom config)
echo "📝 ติดตั้ง NvChad (Mason only mode)..."


# 10. ติดตั้ง Mason fix script สำหรับ Termux
echo "🔧 ติดตั้ง Mason fix script..."
pkg install which -y
curl -o /data/data/com.termux/files/usr/bin/install-in-mason https://raw.githubusercontent.com/Amirulmuuminin/setup-mason-for-termux/main/install-in-mason
chmod +x /data/data/com.termux/files/usr/bin/install-in-mason

echo ""
echo "=========================================="

rm -rf ~/.config/nvim
rm -rf ~/.local/state/nvim
rm -rf ~/.local/share/nvim

#!/data/data/com.termux/files/usr/bin/bash

# สคริปต์ติดตั้ง LazyVim บน Termux
echo "กำลังติดตั้ง LazyVim Starter บน Termux..."

# อัปเดตและอัปเกรดแพ็คเกจ
pkg update -y && pkg upgrade -y

# ติดตั้ง dependencies
pkg install -y git neovim nodejs python ripgrep fd npm ollama curl lsd 

# ลบ config เก่าถ้ามี
NVIM_DIR="$HOME/.config/nvim"
if [ -d "$NVIM_DIR" ]; then
  echo "พบ config เดิม จะทำการสำรองข้อมูล..."
  mv "$NVIM_DIR" "$NVIM_DIR.backup.$(date +%s)"
fi
npm install -g @aa-ok99/ants
npm i -g @devcorex/dev.x

# ติดตั้ง plugins (ผ่าน Lazy.nvim ที่มากับ Starter)
echo "กำลังติดตั้ง plugins..."
nvim --headless "+Lazy! sync" +qa

# ติดตั้ง LSP servers (ผ่าน Mason)
echo "กำลังติดตั้ง LSP servers..."
nvim --headless "+MasonInstallAll" +qa

# ติดตั้ง Treesitter parsers พื้นฐาน
echo "กำลังติดตั้ง Treesitter parsers..."
nvim --headless "+TSInstall lua python bash javascript typescript" +qa

# สร้าง alias
echo "alias vim='nvim'" >> ~/.bashrc
echo "alias nvim='nvim'" >> ~/.bashrc
source ~/.bashrc

echo "การติดตั้งเสร็จสมบูรณ์!"
echo "ใช้คำสั่ง 'nvim' เพื่อเริ่มใช้งาน"


git clone https://github.com/NvChad/starter ~/.config/nvim

echo "✅ ติดตั้งเสร็จสมบูรณ์!"
echo "=========================================="
echo ""
echo "📌 ขั้นตอนต่อไป:"
echo "1. รีสตาร์ท Termux (ปิดแล้วเปิดใหม่)"
echo "2. ตั้งค่า Powerlevel10k: พิมพ์ 'p10k configure'"
echo "3. เปิด Neovim ครั้งแรก: พิมพ์ 'nvim' (กด n เมื่อถาม chadrc template)"
echo "4. ใช้ Mason ติดตั้ง LSP: :MasonInstall# แทนการติดตั้งผ่าน Mason โดยตรง ให้ใช้:
install-in-mason lua-language-server
install-in-mason pyright
install-in-mason typescript-language-server
# หรือตัวอื่นๆ ตามต้องการ แล้วเลือกตัวที่ต้องการ"
echo ""
echo "📂 Project folder อยู่ที่: ~/storage/shared/Projects"
echo "🐚 คำสั่งลัด: 'my' = ไปที่ Project folder"
echo "🐚 'gclonemyp' = git clone ใน Project folder"
echo "=========================================="
