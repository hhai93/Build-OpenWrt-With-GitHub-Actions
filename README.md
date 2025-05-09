# Build OpenWrt With GitHub Actions

![GitHub](https://img.shields.io/badge/license-MIT-blue.svg) ![GitHub last commit](https://img.shields.io/github/last-commit/hhai93/Build-OpenWrt-With-GitHub-Actions)

This repository automates building custom OpenWrt firmware using GitHub Actions, inspired by [P3TERX/Actions-OpenWrt](https://github.com/P3TERX/Actions-OpenWrt). It supports scheduled builds, firmware uploads to GitHub Releases, Telegram notifications. The setup is highly customizable and user-friendly.

---

## ✨ Features

-  **Scheduled Builds**: Run builds automatically or manually.
-  **Firmware Upload**: Publishes firmware files to GitHub Releases and Artifacts.
-  **Notifications**: Sends Telegram alerts for new releases.
-  **Cache**: Use cache to reduce time between builds.
-  **Customizable**: Adjust source, branch, config, and DIY scripts easily.
-  **SSH Access**: Interactive `make menuconfig` via SSH for advanced configuration.

---

## ✅ Prerequisites

📋 Ensure you have:

-  A Telegram bot token and chat ID (optional, via [BotFather](https://t.me/BotFather)).
-  A GitHub Personal Access Token with "Contents" and "Workflows" write permission. [Guide](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token).
-  An OpenWrt `.config` file (generate via `make menuconfig` or copy from another source).

---

## 🛠️ Setup Instructions

📝 Follow these steps:

1. **Fork this Repository**

2. **Set GitHub Secrets**:
   -  Go to **Settings > Secrets and variables > Actions > Secrets**.
   -  Add:
     - `RELEASES_TOKEN`: GitHub Personal Access Token.
     - `TELEGRAM_TOKEN`: Telegram bot token.
     - `TELEGRAM_ID`: Telegram chat ID (use [@userinfobot](https://t.me/userinfobot)).

3. **Customize Workflow**:
   - Edit `.github/workflows/build-openwrt.yml`:
     - `REPO_URL`: OpenWrt source (default: `https://github.com/openwrt/openwrt`).
     - `REPO_BRANCH`: Branch (default: `master`).
     - `CONFIG_FILE`: Config file path (default: `.config`).
     - `DIY_P1_SH`/`DIY_P2_SH`: Custom script paths.
     - `TZ`: Timezone (default: `UTC`).
     - `schedule.cron`: Build schedule (default: weekly Sundays, midnight UTC).
   - Modify `diy-part1.sh` (e.g., add feeds) and `diy-part2.sh` (e.g., change IP).

4. **Add Configuration**:
   -  Place `.config` in the repository root. Generate via `make menuconfig` locally or use SSH (see below).
   -  Ensure `.config` matches `REPO_URL` to avoid build failures.

5. **Trigger Build**:
   -  Manually trigger via **Actions > Build OpenWrt > Run workflow**(set `SSH connection to Actions` to `true` for SSH access).
   -  Scheduled builds run per `cron` schedule.

---

## 🔐 SSH Access Setup

🔧 Use SSH to debug or run `make menuconfig` interactively:

1. **Trigger Workflow with SSH**:
   -  In the **Actions**tab, select **Build OpenWrt**and click **Run workflow**.
   -  Set `SSH connection to Actions` to `true` in the form.
   -  Click **Run workflow**.

2. **Get SSH Command**:
   -  Open the running workflow’s **build**job in the **Actions**tab.
   -  Expand the **SSH connection to Actions**step.
   -  Copy the `SSH` command (e.g., `ssh 123abc@ny4.tmate.io`) or note the `Web` URL.

3. **Connect via SSH**:
   -  In a terminal, run the SSH command (e.g., `ssh 123abc@ny4.tmate.io`).
   -  Alternatively, open the `Web` URL (e.g., `https://tmate.io/t/123abc`) in a browser.
   -  Interact with the runner (e.g., `cd openwrt; make menuconfig` after cloning).

4. **Exit SSH**:
   -  Run `exit` to close the SSH session and resume the workflow.
   -  Keep the session open for debugging while the workflow runs (use `detached: true` for background SSH).

**Tips**:
- Press `Ctrl+C` in the log viewer if the SSH command doesn’t appear.
- Save `.config` to `$GITHUB_WORKSPACE` (e.g., `cp openwrt/.config $GITHUB_WORKSPACE/.config`).

---

## ⚠️ Troubleshooting

- **Build Fails**: Check Actions logs. Verify `.config` compatibility with `REPO_URL`. Run `make V=s` locally to debug.
- **No Release**: Ensure `RELEASES_TOKEN` has `public_repo` scope.
- **No Notifications**: Validate `TELEGRAM_TOKEN` and `TELEGRAM_ID`. Test bot manually.
- **Storage Full**: Cleanup retains 30 days of runs and 5 releases. Reduce `retain_days` or `keep_latest` if needed.
- **SSH Issues**: If `tmate` fails, check logs for SSH command or URL.

---

## 🙌 Credits

- **P3TERX/Actions-OpenWrt** for the original template and guide.
