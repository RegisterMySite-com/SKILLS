This is almost certainly a broken dependency or shared library issue caused by the LibreOffice uninstall. On KDE Neon / Ubuntu-based systems, removing a package can sometimes pull out libraries or recommended packages that Discover and Dolphin rely on, causing immediate crashes.

Here’s the fastest way to fix it. Open a terminal (Konsole) and run these commands one by one.

### 1. Fix any broken packages first
```bash
sudo apt update
sudo apt --fix-broken install
```

### 2. Reinstall Discover and Dolphin
```bash
sudo apt install --reinstall plasma-discover dolphin
```

If that package name fails, try:
```bash
sudo apt install --reinstall discover dolphin
```

### 3. Reinstall common KDE libraries that often get affected
```bash
sudo apt install --reinstall libkf5* plasma-framework* kde-cli-tools
```

(You can also run a broader one if needed:)
```bash
sudo apt install --reinstall plasma-desktop plasma-workspace
```

### 4. Check for leftover broken packages
```bash
sudo apt autoremove
sudo apt clean
sudo dpkg --configure -a
```

### 5. Test them
After the reinstalls, try launching Discover and Dolphin again from the menu or by typing `plasma-discover` / `dolphin` in the terminal.

---

**If they still crash**, run these and reply with the output so I can see the exact error:

```bash
# Crash log for Dolphin
dolphin 2>&1 | head -50

# Crash log for Discover
plasma-discover 2>&1 | head -50

# Or check system journal for recent crashes
journalctl --user -xe | grep -i -E 'dolphin|discover|plasma' | tail -30
```

Also useful:
```bash
# See what was recently removed
cat /var/log/apt/history.log | tail -100
```

Most of the time the `--reinstall` + `--fix-broken` steps bring Discover and Dolphin back. Let me know what the commands output (especially any error messages) and we’ll dig further.