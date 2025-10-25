# COMP2113/ENGG1340 Module 0 Checkpoint 备考笔记
## 一、提交要求（Submission Requirements）
### 重点内容
- **英文原文**：Please submit a document in the form of .doc/.docx/.pdf to Moodle, with the required TWO screen captures for Task 2.
- **中文译文**：请向 Moodle 提交一份 .doc/.docx/.pdf 格式的文档，文档中需包含 Task 2 要求的两张截图。


## 二、HKUVPN 连接（HKUVPN Connection）
### 1. 必要性
所有后续任务（获取 CS 账户、远程连接服务器）均需先连接 HKUVPN。

### 2. 关键信息
- **英文原文**：
  1. Procedures for making HKUVPN connection can be found at https://its.hku.hk/kb/user-guide-on-making-hkuvpn-connection-with-mfa/.
  2. For any technical issue about connecting to HKUVPN, you should contact HKU ITS (https://www.its.hku.hk/service-desk).
- **中文译文**：
  1. HKUVPN 连接步骤可参考链接：https://its.hku.hk/kb/user-guide-on-making-hkuvpn-connection-with-mfa/。
  2. 若遇到 HKUVPN 连接相关的技术问题，需联系 HKU ITS（链接：https://www.its.hku.hk/service-desk）。


## 三、Task 1：获取 CS 计算机账户（Obtain CS Computer Account）
### 1. 前提条件
连接 HKUVPN 以访问 CS 内网（CS Intranet）。

### 2. 操作步骤（英文原文 + 中文译文）
| 步骤 | 英文原文 | 中文译文 |
|------|----------|----------|
| 1    | Visit CS homepage (http://www.cs.hku.hk). | 访问计算机科学系（CS）主页（http://www.cs.hku.hk）。 |
| 2    | Click “Intranet”. | 点击“Intranet”（内网）选项。 |
| 3    | Click “New Student”. | 点击“New Student”（新学生）选项。 |
| 4    | Log in using your HKU Portal account. | 使用你的 HKU Portal 账户登录。 |
| 5    | After approval, you should be able to see your CS username, password and a door PIN (for accessing nonpublic CS labs) on the page. | 审核通过后，页面将显示你的 CS 用户名、密码以及门禁 PIN 码（用于进入非公共 CS 实验室）。 |

### 3. 提交重点
- **英文原文**：In the document that you will submit, please include your CS username.
- **中文译文**：在你提交的文档中，需包含你的 CS 用户名。


## 四、Task 2：远程访问 CS Ubuntu Linux 服务器（Remote Access to CS Ubuntu Linux Server）
### 前提条件
必须先完成 Task 1 以获取 CS 计算机账户。

---

#### 1. SSH 连接（Command-Line Access）
SSH 提供命令行界面，用于与远程服务器交互，需连接的服务器为主机 `academy11.cs.hku.hk` 或 `academy21.cs.hku.hk`。

##### （1）网络环境差异（Key Difference by Network Environment）
| 网络类型 | 英文原文 | 中文译文 | 核心命令（Code Snippet） |
|----------|----------|----------|--------------------------|
| 外部网络（如家庭网络） | If you are connecting the server from outside the CS network (e.g., from home), you should first make an HKUVPN connection. | 若从 CS 网络外（如家庭）连接服务器，需先建立 HKUVPN 连接。 | ```bash
# 直接连接目标服务器（需先连 HKUVPN）
ssh cs_account@academy11.cs.hku.hk
# 或
ssh cs_account@academy21.cs.hku.hk

| 校园网络（如 HKU WiFi） | If you are connecting the server from campus network (e.g., via HKU WiFi), you should first connect to gatekeeper.cs.hku.hk before connecting to academy11.cs.hku.hk or academy21.cs.hku.hk. (2 levels of login are required.) | 若从校园网络（如 HKU WiFi）连接服务器，需先连接 `gatekeeper.cs.hku.hk`，再连接 `academy11.cs.hku.hk` 或 `academy21.cs.hku.hk`（需两级登录）。 | ```bash
# 第一步：连接 gatekeeper 中转服务器
ssh cs_account@gatekeeper.cs.hku.hk
# 第二步：从 gatekeeper 连接目标服务器
ssh cs_account@academy11.cs.hku.hk
# 或
ssh cs_account@academy21.cs.hku.hk


##### （2）SSH 客户端要求（SSH Client Requirements）
- **英文原文**：
  1. Most of the operating systems come with SSH clients. You may use Command Prompt or Windows PowerShell in Windows 10/11 or Terminal in macOS.
  2. If you are using Windows 10, update the OS to the latest version to obtain the SSH feature. For older versions of Windows, third-party SSH client such as PuTTY (https://www.putty.org/) can be used.
- **中文译文**：
  1. 多数操作系统自带 SSH 客户端：Windows 10/11 可使用 Command Prompt 或 Windows PowerShell，macOS 可使用 Terminal。
  2. Windows 10 用户需将系统更新至最新版本以启用 SSH 功能；旧版 Windows 需使用第三方 SSH 客户端（如 PuTTY，链接：https://www.putty.org/）。

##### （3）登录后操作（Post-Login Operations）
- **密码输入提示**：
  - **英文原文**：When typing your password, it is normal that your password is not being shown. Press Enter when finished typing.
  - **中文译文**：输入密码时，密码不显示是正常现象，输入完成后按 Enter 键即可。
- **Terminal type 提示**：
  - **英文原文**：After login, it might ask you “Terminal type?”. Just press Enter to bypass the message.
  - **中文译文**：登录后可能会提示“Terminal type?”，直接按 Enter 键跳过即可。
- **验证命令与截图要求**：
  - **英文原文**：Try entering the first Linux command `date`, what can you see? Take a screenshot of your command-line window and attach it to the document.
  - **中文译文**：尝试输入第一个 Linux 命令 `date`，观察输出结果。将命令行窗口截图并附在提交文档中。
  - **核心命令**：
    ```bash
    # 查看当前日期与时间
    date
    ```

---

#### 2. SFTP 连接（File Transfer）
SFTP 用于本地计算机与远程服务器间的文件传输，服务器主机名为 `sftp://academy.cs.hku.hk`。

##### （1）网络访问要求
- **英文原文**：Similar to SSH, it does not allow direct access from a computer outside the CS Network. If you are connecting the server from outside the CS network (e.g., from home), you should first make an HKUVPN connection.
- **中文译文**：与 SSH 类似，CS 外部网络无法直接访问 SFTP 服务器。若从外部网络（如家庭）连接，需先建立 HKUVPN 连接。

##### （2）FileZilla 操作步骤（推荐工具）
| 步骤 | 英文原文 | 中文译文 | 关键配置信息 |
|------|----------|----------|--------------|
| 1    | Download and install FileZilla Client (https://filezilla-project.org/). Be careful: The installer may include options for installing other sponsored software. Uncheck or decline them if you don’t want to install those software. | 下载并安装 FileZilla Client（链接：https://filezilla-project.org/）。注意：安装程序可能包含其他赞助软件的安装选项，若不需要请取消勾选或拒绝。 | - 下载链接：https://filezilla-project.org/ |
| 2    | Open FileZilla, fill in the following information and click “Quickconnect”: Host: sftp://academy.cs.hku.hk; Username: [your_CS_account]; Password: [your_CS_account_password]; Port: Leave it empty. | 打开 FileZilla，填写以下信息并点击“Quickconnect”：主机（Host）：sftp://academy.cs.hku.hk；用户名（Username）：[你的 CS 账户]；密码（Password）：[你的 CS 账户密码]；端口（Port）：留空。 | ```
Host: sftp://academy.cs.hku.hk
Username: 你的 CS 用户名（如 twchim）
Password: 你的 CS 账户密码
Port: （空）
 3    | After connection, the window on the right side shows your home directory on the academy server. You can now drag and drop files between your local computer and the server. If the connection fails, check if you have connected to HKUVPN. | 连接成功后，右侧窗口将显示你在 academy 服务器上的主目录（home directory）。此时可通过拖拽文件实现本地与服务器间的文件传输。若连接失败，请检查是否已连接 HKUVPN。 | - 右侧窗口：远程服务器主目录<br>- 左侧窗口：本地计算机目录 |

##### （3）截图要求
- **英文原文**：Do not forget to take a screenshot of your home directory and attach it to the document.
- **中文译文**：务必将服务器主目录（home directory）截图并附在提交文档中。


## 五、核心考点总结（Key Exam/Checkpoint Focus）
1. **HKUVPN 必要性**：所有 CS 服务器访问（账户申请、SSH/SFTP）均需先连 HKUVPN。
2. **CS 账户信息**：需记录用户名、密码（提交文档需包含用户名）。
3. **SSH 命令与网络差异**：
   - 外部网络：`ssh cs_account@academy11.cs.hku.hk`（需 HKUVPN）
   - 校园网络：先连 gatekeeper，再连 academy 服务器
   - 密码输入不显示、`date` 命令与截图
4. **SFTP 配置**：
   - 主机：`sftp://academy.cs.hku.hk`
   - FileZilla 填写项（Host/Username/Password/Port）
   - 主目录截图
5. **提交格式**：.doc/.docx/.pdf + 2 张截图（SSH 命令行、SFTP 主目录）