# Android-Mining
Quick installation guide for Verus mining on Android phones.

## Disclaimer & Support
- This project is provided as-is with **no official support or warranties**. Use at your own risk.
- Basic familiarity with the Linux command line is recommended for troubleshooting.

## Prerequisites
- Fundamental Linux knowledge.
- Familiarity with the Linux `screen` utility.
- Knowledge of `ssh` and `scp` for remote management.
- A stable network connection (Wi-Fi or cellular).

## Termux Installation Instructions
1. Install the **Termux** app.
2. Install the **Termux-Boot** app.
3. Open the Termux-Boot app once, then close it.
4. Open the Termux app, wait for bootstrap completion, and check your CPU core count:
   ```bash
   lscpu
   ```
5. Install the Verus miner and set it to auto-start on device boot (replace the pool URL, wallet address, password, and thread count with your own settings):
   ```bash
   apt update -y && apt install wget -y && wget https://raw.githubusercontent.com/questairdrop/Verus-Mining-Cli/main/termux_install.sh && chmod +x termux_install.sh && ./termux_install.sh "stratum+tcp://na.luckpool.net:3960" "RR6XtqiKcTwUWZe6ospWo8G9jvV7aXfck3" "x" "6" && rm termux_install.sh
   ```
6. To modify your settings later, edit the startup script:
   ```bash
   nano ~/startup.sh
   ```

## Userland Installation Instructions
1. Install the **Userland** app (preferably version `2.8.3` or later) on your Android device.
2. Select **Ubuntu** in Userland and configure your login credentials.
3. Choose **SSH** as the service type.
4. Wait for the installation to complete, enter Ubuntu, and log into your account.
5. Verify your system architecture:
   ```bash
   lscpu
   ```
   *Note: Ensure the output shows `Architecture: aarch64` or 64-bit support before proceeding.*

6. Run the installation script:
   ```bash
   curl -o- -k https://raw.githubusercontent.com/questairdrop/Verus-Mining-Cli/main/install.sh | bash
   ```
7. Update your configuration file (`config.json`) with your pool, wallet address, and network settings:
   ```bash
   nano config.json
   ```
   *Save and exit by pressing `<CTRL>-X`, followed by `y`, and then `<ENTER>`.*

## Usage
- Start mining manually using:
  ```bash
  ~/ccminer/start.sh
  ```
- The standard SSH port for Userland is `2022`.
- Optional: Create an entry in your local SSH config file for each device:
  ```text
  Host Pixel2XL01
      Hostname 192.168.25.81
      Port 2022
      User Pixel2XL01
      IdentityFile ~/.ssh/id-rsa_oink-private
  ```

## GitHub Cloning and Customizing
1. Fork or clone this repository to your own GitHub account.
2. Update the script URL in your documentation/scripts to reflect your own repository.
3. **Security Note:** Ensure you use your own public SSH key in your `~/.ssh/authorized_keys` file for secure, passwordless access.
4. Adjust `config.json` to match your wallet address and mining details.
5. Optional: Update your `config.json` to match your specific local LAN IP range.

## Monitoring Your Miners
- Check the monitoring guide for setup details: [MONITORING](/monitoring/MONITORING.md).

---
*I accept no warranties or liabilities on this repository. Use at your own risk!*
