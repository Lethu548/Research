# Using Oracle Linux on WSL

## 1. Install WSL

1. Open **PowerShell as Administrator**.
2. Check whether WSL is installed:

```powershell
wsl --version
```

3. If WSL is not installed, Windows may prompt you to install it automatically.
4. The installation process may take approximately **10 minutes**, depending on your internet connection speed.
5. Restart your computer after the installation is complete.

## 2. Verify the Installation

After rebooting, open PowerShell and run:

```powershell
wsl --version
```

If WSL is installed correctly, version information will be displayed.

In some cases, WSL may guide you through the next steps automatically, such as creating a Unix username and password.

If you need additional help, run:

```powershell
wsl.exe --help
```

This command displays available WSL commands and usage information.

## 3. Enable WSL (If Required)

On some computers, WSL must be enabled manually.

Run:

```powershell
wsl.exe --enable-wsl
```

> Note: Administrator privileges may be required.

## 4. View Available Linux Distributions

To see the distributions available for installation, run:

```powershell
wsl.exe --list --online
```

This command displays all Linux distributions that can be installed through WSL.

## 5. Install a Distribution

To install a specific Linux distribution, use:

```powershell
wsl --install -d <DistroName>
```

Example:

```powershell
wsl --install -d OracleLinux_9_5
```

Replace `<DistroName>` with the distribution name you want to install.

## 6. Create a Unix User Account

After the distribution is installed:

1. Create a Unix username.
2. Create a password for the account.
3. Use these credentials whenever you log into the distribution.

---

# Installing Java on Oracle Linux

## 1. Check if Java Is Installed

Verify whether Java is already installed:

```bash
javac --version
```

If Java is installed, the command will display the installed version.

## 2. View Available JDK Packages

To list available Java Development Kit (JDK) packages:

```bash
sudo dnf list jdk
```

Review the available versions and choose the one you want to install.

## 3. Install a JDK Version

Install your chosen JDK package:

```bash
sudo dnf install <jdk-version>
```

Example:

```bash
sudo dnf install java-21-openjdk-devel
```

Replace `<jdk-version>` with the package name of your selected version.

## 4. Verify the Installation

Check the Java Runtime Environment (JRE):

```bash
java --version
```

Check the Java Compiler (JDK):

```bash
javac --version
```

If both commands return version information, Java has been installed successfully.

## Summary

Common commands used during setup:

```powershell
wsl --version
wsl.exe --help
wsl.exe --enable-wsl
wsl.exe --list --online
wsl --install -d <DistroName>
```

```bash
javac --version
sudo dnf list jdk
sudo dnf install <jdk-version>
java --version
javac --version
```
