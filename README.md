# Whatweb-on-Linux

Here's a distro-agnostic clean install that keeps WhatWeb and its gems self-contained in `/opt/WhatWeb`, with a small launcher in `/usr/local/bin`, so nothing collides with package files again.

**1. Remove every old copy**
```bash
# Remove the package (run whichever matches your distro)
sudo apt purge -y whatweb 2>/dev/null      # Debian/Ubuntu/Kali/Parrot
sudo dnf remove -y whatweb 2>/dev/null     # Fedora/RHEL
sudo pacman -Rns --noconfirm whatweb 2>/dev/null  # Arch/BlackArch

# Remove stray manual installs
sudo rm -f /usr/bin/whatweb /usr/local/bin/whatweb
sudo rm -rf /opt/WhatWeb /usr/share/whatweb
hash -r
```

**2. Install dependencies** (Ruby, headers, git, compiler)
```bash
# Debian / Ubuntu / Kali / Parrot
sudo apt update && sudo apt install -y ruby ruby-dev git build-essential

# Fedora / RHEL / Rocky / Alma
sudo dnf install -y ruby ruby-devel rubygem-bundler git gcc make redhat-rpm-config

# Arch / Manjaro
sudo pacman -S --needed ruby ruby-bundler git base-devel

# openSUSE
sudo zypper install -y ruby ruby-devel git gcc make

# Alpine
sudo apk add ruby ruby-dev ruby-bundler git build-base
```
Then make sure Bundler exists (most modern Ruby versions ship it):
```bash
bundle -v || sudo gem install bundler
```

**3. Clone and install gems locally inside the repo**
```bash
sudo git clone https://github.com/urbanadventurer/WhatWeb.git /opt/WhatWeb
sudo chown -R "$USER": /opt/WhatWeb
cd /opt/WhatWeb
bundle config set --local path vendor/bundle
bundle install
```
Gems land in `/opt/WhatWeb/vendor/bundle`, so they don't touch system Ruby and no `sudo gem` mess builds up.

**4. Create the launcher**
```bash
sudo tee /usr/local/bin/whatweb >/dev/null <<'EOF'
#!/bin/sh
export BUNDLE_GEMFILE=/opt/WhatWeb/Gemfile
exec bundle exec /opt/WhatWeb/whatweb "$@"
EOF
sudo chmod +x /usr/local/bin/whatweb
hash -r
```

**5. Verify**
```bash
which whatweb          # should be /usr/local/bin/whatweb
whatweb --version
whatweb -a 3 https://<target_domain>
```

**Updating later**, no sudo needed:
```bash
cd /opt/WhatWeb && git pull && bundle install
```

**Uninstalling:** `sudo rm -rf /opt/WhatWeb /usr/local/bin/whatweb`

The key point compared with what broke before: never `cp` the `whatweb` script or run `make install`. The script must stay next to its `lib/` and `plugins/` folders, and the launcher just calls it in place.
