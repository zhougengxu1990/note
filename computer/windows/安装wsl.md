# 安装wsl
安装Windows Subsystem for Linux (WSL) 的步骤如下，分为 **WSL 1** 和 **WSL 2**（推荐使用性能更强的 WSL 2）：

---

### **一、系统要求**
- **Windows 10 版本 2004（Build 19041）或更高**，或 **Windows 11**。
- 确保系统已启用虚拟化功能（需在 BIOS/UEFI 中开启，具体方法因主板型号而异）。

---

### **二、安装 WSL**
#### **方法 1：一键安装（仅限 Windows 10 2004+ 或 Windows 11）**
1. 以管理员身份打开 **PowerShell** 或 **命令提示符**。
2. 运行以下命令自动启用所需功能并安装默认 Linux 发行版（Ubuntu）：
   ```powershell
   wsl --install
   ```
3. 重启计算机，系统会自动完成剩余配置。

---

#### **方法 2：手动安装（适用于所有版本）**
1. **启用 WSL 功能**：
   - 以管理员身份打开 PowerShell，运行：
     ```powershell
     dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
     ```
2. **启用虚拟机平台（WSL 2 必需）**：
   - 继续在 PowerShell 中运行：
     ```powershell
     dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
     ```
   - **重启电脑**。

3. **安装 WSL 2 内核更新包**：
   - 下载并安装：[WSL 2 Linux 内核更新包](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi)
   - 安装完成后，设置 WSL 2 为默认版本：
     ```powershell
     wsl --set-default-version 2
     ```

