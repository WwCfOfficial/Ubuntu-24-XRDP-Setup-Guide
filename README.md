# 💻 Ubuntu 24.04 LTS XRDP Setup Guide for VPS

Set up a lightweight desktop environment with **XRDP** on your Ubuntu 24.04 LTS VPS — perfect for GUI apps, web browsing, or development.
Works well even on low-RAM VPS (2 GB–8 GB), especially when combined with a **swap file**.

---

## 📦 Step 1: Update Your VPS

```bash
sudo apt update && sudo apt upgrade -y
```

---

## 🧠 Step 2: (Recommended) Create Swap File (If RAM < 8 GB)

Follow [this Swap Guide](https://github.com/WwCfOfficial/Ubuntu-Swap-Guide) if not already done, or run:

```bash
sudo fallocate -l 8G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
echo 'vm.swappiness=10' | sudo tee -a /etc/sysctl.conf
sudo sysctl -p
```

---

## 💽 Step 3: Install Desktop Environment

We’ll install the lightweight **Xfce** desktop (recommended for VPS):

```bash
sudo apt install xfce4 xfce4-goodies -y
```

---

## 🔌 Step 4: Install XRDP

```bash
sudo apt install xrdp -y
```

---

## 🔐 Step 5: Configure XRDP

1. **Add user to the `ssl-cert` group**:

```bash
sudo usermod -aG ssl-cert $USER
```

2. **Set Xfce as the default session** for XRDP:

```bash
echo "startxfce4" > ~/.xsession
```

Also apply for future users:

```bash
echo "startxfce4" | sudo tee /etc/skel/.xsession
```

---

## 🧽 Step 6: (Fix) Polkit Error & Black Screen Issues

Ubuntu 24.04 may show a black screen or auth errors after login. Fix it:

```bash
echo "allowed_users=anybody" | sudo tee /etc/X11/Xwrapper.config
```

---

## 🚀 Step 7: Enable & Start XRDP Service

```bash
sudo systemctl enable xrdp
sudo systemctl restart xrdp
```

Check status:

```bash
sudo systemctl status xrdp
```

---

## 🌐 Step 8: Allow XRDP Port (3389) on Firewall (Optional)

If UFW is active:

```bash
sudo ufw allow 3389/tcp
```

Then reload or enable UFW (if not already):

```bash
sudo ufw enable       # Enable firewall (if not already)
sudo ufw status       # Confirm 3389 is allowed
```

---

## ✅ Step 9: Connect to XRDP from Your PC

1. Open **Remote Desktop Connection** (Windows) or use **Remmina** (Linux/macOS).
2. Enter your **VPS IP address**.
3. Use the **username/password** of your Ubuntu VPS.
4. Select **Xorg** if asked, and log in.

---

## 🔁 (Optional) Reboot VPS

```bash
sudo reboot
```

---

## 🧪 Troubleshooting

* **Black screen after login?**
  Make sure `/etc/X11/Xwrapper.config` exists with:

```bash
allowed_users=anybody
```

* **No desktop environment?**
  Ensure `xfce4` is installed and `startxfce4` is in `.xsession`.

---

## 🧠 Tips

* Combine with a **swap file** to handle low memory VPS.
* Use `htop` or `top` to monitor resource usage:

```bash
htop
# or
top
```


---

## 🔗 Credits

* Telegram: [https://t.me/WwCfAirdrops](https://t.me/WwCfAirdrops)
* YouTube: [https://youtube.com/@WwCfOfficial](https://youtube.com/@WwCfOfficial)
* Twitter/X: [https://x.com/WwCfOfficial](https://x.com/WwCfOfficial)
