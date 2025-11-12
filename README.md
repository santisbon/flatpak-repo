# Santisbon Flatpak Repository

A self-hosted Flatpak repository for distributing custom Linux applications with automatic updates.

**Repository URL**: https://flatpak.santisbon.me/

## About This Repository

This repository hosts an OSTree-based Flatpak repository containing multiple applications. Users can add this repository to their Flatpak installation and receive automatic updates.

## Available Applications

- **iCloud Services** (`me.santisbon.iCloudServices`) - Access all your iCloud web services on Linux with native desktop integration
  - [Source Code](https://github.com/santisbon/icloud-flatpak)

## Installation Instructions

### One-Time Setup (Command Line)

1. **Install Flatpak**:
   ```bash
   sudo apt install flatpak  # Debian/Ubuntu
   sudo dnf install flatpak  # Fedora
   ```

2. **Add Flathub** (required for dependencies):
   ```bash
   flatpak remote-add --user --if-not-exists flathub https://dl.flathub.org/repo/flathub.flatpakrepo
   ```

3. **Add Santisbon Apps Repository**:
   ```bash
   flatpak remote-add --user --if-not-exists santisbon-apps https://flatpak.santisbon.me/santisbon-apps.flatpakrepo
   ```

4. **Browse Available Apps**:
   ```bash
   flatpak remote-ls santisbon-apps
   ```

### Installing Applications

**Example: iCloud Services**

```bash
# Install prerequisites
flatpak install --user flathub org.chromium.Chromium

# Install iCloud Services
flatpak install --user santisbon-apps me.santisbon.iCloudServices

# Launch an iCloud service
flatpak run me.santisbon.iCloudServices mail
flatpak run me.santisbon.iCloudServices drive
flatpak run me.santisbon.iCloudServices photos
```

### Updates

Applications installed from this repository receive automatic updates:

```bash
flatpak update
```

### Uninstall

```bash
# Remove a specific application
flatpak uninstall me.santisbon.iCloudServices

# Remove the repository
flatpak remote-delete santisbon-apps

# Clean up unused dependencies
flatpak uninstall --unused
```

## For Developers

### Repository Structure

```
flatpak-repo/
├── repo/                           # OSTree repository
│   ├── objects/                    # Application objects
│   ├── refs/                       # Application references
│   ├── deltas/                     # Static deltas for efficient updates
│   └── summary                     # Repository summary (GPG signed)
├── santisbon-apps.flatpakrepo      # Repository configuration file
├── index.html                      # Repository website
└── .gitignore                      # Git ignore rules
```

### Adding Applications to This Repository

This repository can host multiple applications. To add a new application:

```bash
# 1. Build your application and add to this repository
cd ~/code/your-application
flatpak-builder \
  --arch=x86_64 \
  --repo=~/code/flatpak-repo/repo \
  --gpg-sign=YOUR_GPG_KEY_ID \
  --force-clean \
  build-x86_64 \
  your.app.manifest.yaml

# 2. Update repository summary
flatpak build-update-repo \
  --generate-static-deltas \
  --gpg-sign=YOUR_GPG_KEY_ID \
  ~/code/flatpak-repo/repo

# 3. Commit and push
cd ~/code/flatpak-repo
git add repo/
git commit -m "Add new application"
git push origin main
```

**Note**: This repository does not require Git LFS. The iCloud Services app is lightweight (total repo size ~1.6 MB) and all files are well below GitHub's size limits.

### GitHub Pages Hosting

This repository is hosted on GitHub Pages:

- **Website**: https://flatpak.santisbon.me/
- **Repository URL**: https://flatpak.santisbon.me/repo
- **Configuration**: https://flatpak.santisbon.me/santisbon-apps.flatpakrepo

To enable GitHub Pages:
1. Go to repository Settings → Pages
2. Source: Deploy from a branch
3. Branch: main, Folder: / (root)
4. Click Save

## Security

### GPG Signing

All commits to this repository are signed with GPG to ensure integrity. The GPG public key is embedded in the `santisbon-apps.flatpakrepo` file.

### HTTPS Only

This repository is served over HTTPS via GitHub Pages, which is required for Flatpak repositories.

## Support

- **Issues**: Report issues with specific applications in their respective repositories
- **Repository Issues**: [Open an issue](https://github.com/santisbon/flatpak-repo/issues)

## Resources

- [Flatpak Documentation](https://docs.flatpak.org/)
- [OSTree Documentation](https://ostreedev.github.io/ostree/)
- [Hosting a Flatpak Repository](https://docs.flatpak.org/en/latest/hosting-a-repository.html)
- [GitHub Pages](https://docs.github.com/en/pages)

## License

This repository configuration is released under the GNU GPL v3 License. Individual applications have their own licenses - see their respective repositories for details.
