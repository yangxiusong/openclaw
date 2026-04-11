### 一、先配置VirtualBox Ubuntu与Windows共享文件夹（必须先做）
#### 1. 安装VirtualBox增强功能（Guest Additions）
1. 启动Ubuntu虚拟机，登录后，在VirtualBox顶部菜单：**设备 → 安装增强功能**
2. 打开终端，执行：
```bash
sudo apt update
sudo apt install -y build-essential dkms linux-headers-$(uname -r)  # 安装编译依赖
sudo mkdir -p /mnt/cdrom
sudo mount /dev/cdrom /mnt/cdrom
cd /mnt/cdrom
sudo ./VBoxLinuxAdditions.run
sudo reboot  # 重启生效
```
3. 重启后，把当前用户加入`vboxsf`组（解决共享文件夹权限）：
```bash
sudo usermod -aG vboxsf $USER
```
**退出当前终端、重新登录Ubuntu**，权限才生效。

#### 2. 设置Windows主机共享目录
1. 关闭Ubuntu虚拟机，在VirtualBox管理器：选中Ubuntu → **设置 → 共享文件夹 → 点击右侧+号（添加共享文件夹）**
2. 配置：
    - **文件夹路径**：选Windows上要共享的目录（如`D:\openclaw_shared`，新建空文件夹）
    - **文件夹名称**：填`openclaw_shared`（英文，Ubuntu里用这个名字）
    - 勾选：**固定分配、自动挂载、只读（取消，要读写）**
    - 确定保存

#### 3. 挂载并验证共享文件夹
1. Ubuntu内创建挂载点（推荐固定路径，方便后续放OpenClaw源码）：
```bash
sudo mkdir -p /mnt/openclaw_shared
```
2. 手动挂载测试：
```bash
sudo mount -t vboxsf openclaw_shared /mnt/openclaw_shared
ls /mnt/openclaw_shared  # 应看到Windows里的文件
```
3. 配置**开机自动挂载**（永久生效）：
```bash
sudo nano /etc/fstab
```
在文件末尾添加一行（替换`你的Ubuntu用户名`，如`ubuntu`）：
```
openclaw_shared /mnt/openclaw_shared vboxsf defaults,uid=你的Ubuntu用户名,gid=你的Ubuntu用户名,umask=0022 0 0
```
保存退出（Ctrl+O → 回车 → Ctrl+X），执行：
```bash
sudo mount -a  # 测试挂载，无报错即成功
```
现在`/mnt/openclaw_shared`就是Windows`D:\openclaw_shared`的共享目录，双向读写。

---

### 二、Ubuntu源码安装OpenClaw（放在共享文件夹，支持更新）
#### 1. 安装依赖（Node.js 22+/pnpm/git）
```bash
sudo apt update
sudo apt install -y git curl
# 安装nvm（管理Node版本）
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
source ~/.bashrc  # 加载nvm
# 安装Node.js 22 LTS（OpenClaw要求≥22）
nvm install 22
nvm use 22
nvm alias default 22
# 安装pnpm（OpenClaw官方推荐）
npm install -g pnpm
```
验证：
```bash
node -v  # 应≥v22.x
pnpm -v
git --version
```

#### 2. 克隆OpenClaw源码到共享文件夹（关键：放共享目录，Windows可直接访问）
```bash
cd /mnt/openclaw_shared  # 进入共享目录
git clone https://github.com/openclaw/openclaw.git  # 克隆源码
cd openclaw
```

#### 3. 编译安装并全局链接
```bash
pnpm install       # 安装所有依赖
pnpm ui:build      # 构建前端界面
pnpm build         # 构建核心程序
pnpm link --global # 全局链接，让openclaw命令全局可用
```

#### 4. 初始化OpenClaw
```bash
openclaw onboard --install-daemon
```
按向导完成配置（选择AI服务商、填API Key等）。

#### 5. 验证安装
```bash
openclaw --version
openclaw doctor  # 检查环境
```

---

### 三、保持OpenClaw更新（源码方式）
因为源码放在**共享文件夹+git仓库**，更新只需拉取最新代码、重新构建：
```bash
# 进入源码目录（共享目录内）
cd /mnt/openclaw_shared/openclaw
# 拉取GitHub最新代码
git pull origin main
# 重新安装依赖、构建
pnpm install
pnpm ui:build
pnpm build
# 重新全局链接（确保命令是最新）
pnpm link --global
# 重启服务
openclaw update
openclaw restart
```
> 也可以直接用`openclaw update`命令自动更新（官方内置更新）。

---

### 四、关键说明&常见问题
1. **共享目录路径固定**：OpenClaw源码、配置、数据都在`/mnt/openclaw_shared/openclaw`，Windows对应`D:\openclaw_shared\openclaw`，可直接编辑、备份。
2. **权限**：必须加入`vboxsf`组，否则Ubuntu无法读写共享目录，编译会失败。
3. **更新**：源码方式每次`git pull`+重新构建即可，比npm安装更灵活，适合需要定制/最新版的场景。
4. 若共享目录无法访问：重启Ubuntu、重新登录、检查`/etc/fstab`、确认用户在`vboxsf`组。

需要我帮你把以上所有命令整理成一键执行的脚本，直接复制到Ubuntu终端运行即可完成全部配置吗？

updatenew