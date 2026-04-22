**🏰 Bazzite-Fortress**

A custom, slightly opinionated version of Bazzite and Fedora Silverblue. Designed for power users, Bazzite-Fortress was created for the stability of a cloud-native, immutable home server.

Built autonomously via GitHub Actions using BlueBuild.

**📖 The Philosophy**

The goal of the Fortress is simple: Zero Maintenance. 
By baking third-party repositories and systemd services into the OS image, the operating system manages its own dependencies. If a hard drive fails or you buy a new computer, you can recreate your exact media server - including Cloudflare tunnels - in a single command, in under 10 minutes.

**✨ Features & Arsenal**

This image starts with the `bazzite-dx-gnome` base and layers on a customized stack:

**☁️ Storage & Networking (Headless)**

Native Cloudflared: Bakes in the official Cloudflare RPM to run your zero-trust domain tunnels headlessly at boot.

Cloud Sync: Natively integrates `rclone` (for 10TB mounts) and `jotta-cli` (official RPM) for background syncing.

**⚙️ Baked-In Systemd Automation**

The OS image natively enables the following services on installation:

System (Headless): 
`cloudflared.service`, `rclone-mounts.service`

User    (Wayland): 
`jottad.service`

**🚀 Installation**

To rebase an existing atomic Fedora/Bazzite installation to bazzite-fortress:

1. Rebase to the Unverified Image

First, you must pull the unsigned image so your system can download and install the proper cryptographic signing keys and policies:

`sudo rpm-ostree rebase ostree-unverified-registry:ghcr.io/dcoffline/bazzite-fortress:latest`

2. Reboot

Reboot your machine to complete the initial rebase and boot into the Fortress:

`systemctl reboot`

3. Lock the Gates (Enforce Signatures)

Once rebooted, rebase one final time to the verified, signed image. This ensures all future background updates are cryptographically verified for security:

`sudo rpm-ostree rebase ostree-image-signed:docker://ghcr.io/dcoffline/bazzite-fortress:latest`

`systemctl reboot`

**🛡️ Verification**

These images are signed with Sigstore's Cosign. Because the signing module is active in the recipe, your system will automatically verify the image integrity during every future background update. If the cryptographic keys do not match, the system will refuse the update, ensuring a compromised image never boots on your hardware.
