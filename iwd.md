# iwd

From ArchWiki

[iwd](https://iwd.wiki.kernel.org/) (iNet wireless daemon) is a wireless daemon for Linux written by Intel. The core goal of the project is to optimize resource utilization by not depending on any external libraries and instead utilizing features provided by the Linux Kernel to the maximum extent possible.

Iwd (iNet 无线守护进程)是由 Intel 编写的用于 Linux 的无线守护进程。该项目的核心目标是通过不依赖任何外部库，而是最大限度地利用 Linux 内核提供的特性来优化资源利用率。

iwd can work in standalone mode or in combination with comprehensive network managers like [ConnMan](https://wiki.archlinux.org/index.php/ConnMan "ConnMan"), [systemd\-networkd](https://wiki.archlinux.org/index.php/Systemd-networkd "Systemd-networkd") and [NetworkManager](https://wiki.archlinux.org/index.php/NetworkManager#Using_iwd_as_the_Wi-Fi_backend "NetworkManager").

Iwd 可以在独立模式下工作，也可以与像 ConnMan、 systemd\-networkd 和 NetworkManager 这样的综合网络管理器组合使用。

## Contents

## 内容

*   [1 Installation 安装](#Installation)
*   [2 Usage 用法](#Usage)
    *   [2.1 iwctl 国际文化遗产协会](#iwctl)
        *   [2.1.1 Connect to a network 连接到网络](#Connect_to_a_network)
        *   [2.1.2 Connect to a network using WPS/WSC 使用 WPS/WSC 连接到网络](#Connect_to_a_network_using_WPS/WSC)
        *   [2.1.3 Disconnect from a network 断开与网络的连接](#Disconnect_from_a_network)
        *   [2.1.4 Show device and connection information 显示设备和连接信息](#Show_device_and_connection_information)
        *   [2.1.5 Manage known networks 管理已知的网络](#Manage_known_networks)
*   [3 Network configuration 网络配置](#Network_configuration)
    *   [3.1 WPA\-PSK](#WPA-PSK)
    *   [3.2 WPA Enterprise 深圳市万邦进出口有限公司](#WPA_Enterprise)
        *   [3.2.1 EAP\-PWD](#EAP-PWD)
        *   [3.2.2 EAP\-PEAP](#EAP-PEAP)
        *   [3.2.3 TTLS\-PAP](#TTLS-PAP)
        *   [3.2.4 Eduroam](#Eduroam)
        *   [3.2.5 Other cases 其他个案](#Other_cases)
*   [4 Optional configuration 可选配置](#Optional_configuration)
    *   [4.1 Disable auto\-connect for a particular network 禁用特定网络的自动连接](#Disable_auto-connect_for_a_particular_network)
    *   [4.2 Disable periodic scan for available networks 禁用可用网络的周期性扫描](#Disable_periodic_scan_for_available_networks)
    *   [4.3 Enable built\-in network configuration 启用内置网络配置](#Enable_built-in_network_configuration)
        *   [4.3.1 IPv6 support 支援 IPv6](#IPv6_support)
        *   [4.3.2 Setting static IP address in network configuration 在网络配置中设置静态 IP 地址](#Setting_static_IP_address_in_network_configuration)
        *   [4.3.3 Select DNS manager 选择 DNS 管理器](#Select_DNS_manager)
    *   [4.4 Deny console (local) user from modifying the settings 拒绝控制台(本地)用户修改设置](#Deny_console_(local)_user_from_modifying_the_settings)
*   [5 Troubleshooting 故障排除](#Troubleshooting)
    *   [5.1 Verbose TLS debugging 详细 TLS 调试](#Verbose_TLS_debugging)
    *   [5.2 Connect issues after reboot 重新启动后连接问题](#Connect_issues_after_reboot)
    *   [5.3 Wireless device is not renamed by udev Udev 不会重命名无线设备](#Wireless_device_is_not_renamed_by_udev)
*   [6 See also 参见](#See_also)

## Installation 安装

[Install](https://wiki.archlinux.org/index.php/Install "Install") the [iwd](https://archlinux.org/packages/?name=iwd) package.

安装 iwd 包。

## Usage 用法

The [iwd](https://archlinux.org/packages/?name=iwd) package provides the client program `iwctl`, the daemon `iwd` and the Wi\-Fi monitoring tool `iwmon`.

Iwd 包提供了客户机程序 iwctl、守护进程 iwd 和 Wi\-Fi 监控工具 iwmon。

[Start/enable](https://wiki.archlinux.org/index.php/Start/enable "Start/enable") `iwd.service` so it can be controlled using the `iwctl` command.

启动/启用 iwd.service，以便可以使用 iwctl 命令控制它。

### iwctl 国际文化遗产协会

To get an interactive prompt do:

要得到一个交互式提示符，请做:

$ iwctl

The interactive prompt is then displayed with a prefix of `[iwd]#`.

然后显示交互式提示符，前缀为\[ iwd \] # 。

**Tip: 提示:**

*   In the 在`iwctl` prompt you can auto\-complete commands and device names by hitting 提示你可以自动完成命令和设备名称的命令和点击`Tab`.
*   To exit the interactive prompt, send 要退出交互式提示符，请发送[EOF 电子自旋共振](https://en.wikipedia.org/wiki/EOF_character "wikipedia:EOF character") by pressing 通过压榨`Ctrl+d`.
*   You can use all commands as command line arguments without entering an interactive prompt. For example: 您可以使用所有命令作为命令行参数，而无需输入交互式提示符。例如:`iwctl device wlp3s0 show`.

To list all available commands:

列出所有可用的命令:

```
[iwd]# help
```

#### Connect to a network 连接到网络

First, if you do not know your wireless device name, list all Wi\-Fi devices:

首先，如果你不知道你的无线设备名称，列出所有的 Wi\-Fi 设备:

```
[iwd]# device list
```
Then, to scan for networks:

然后，扫描网络:

```
[iwd]# station *device* scan
```
You can then list all available networks:

然后你可以列出所有可用的网络:

```
[iwd]# station *device* get\-networks
```
Finally, to connect to a network:

最后，连接到一个网络:

```
[iwd]# station *device* connect *SSID*

**Tip: 提示:** The user interface supports autocomplete, by typing 用户界面支持自动完成，输入`station` and 及`Tab` `Tab`, the available devices are displayed, type the first letters of the device and ，显示可用的设备，键入设备的第一个字母和`Tab` to complete. The same way, type 同样的方式，输入`connect` and 及`Tab` `Tab` in order to have the list of available networks displayed. Then, type the first letters of the chosen network followed by 以便显示可用网络的列表。然后，键入所选网络的第一个字母，接着是`Tab` in order to complete the command. 才能完成指令

If a passphrase is required, you will be prompted to enter it. Alternatively, you can supply it as a command line argument:

如果需要密码短语，系统会提示您输入它。或者，你可以提供一个命令行参数:

$ iwctl \-\-passphrase *passphrase* station *device* connect *SSID*

**Note: 注意:**

*   `iwd` automatically stores network passphrases in the 自动存储网络密码在`/var/lib/iwd` directory and uses them to auto\-connect in the future. See 目录，并使用它们在将来自动连接[#Network configuration # 网络配置](#Network_configuration).
*   To connect to a network with spaces in the SSID, the network name should be double quoted when connecting. 要连接到 SSID 中有空格的网络，连接时网络名称应该使用双引号
*   iwd only supports PSK pass\-phrases from 8 to 63 ASCII\-encoded characters. The following error message will be given if the requirements are not met: Iwd 只支持8到63个 ascii 编码字符的 PSK 密钥短语。如果没有满足要求，将给出以下错误消息:`PMK generation failed. Ensure Crypto Engine is properly configured`.

#### Connect to a network using WPS/WSC 使用 WPS/WSC 连接到网络

If your network is configured such that you can connect to it by pressing a button ([Wikipedia:Wi\-Fi Protected Setup](https://en.wikipedia.org/wiki/Wi-Fi_Protected_Setup "wikipedia:Wi-Fi Protected Setup")), check first that your network device is also capable of using this setup procedure.

如果你的网络被配置成你可以通过按下一个按钮连接到它(Wikipedia: google Wi\-Fi保护设置) ，首先检查你的网络设备是否也能够使用这个设置过程。

```
[iwd]# wsc list

Then, provided that your device appeared in the above list,

然后，假设你的设备出现在上面的列表中,

```
[iwd]# wsc *device* push\-button

and push the button on your router. The procedure works also if the button was pushed beforehand, less than 2 minutes earlier.

按下路由器上的按钮。如果事先按下按钮，这个过程也会起作用，提前不到2分钟。

If your network requires to validate a PIN number to connect that way, check the `help` command output to see how to provide the right options to the `wsc` command.

如果您的网络需要验证一个 PIN 号码才能以这种方式连接，请检查 help 命令输出，以查看如何为 wsc 命令提供正确的选项。

#### Disconnect from a network 断开与网络的连接

To disconnect from a network:

断开与网络的连接:

```
[iwd]# station *device* disconnect

#### Show device and connection information 显示设备和连接信息

To display the details of a WiFi device, like MAC address:

显示 WiFi 设备的详细信息，比如 MAC 地址:

```
[iwd]# device *device* show

To display the connection state, including the connected network of a Wi\-Fi device:

显示连接状态，包括 Wi\-Fi 设备的连接网络:

```
[iwd]# station *device* show

#### Manage known networks 管理已知的网络

To list networks you have connected to previously:

列出你以前连接过的网络:

```
[iwd]# known\-networks list

To forget a known network:

忘记一个已知的网络:

```
[iwd]# known\-networks *SSID* forget

## Network configuration 网络配置

By default, `iwd` stores the network configuration in `/var/lib/iwd` directory. The configuration file is named as `*network*.*type*` where *network* is network SSID and *type* is network type i.e. one of "open", "wep", "psk", "8021x". The file is used to store the encrypted `PreSharedKey` and optionally the cleartext `Passphrase` and can be created by the user without invoking `iwctl`. The file can also be used for other configuration pertaining to that network SSID. For more settings, see [iwd.network(5)](https://man.archlinux.org/man/iwd.network.5).

默认情况下，iwd 将网络配置存储在/var/lib/iwd 目录中。配置文件命名为 network.type，其中网络为网络 SSID，类型为网络类型，即“ open”、“ wep”、“ psk”、“8021x”。该文件用于存储加密的 PreSharedKey 和可选的 cleartext Passphrase，用户可以在不调用 iwctl 的情况下创建该文件。该文件还可以用于属于该网络 SSID 的其他配置。有关更多设置，请参见 iwd. net work (5)。

### WPA\-PSK

A minimal example file to connect to a WPA\-PSK or WPA2\-PSK secured network with SSID "spaceship" and passphrase "test1234":

用 SSID“ spaceship”和密码“ test1234”连接 WPA\-PSK 或 WPA2\-PSK 安全网络的最小示例文件:

/var/lib/iwd/spaceship.psk

\[Security\]
PreSharedKey=aafb192ce2da24d8c7805c956136f45dd612103f086034c402ed266355297295

The PreSharedKey can be calculated from the SSID and the WiFi passphrase using *wpa\_passphrase* (from [wpa\_supplicant](https://archlinux.org/packages/?name=wpa_supplicant)) or [wpa\-psk](https://aur.archlinux.org/packages/wpa-psk/)AUR:

使用 wpa \_ passphrase (来自 wpa \_ supplicant)或 wpa\-pskAUR，可以通过 SSID 和 WiFi 口令计算 PreSharedKey:

$ wpa\_passphrase spaceship test1234

network={
        ssid="spaceship"
        #psk="test1234"
        psk=aafb192ce2da24d8c7805c956136f45dd612103f086034c402ed266355297295
}

**Note: 注意:**

*   If the SSID contains spaces or other special characters, they have to be quoted to be passed correctly to 如果 SSID 包含空格或其他特殊字符，必须引用它们才能正确地传递给*wpa\_passphrase 密码短语* by the 由[shell 贝壳](https://wiki.archlinux.org/index.php/Shell "Shell").
*   The SSID of the network is used as a filename only when it contains only alphanumeric characters or one of 只有当网络的 SSID 只包含字母数字字符或`- _`. If it contains any other characters, the name will instead be an 。如果它包含任何其他字符，则名称将改为`=`\-character followed by the hex\-encoded version of the SSID. \- 字符后跟十六进制编码版本的 SSID

### WPA Enterprise 深圳市万邦进出口有限公司

#### EAP\-PWD

For connecting to a EAP\-PWD protected enterprise access point you need to create a file called: `*essid*.8021x` in the folder `/var/lib/iwd` with the following content:

要连接到 EAP\-PWD 保护的企业访问点，需要在/var/lib/iwd 文件夹中创建一个名为: essid. 8021 x 的文件，其内容如下:

/var/lib/iwd/*essid*.8021x

\[Security\]
EAP\-Method=PWD
EAP\-Identity=*your\_enterprise\_email*
EAP\-Password=*your\_password*

\[Settings\]
AutoConnect=True

If you do not want autoconnect to the AP you can set the option to False and connect manually to the access point via `iwctl`. The same applies to the password, if you do not want to store it plaintext leave the option out of the file and just connect to the enterprise AP.

如果您不希望自动连接到 AP，可以将选项设置为 False 并通过 iwctl 手动连接到访问点。这同样适用于密码，如果你不想存储它纯文本留出选项的文件，只是连接到企业 AP。

#### EAP\-PEAP

Like EAP\-PWD, you also need to create a `*essid*.8021x` in the folder. Before you proceed to write the configuration file, this is also a good time to find out which CA certificate your organization uses. For MSCHAPv2 to work you also need to install [ppp](https://archlinux.org/packages/?name=ppp). Please see [MS\-CHAPv2](https://wiki.archlinux.org/index.php/Network_configuration/Wireless#MS-CHAPv2 "Network configuration/Wireless") for more infos. This is an example configuration file that uses MSCHAPv2 password authentication:

与 EAP\-PWD 一样，您还需要在文件夹中创建一个 essid. 8021 x。在继续编写配置文件之前，这也是查明组织使用哪个 CA 证书的好时机。要使用 MSCHAPv2，还需要安装 ppp。更多信息请参见 MS\-CHAPv2。这是一个使用 MSCHAPv2密码身份验证的示例配置文件:

/var/lib/iwd/*essid*.8021x

\[Security\]
EAP\-Method=PEAP
EAP\-Identity=anonymous@realm.edu
EAP\-PEAP\-CACert=/path/to/root.crt
EAP\-PEAP\-ServerDomainMask=radius.realm.edu
EAP\-PEAP\-Phase2\-Method=MSCHAPV2
EAP\-PEAP\-Phase2\-Identity=johndoe@realm.edu
EAP\-PEAP\-Phase2\-Password=hunter2

\[Settings\]
AutoConnect=true

**Tip: 提示:** If you are planning on using 如果你打算使用*eduroam 教育*, see also ，请参阅[#Eduroam \* 教育漫步](#Eduroam).

#### TTLS\-PAP

Like EAP\-PWD, you also need to create a `*essid*.8021x` in the folder. Before you proceed to write the configuration file, this is also a good time to find out which CA certificate your organization uses. This is an example configuration file that uses PAP password authentication:

与 EAP\-PWD 一样，您还需要在文件夹中创建一个 essid. 8021 x。在继续编写配置文件之前，这也是查明组织使用哪个 CA 证书的好时机。下面是一个使用密码验证的配置文件示例:

/var/lib/iwd/*essid*.8021x

\[Security\]
EAP\-Method=TTLS
EAP\-Identity=anonymous@uni\-test.de
EAP\-TTLS\-CACert=cert.pem
EAP\-TTLS\-ServerDomainMask=\*.uni\-test.de
EAP\-TTLS\-Phase2\-Method=Tunneled\-PAP
EAP\-TTLS\-Phase2\-Identity=user
EAP\-TTLS\-Phase2\-Password=password

\[Settings\]
AutoConnect=true

#### Eduroam

Eduroam offers a [configuration assistant tool (CAT)](https://cat.eduroam.org/), which unfortunately does not support iwd. However, the installer, which you can download by clicking on the download button then selecting your university, is just a Python script. It is easy to extract the necessary configuration options, including the certificate and server domain mask.

Eduroam 提供了一个配置助理工具(CAT) ，但不幸的是它不支持 iwd。然而，安装程序只是一个 Python 脚本，你可以点击下载按钮然后选择你的大学来下载。很容易提取必要的配置选项，包括证书和服务器域掩码。

The following table contains a mapping of iwd configuration options to eduroam CAT install script variables.

下表包含 iwd 配置选项到 eduroam CAT 安装脚本变量的映射。

| Iwd Configuration Option Iwd 配置选项 | CAT Script Variable 脚本变量 |
| --- | --- |
| file name 文件名 | one of 其中之一`Config.ssids` |
| `EAP-Method` | `Config.eap_outer` |
| `EAP-Identity` | `Config.anonymous_identity` |
| `EAP-PEAP-CACert` | `Config.CA` |
| `EAP-PEAP-ServerDomainMask` | one of 其中之一`Config.servers` |
| `EAP-PEAP-Phase2-Method` | `Config.eap_inner` |
| `EAP-PEAP-Phase2-Identity` | username@ 用户名@`Config.user_realm` |

**Note: 注意:**

*   `EAP-Identity` may not be required by your Eduroam provider, in which case you can use 你的教育漫游服务供应商可能不会要求你使用`anonymous` in this field. 在这个领域
*   If your 如果你的`EAP-PEAP-ServerDomainMask` starts with 从... 开始`DNS:`, use only the part after ，只使用之后的部分`DNS:`.

#### Other cases 其他个案

More example tests can be [found in the test cases](https://git.kernel.org/pub/scm/network/wireless/iwd.git/tree/autotests) of the upstream repository.

在上游存储库的测试用例中可以找到更多的示例测试。

## Optional configuration 可选配置

File `/etc/iwd/main.conf` can be used for main configuration. See [iwd.config(5)](https://man.archlinux.org/man/iwd.config.5).

文件/etc/iwd/main.conf 可用于主配置。

### Disable auto\-connect for a particular network 禁用特定网络的自动连接

Create / edit file `/var/lib/iwd/*network*.*type*`. Add the following section to it:

创建/编辑文件/var/lib/iwd/network.type:

/var/lib/iwd/spaceship.psk (for example)

\[Settings\]
AutoConnect=false

### Disable periodic scan for available networks 禁用可用网络的周期性扫描

By default when `iwd` is in disconnected state, it periodically scans for available networks. To disable periodic scan (so as to always scan manually), create / edit file `/etc/iwd/main.conf` and add the following section to it:

默认情况下，当 iwd 处于断开状态时，它会定期扫描可用的网络。要禁用周期性扫描(以便始终手动扫描) ，请创建/编辑文件/etc/iwd/main.conf 并添加以下部分:

/etc/iwd/main.conf

\[Scan\]
DisablePeriodicScan=true

### Enable built\-in network configuration 启用内置网络配置

Since version 0.19, iwd can assign IP address(es) and set up routes using a built\-in DHCP client or with static configuration. It is a good alternative to [standalone DHCP clients](https://wiki.archlinux.org/index.php/Network_configuration#DHCP "Network configuration").

自0.19版本以来，iwd 可以使用内置的 DHCP 客户机或静态配置分配 IP 地址和设置路由。它是独立 DHCP 客户端的一个很好的替代品。

To activate iwd's network configuration feature, create/edit `/etc/iwd/main.conf` and add the following section to it:

要激活 iwd 的网络配置特性，请创建/edit/etc/iwd/main.conf 并添加以下部分:

/etc/iwd/main.conf

\[General\]
EnableNetworkConfiguration=true

There is also ability to set route metric with `RoutePriorityOffset`:

还可以使用 routepriityoffset 设置路由度量:

/etc/iwd/main.conf

\[Network\]
RoutePriorityOffset=300

#### IPv6 support 支援 IPv6

Since version 1.10, iwd supports IPv6, but it is disabled by default. To enable it, add the following to the configuration file:

由于版本1.10，iwd 支持 IPv6，但默认情况下是禁用的。要启用它，在配置文件中添加以下内容:

/etc/iwd/main.conf

\[Network\]
EnableIPv6=true

This setting is required whether you want to use DHCPv6 or static IPv6 configuration. It can also be set on a per\-network basis.

无论您想使用 DHCPv6还是静态 IPv6配置，都需要这个设置。它也可以根据每个网络进行设置。

#### Setting static IP address in network configuration 在网络配置中设置静态 IP 地址

Add the following section to `/var/lib/iwd/*network*.*type*` file. For example:

将以下部分添加到/var/lib/iwd/network.type 文件中:

/var/lib/iwd/spaceship.psk

\[IPv4\]
Address=192.168.1.10
Netmask=255.255.255.0
Gateway=192.168.1.1
Broadcast=192.168.1.255
DNS=192.168.1.1

#### Select DNS manager 选择 DNS 管理器

At the moment, iwd supports two DNS managers—[systemd\-resolved](https://wiki.archlinux.org/index.php/Systemd-resolved "Systemd-resolved") and [resolvconf](https://wiki.archlinux.org/index.php/Resolvconf "Resolvconf").

目前，iwd 支持两个 DNS 管理器: systemd\-resolved 和 resolvconf。

Add the following section to `/etc/iwd/main.conf` for `systemd-resolved`:

将以下部分添加到/etc/iwd/main.conf 中的 systemd\-resolved:

/etc/iwd/main.conf

\[Network\]
NameResolvingService=systemd

For `resolvconf`:

对于 resolvconf:

/etc/iwd/main.conf

\[Network\]
NameResolvingService=resolvconf

### Deny console (local) user from modifying the settings 拒绝控制台(本地)用户修改设置

By default `iwd` D\-Bus interface allows *any* console user to connect to `iwd` daemon and modify the settings, even if that user is not a *root* user.

默认情况下，iwd D\-Bus 接口允许任何控制台用户连接到 iwd 守护进程并修改设置，即使该用户不是根用户。

If you do not want to allow console user to modify the settings but allow reading the status information, then create a D\-Bus configuration file as follows.

如果不希望允许控制台用户修改设置，但允许读取状态信息，那么按照以下方式创建一个 D\-Bus 配置文件。

/etc/dbus\-1/system.d/iwd\-strict.conf

<!\-\- prevent local users from changing iwd settings, but allow
     reading status information. overrides some part of
     /usr/share/dbus\-1/system.d/iwd\-dbus.conf. \-\->

<!\-\- This configuration file specifies the required security policies
     for iNet Wireless Daemon to work. \-\->

<!DOCTYPE busconfig PUBLIC "\-//freedesktop//DTD D\-BUS Bus Configuration 1.0//EN"
 "http://www.freedesktop.org/standards/dbus/1.0/busconfig.dtd">
<busconfig>

  <policy at\_console="true">
    <deny send\_destination="net.connman.iwd"/>
    <allow send\_destination="net.connman.iwd" send\_interface="org.freedesktop.DBus.Properties" send\_member="GetAll" />
    <allow send\_destination="net.connman.iwd" send\_interface="org.freedesktop.DBus.Properties" send\_member="Get" />
    <allow send\_destination="net.connman.iwd" send\_interface="org.freedesktop.DBus.ObjectManager" send\_member="GetManagedObjects" />
    <allow send\_destination="net.connman.iwd" send\_interface="net.connman.iwd.Device" send\_member="RegisterSignalLevelAgent" />
    <allow send\_destination="net.connman.iwd" send\_interface="net.connman.iwd.Device" send\_member="UnregisterSignalLevelAgent" />
  </policy>

</busconfig>

**Tip: 提示:** Remove 移除*<allow> < 允许 >* lines above to deny reading the status information as well. 第行以拒绝阅读状态信息

## Troubleshooting 故障排除

### Verbose TLS debugging 详细 TLS 调试

This can be useful, if you have trouble setting up MSCHAPv2 or TTLS. You can set the following [environment variable](https://wiki.archlinux.org/index.php/Environment_variable "Environment variable") via a [drop\-in snippet](https://wiki.archlinux.org/index.php/Drop-in_snippet "Drop-in snippet"):

如果您在设置 MSCHAPv2或 TTLS 时遇到困难，这可能会很有用。你可以通过下拉菜单设置以下环境变量:

/etc/systemd/system/iwd.service.d/tls\-debug.conf

\[Service\]
Environment=IWD\_TLS\_DEBUG=TRUE

Check the iwd logs afterwards via `journalctl -u iwd.service`

之后通过 journalctl\-u iwd.service 检查 iwd 日志

### Connect issues after reboot 重新启动后连接问题

A low entropy pool can cause connection problems in particular noticeable after reboot. See [Random number generation](https://wiki.archlinux.org/index.php/Random_number_generation "Random number generation") for suggestions to increase the entropy pool.

低熵池会导致连接问题，在重新启动后尤其明显。有关增加熵池的建议，请参见随机数生成。

### Wireless device is not renamed by udev Udev 不会重命名无线设备

Since version 1.0, iwd disables predictable renaming of wireless device. It installs the following systemd network link configuration file which prevents udev from renaming the interface to `wlp#s#`:

由于版本1.0，iwd 禁止无线设备的可预测重命名。它安装以下 systemd 网络链接配置文件，防止 udev 将接口重命名为 wlp # s # :

/usr/lib/systemd/network/80\-iwd.link

\[Match\]
Type=wlan

\[Link\]
NamePolicy=keep kernel

As a result the wireless link name `wlan#` is kept after boot. This resolved a race condition between *iwd* and [udev](https://wiki.archlinux.org/index.php/Udev "Udev") on interface renaming as explained in [iwd udev interface renaming](https://iwd.wiki.kernel.org/interface_lifecycle#udev_interface_renaming).

因此，无线链接名称 wlan # 在引导后保留。这解决了界面重命名中 iwd 和 udev 之间的竞争条件，如 iwd udev 界面重命名所解释的那样。

If this results in issues try masking it with:

如果这导致了问题，试着用下面的方法掩盖它:

\# ln \-s /dev/null /etc/systemd/network/80\-iwd.link

## See also 参见

*   [Getting Started with iwd 开始使用 iwd](https://iwd.wiki.kernel.org/gettingstarted)
*   [Network Configuration Settings 网络配置设置](https://iwd.wiki.kernel.org/networkconfigurationsettings)
*   [More Examples for WPA Enterprise 更多 WPA 企业实例](https://git.kernel.org/pub/scm/network/wireless/iwd.git/tree/autotests)
*   [The IWD thread on the Arch Linux Forums 在 Arch Linux 论坛上的 IWD 线程](https://bbs.archlinux.org/viewtopic.php?id=237074)
*   [2017 Update on new WiFi daemon for Linux by Marcel Holtmann \- YouTube 2017年更新 Linux 版 WiFi 守护进程作者: Marcel Holtmann\-YouTube](https://www.youtube.com/watch?v=F2Q86cphKDo)
*   [The New Wi\-Fi Experience for Linux \- Marcel Holtmann, Intel \- YouTube Linux 的新 Wi\-Fi 体验\-Marcel Holtmann，Intel\-YouTube](https://www.youtube.com/watch?v=QIqT2obSPDk)

Retrieved from "[https://wiki.archlinux.org/index.php?title=Iwd&oldid=649818](https://wiki.archlinux.org/index.php?title=Iwd&oldid=649818)"

[Categories 类别](https://wiki.archlinux.org/index.php/Special:Categories "Special:Categories"):

*   [Wireless networking 无线网络](https://wiki.archlinux.org/index.php/Category:Wireless_networking "Category:Wireless networking")
*   [Network configuration 网络配置](https://wiki.archlinux.org/index.php/Category:Network_configuration "Category:Network configuration")

## Navigation menu

## 导航菜单

### Personal tools

*   [Create account 创建账户](https://wiki.archlinux.org/index.php?title=Special:CreateAccount&returnto=Iwd "You are encouraged to create an account and log in; however, it is not mandatory")
*   [Log in 登录](https://wiki.archlinux.org/index.php?title=Special:UserLogin&returnto=Iwd "You are encouraged to log in; however, it is not mandatory [Alt+Shift+o]")

### Namespaces

*   [Page 第二页](https://wiki.archlinux.org/index.php/Iwd "View the content page [Alt+Shift+c]")
*   [Discussion 讨论](https://wiki.archlinux.org/index.php/Talk:Iwd "Discussion about the content page [Alt+Shift+t]")

### Variants

### Views

*   [Read 阅读](https://wiki.archlinux.org/index.php/Iwd)
*   [View source 查看来源](https://wiki.archlinux.org/index.php?title=Iwd&action=edit "This page is protected.
    You can view its source [Alt+Shift+e]")
*   [View history 查看历史](https://wiki.archlinux.org/index.php?title=Iwd&action=history "Past revisions of this page [Alt+Shift+h]")

### More

### Search

### 搜寻