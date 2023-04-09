Install Docker Engine on CentOS 在CentOS上安装Docker引擎
To get started with Docker Engine on CentOS, make sure you meet the prerequisites, then install Docker.
要在CentOS上开始使用Docker Engine，请确保满足先决条件，然后安装Docker。

Prerequisites 先决条件 
OS requirements 操作系统要求 
To install Docker Engine, you need a maintained version of one of the following CentOS versions:
要安装Docker Engine，您需要以下CentOS版本之一的维护版本：

CentOS 7 中央操作系统7
CentOS 8 (stream) CentOS 8（流媒体）
CentOS 9 (stream) CentOS 9（流媒体）
Archived versions aren’t supported or tested.
不支持或测试存档版本。

The centos-extras repository must be enabled. This repository is enabled by default, but if you have disabled it, you need to re-enable it.
必须启用 centos-extras 存储库。默认情况下，此存储库处于启用状态，但如果您已禁用它，则需要重新启用它。

The overlay2 storage driver is recommended.
建议使用 overlay2 存储驱动程序。

Uninstall old versions 卸载旧版本 
Older versions of Docker went by the names of docker or docker-engine. Uninstall any such older versions before attempting to install a new version, along with associated dependencies:
旧版本的Docker被命名为 docker 或 docker-engine 。在尝试安装新版本之前，请卸载任何此类旧版本沿着相关的依赖项：

```bash
 sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```


It’s OK if yum reports that none of these packages are installed.
如果 yum 报告未安装这些软件包，则没有问题。

Images, containers, volumes, and networks stored in /var/lib/docker/ aren’t automatically removed when you uninstall Docker.
卸载Docker时，不会自动删除存储在 /var/lib/docker/ 中的映像、容器、卷和网络。

Installation methods 安装方法 
You can install Docker Engine in different ways, depending on your needs:
您可以根据需要以不同的方式安装Docker Engine：

You can set up Docker’s repositories and install from them, for ease of installation and upgrade tasks. This is the recommended approach.
您可以设置Docker的存储库并从其中安装，以简化安装和升级任务。这是推荐的方法。

You can download the RPM package and install it manually and manage upgrades completely manually. This is useful in situations such as installing Docker on air-gapped systems with no access to the internet.
您可以下载RPM包并手动安装它，也可以完全手动管理升级。这在某些情况下很有用，例如在无法访问Internet的气隙系统上安装Docker。

In testing and development environments, you can use automated convenience scripts to install Docker.
在测试和开发环境中，您可以使用自动化的方便脚本来安装Docker。

Install using the repository
使用存储库  安装
Before you install Docker Engine for the first time on a new host machine, you need to set up the Docker repository. Afterward, you can install and update Docker from the repository.
在新主机上首次安装Docker Engine之前，需要设置Docker存储库。之后，您可以从存储库安装和更新Docker。

Set up the repository 设置存储库
Install the yum-utils package (which provides the yum-config-manager utility) and set up the repository.
安装 yum-utils 包（提供 yum-config-manager 实用程序）并设置存储库。

 

```bash
sudo yum install -y yum-utils
 sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
```


Install Docker Engine 安装Docker引擎
Install Docker Engine, containerd, and Docker Compose:
安装Docker引擎、containerd和Docker组合：

Latest
Specific version 特定版本

To install the latest version, run:
要安装最新版本，请运行：

```bash
 sudo yum install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```


If prompted to accept the GPG key, verify that the fingerprint matches 060A 61C5 1B55 8A7F 742B 77AA C52F EB6B 621E 9F35, and if so, accept it.
如果提示接受GPG密钥，请验证指纹是否与 060A 61C5 1B55 8A7F 742B 77AA C52F EB6B 621E 9F35 匹配，如果匹配，则接受它。

This command installs Docker, but it doesn’t start Docker. It also creates a docker group, however, it doesn’t add any users to the group by default.
此命令安装Docker，但不启动Docker。它还创建了一个 docker 组，但默认情况下不会向该组添加任何用户。

Start Docker. 启动Docker。

```bash
 sudo systemctl start docker
```

Verify that Docker Engine installation is successful by running the hello-world image.
通过运行 hello-world 映像验证Docker Engine安装是否成功。

```bash
 sudo docker run hello-world
```


This command downloads a test image and runs it in a container. When the container runs, it prints a confirmation message and exits.
此命令下载测试映像并在容器中运行它。当容器运行时，它将打印一条确认消息并退出。

You have now successfully installed and started Docker Engine. The docker user group exists but contains no users, which is why you’re required to use sudo to run Docker commands. Continue to Linux postinstall to allow non-privileged users to run Docker commands and for other optional configuration steps.
现在，您已成功安装并启动Docker引擎。Docker用户组存在，但不包含任何用户，这就是为什么需要使用sudo来运行Docker命令。继续Linux安装后配置以允许非特权用户运行Docker命令并执行其他可选配置步骤。

Upgrade Docker Engine 升级Docker引擎
To upgrade Docker Engine, follow the installation instructions, choosing the new version you want to install.
要升级Docker Engine，请按照安装说明操作，选择要安装的新版本。

Install from a package
从包  安装
If you can’t use Docker’s repository to install Docker, you can download the .rpm file for your release and install it manually. You need to download a new file each time you want to upgrade Docker Engine.
如果您无法使用Docker的存储库来安装Docker，您可以下载适用于您的版本的 .rpm 文件并手动安装。每次要升级Docker Engine时，都需要下载新文件。

Go to https://download.docker.com/linux/centos/ and choose your version of CentOS. Then browse to x86_64/stable/Packages/ and download the .rpm file for the Docker version you want to install.
转到 https://download.docker.com/linux/centos/ 并选择您的CentOS版本。然后浏览到 x86_64/stable/Packages/ 并下载要安装的Docker版本的 .rpm 文件。

Install Docker Engine, changing the path below to the path where you downloaded the Docker package.
安装Docker引擎，将下面的路径更改为您下载Docker软件包的路径。


 sudo yum install /path/to/package.rpm
Docker is installed but not started. The docker group is created, but no users are added to the group.
Docker已安装但未启动。已创建 docker 组，但未向该组添加任何用户。

Start Docker. 启动Docker。


 sudo systemctl start docker
Verify that Docker Engine installation is successful by running the hello-world image.
通过运行 hello-world 映像验证Docker Engine安装是否成功。


 sudo docker run hello-world
This command downloads a test image and runs it in a container. When the container runs, it prints a confirmation message and exits.
此命令下载测试映像并在容器中运行它。当容器运行时，它将打印一条确认消息并退出。

You have now successfully installed and started Docker Engine. The docker user group exists but contains no users, which is why you’re required to use sudo to run Docker commands. Continue to Linux postinstall to allow non-privileged users to run Docker commands and for other optional configuration steps.
现在，您已成功安装并启动Docker引擎。Docker用户组存在，但不包含任何用户，这就是为什么需要使用sudo来运行Docker命令。继续Linux安装后配置以允许非特权用户运行Docker命令并执行其他可选配置步骤。

Upgrade Docker Engine 升级Docker引擎
To upgrade Docker Engine, download the newer package file and repeat the installation procedure, using yum -y upgrade instead of yum -y install, and point to the new file.
要升级Docker Engine，请下载较新的程序包文件，使用 yum -y upgrade 而不是 yum -y install 重复安装过程，然后指向新文件。

Install using the convenience script
使用便捷脚本  安装
Docker provides a convenience script at https://get.docker.com/ to install Docker into development environments non-interactively. The convenience script isn’t recommended for production environments, but it’s useful for creating a provisioning script tailored to your needs. Also refer to the install using the repository steps to learn about installation steps to install using the package repository. The source code for the script is open source, and you can find it in the docker-install repository on GitHub.
Docker在 https://get.docker.com/ 处提供了一个方便的脚本，用于以非交互方式将Docker安装到开发环境中。不建议将便捷脚本用于生产环境，但它对于创建适合您需求的预配脚本非常有用。另请参阅使用资料档案库安装的步骤，以了解使用程序包资料档案库进行安装的安装步骤。该脚本的源代码是开源的，您可以在GitHub上的 https://get.docker.com/ 存储库中找到它。

Always examine scripts downloaded from the internet before running them locally. Before installing, make yourself familiar with potential risks and limitations of the convenience script:
在本地运行脚本之前，请始终检查从Internet下载的脚本。安装之前，请熟悉便捷脚本的潜在风险和限制：

The script requires root or sudo privileges to run.
脚本需要 root 或 sudo 权限才能运行。
The script attempts to detect your Linux distribution and version and configure your package management system for you.
该脚本尝试检测您的Linux发行版和版本，并为您配置软件包管理系统。
The script doesn’t allow you to customize most installation parameters.
该脚本不允许您自定义大多数安装参数。
The script installs dependencies and recommendations without asking for confirmation. This may install a large number of packages, depending on the current configuration of your host machine.
脚本安装依赖项和建议，而不要求确认。这可能会安装大量软件包，具体取决于主机的当前配置。
By default, the script installs the latest stable release of Docker, containerd, and runc. When using this script to provision a machine, this may result in unexpected major version upgrades of Docker. Always test upgrades in a test environment before deploying to your production systems.
默认情况下，脚本安装Docker、containerd和runc的最新稳定版本。使用此脚本预配计算机时，可能会导致Docker意外的主要版本升级。在部署到生产系统之前，始终在测试环境中测试升级。
The script isn’t designed to upgrade an existing Docker installation. When using the script to update an existing installation, dependencies may not be updated to the expected version, resulting in outdated versions.
该脚本不用于升级现有Docker安装。使用脚本更新现有安装时，依赖项可能不会更新为预期版本，从而导致版本过期。
Tip: preview script steps before running
提示：运行前预览脚本步骤

You can run the script with the --dry-run option to learn what steps the script will run when invoked:
您可以使用 --dry-run 选项运行脚本，以了解调用脚本时将运行哪些步骤：


 curl -fsSL https://get.docker.com -o get-docker.sh
 sudo sh ./get-docker.sh --dry-run
This example downloads the script from https://get.docker.com/ and runs it to install the latest stable release of Docker on Linux:
此示例从 https://get.docker.com/ 下载脚本并运行它以在Linux上安装Docker的最新稳定版本：


 curl -fsSL https://get.docker.com -o get-docker.sh
 sudo sh get-docker.sh
You have now successfully installed and started Docker Engine. The docker service starts automatically on Debian based distributions. On RPM based distributions, such as CentOS, Fedora, RHEL or SLES, you need to start it manually using the appropriate systemctl or service command. As the message indicates, non-root users can’t run Docker commands by default.
现在，您已成功安装并启动Docker引擎。 docker 服务在基于Debian的发行版上自动启动。在基于 RPM 的发行版上，如CentOS、Fedora、RHEL或SLES，您需要使用适当的 systemctl 或 service 命令手动启动它。如消息所示，默认情况下，非root用户不能运行Docker命令。

Use Docker as a non-privileged user, or install in rootless mode?
作为非特权用户使用Docker，还是以无根模式安装？

The installation script requires root or sudo privileges to install and use Docker. If you want to grant non-root users access to Docker, refer to the post-installation steps for Linux. You can also install Docker without root privileges, or configured to run in rootless mode. For instructions on running Docker in rootless mode, refer to run the Docker daemon as a non-root user (rootless mode).
安装脚本需要 root 或 sudo 权限才能安装和使用Docker。如果要授予非root用户对Docker的访问权限，请参阅Linux的安装后步骤。您也可以在没有 root 权限的情况下安装Docker，或者将Docker配置为在无根模式下运行。有关在无根模式下运行Docker的说明，请参阅以非根用户身份运行Docker守护程序（无根模式）。

Install pre-releases 安装预发行版
Docker also provides a convenience script at https://test.docker.com/ to install pre-releases of Docker on Linux. This script is equal to the script at get.docker.com, but configures your package manager to use the test channel of the Docker package repository. The test channel includes both stable and pre-releases (beta versions, release-candidates) of Docker. Use this script to get early access to new releases, and to evaluate them in a testing environment before they’re released as stable.
Docker还在 https://test.docker.com/ 提供了一个方便的脚本，用于在Linux上安装Docker的预发行版。这个脚本与 get.docker.com 中的脚本相同，但是配置了包管理器以使用Docker包存储库的测试通道。测试通道包括Docker的稳定版和预发行版（beta版本、候选发行版）。使用此脚本可以抢先体验新版本，并在发布为稳定版本之前在测试环境中对其进行评估。

To install the latest version of Docker on Linux from the test channel, run:
要从测试通道在Linux上安装Docker的最新版本，请运行：


 curl -fsSL https://test.docker.com -o test-docker.sh
 sudo sh test-docker.sh
Upgrade Docker after using the convenience script
使用便捷脚本后升级Docker
If you installed Docker using the convenience script, you should upgrade Docker using your package manager directly. There’s no advantage to re-running the convenience script. Re-running it can cause issues if it attempts to re-install repositories which already exist on the host machine.
如果您使用便捷脚本安装了Docker，您应该直接使用包管理器升级Docker。重新运行方便脚本没有任何好处。如果尝试重新安装主机上已存在的存储库，则重新运行它可能会导致问题。

Uninstall Docker Engine 卸载Docker引擎 
Uninstall the Docker Engine, CLI, containerd, and Docker Compose packages:
卸载Docker引擎、CLI、containerd和Docker合成程序包：


 sudo yum remove docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin docker-ce-rootless-extras
Images, containers, volumes, or customized configuration files on your host are not automatically removed. To delete all images, containers, and volumes:
主机上的映像、容器、卷或自定义配置文件不会自动删除。要删除所有映像、容器和卷：


 sudo rm -rf /var/lib/docker
 sudo rm -rf /var/lib/containerd
You must delete any edited configuration files manually.
必须手动删除所有编辑过的配置文件。

Next steps 后续步骤 
Continue to Post-installation steps for Linux.
继续执行Linux的安装后步骤。
Review the topics in Develop with Docker to learn how to build new applications using Docker.
查看使用Docker开发中的主题，了解如何使用Docker构建新应用程序。