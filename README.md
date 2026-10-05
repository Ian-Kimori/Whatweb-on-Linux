# Whatweb-on-Linux

WhatWeb is in the default repos for most security-focused and Debian-based distros:

**Debian / Ubuntu / Kali / Parrot**
```bash
sudo apt update
sudo apt install whatweb
```

**Arch / BlackArch** (it's in the AUR on plain Arch, official repo on BlackArch)
```bash
yay -S whatweb        # Arch via AUR
sudo pacman -S whatweb  # BlackArch
```

**Fedora**
```bash
sudo dnf install whatweb
```

**From source** (latest version, works on any distro with Ruby)
```bash
sudo apt install ruby ruby-dev git build-essential   # or your distro's equivalent
git clone https://github.com/urbanadventurer/WhatWeb.git
cd WhatWeb
sudo gem install bundler
bundle install
./whatweb --version
```
Optionally `sudo make install` to put it on your PATH.

**Verify and test:**
```bash
whatweb --version
whatweb -v example.com          # verbose
whatweb -a 3 example.com        # aggression level 3 (more requests, more fingerprints)
```

If the source build complains about missing gems, running `bundle update` inside the WhatWeb directory usually fixes it.
