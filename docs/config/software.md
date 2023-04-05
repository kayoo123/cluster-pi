## OS - DietPi
Apres avoir joué longtemps sur la distribution classique `Raspberry PI OS`, j'ai décidé de donner sa chance à une autre : **DietPi**.

Ce petit frere, n'a rien a lui envier, et il a de bon argument pour devenir mon OS favori sur ce genre de projet : 

- [Version allégé](https://dietpi.com/stats.html#distrostats) (taille/process)
- Gestion de logs en RAM native
- Automatisation complete [POST-Install](https://dietpi.com/docs/usage/#how-to-do-an-automatic-base-installation-at-first-boot-dietpi-automation)

Et bien plus encore : https://dietpi.com/#features


## Creation du support

On va préparer nos cartes SD et flasher [la derniere image de DietPi](https://dietpi.com/#downloadinfo) à l'aide d'un super outil : [`BalenaEtcher`](https://www.balena.io/etcher).

Une fois fait, ne démarrer pas tout de suite vos petits serveurs, on va passer à une étape crucial, la configuration automatique de notre systeme.

## DietPi Automation

Merci DietPi, grace a toi, je m'evite de nombreuse manipulation manuelle pour la gestion de : 

- Installation complete sans action de l'utilisation (on boot, on attend, et on profite)
- Gestion des locales (FR)
- Désactivation du Wifi
- Désactivation de l'IPv6
- Force une IP Fixe
- Set un nom de machine (Hostname)
- Gestion de la swap
- Gestion du boot
- Installation de packages 
- et bien d'autre choses (OverClock,proxy,repo,etc...)

Tout ce fait a partir d'un unique fichier (KISS), il est très bien documenté mais voici en exemple ce que j'ai parametré : 

### Dietpi.txt

```bash
AUTO_SETUP_ACCEPT_LICENSE=1
AUTO_SETUP_LOCALE=C.UTF-8
AUTO_SETUP_KEYBOARD_LAYOUT=us
AUTO_SETUP_TIMEZONE=Europe/France
AUTO_SETUP_NET_ETHERNET_ENABLED=1
AUTO_SETUP_NET_WIFI_ENABLED=0
AUTO_SETUP_NET_ETH_FORCE_SPEED=0
AUTO_SETUP_NET_WIFI_COUNTRY_CODE=FR

AUTO_SETUP_NET_USESTATIC=1
AUTO_SETUP_NET_STATIC_IP=192.168.1.200
AUTO_SETUP_NET_STATIC_MASK=255.255.255.0
AUTO_SETUP_NET_STATIC_GATEWAY=192.168.1.1
AUTO_SETUP_NET_STATIC_DNS=192.168.1.2

AUTO_SETUP_DHCP_TO_STATIC=0

AUTO_SETUP_NET_HOSTNAME=control01

AUTO_SETUP_BOOT_WAIT_FOR_NETWORK=1
AUTO_SETUP_SWAPFILE_SIZE=1
AUTO_SETUP_SWAPFILE_LOCATION=/var/swap
AUTO_SETUP_HEADLESS=1
AUTO_UNMASK_LOGIND=0
AUTO_SETUP_CUSTOM_SCRIPT_EXEC=0
AUTO_SETUP_BACKUP_RESTORE=0
AUTO_SETUP_SSH_SERVER_INDEX=-2
AUTO_SETUP_LOGGING_INDEX=-1
AUTO_SETUP_RAMLOG_MAXSIZE=200

AUTO_SETUP_WEB_SERVER_INDEX=0
AUTO_SETUP_DESKTOP_INDEX=0
AUTO_SETUP_BROWSER_INDEX=0
AUTO_SETUP_AUTOSTART_TARGET_INDEX=7
AUTO_SETUP_AUTOSTART_LOGIN_USER=root
AUTO_SETUP_GLOBAL_PASSWORD=dietpi
AUTO_SETUP_AUTOMATED=1
SURVEY_OPTED_IN=0


AUTO_SETUP_INSTALL_SOFTWARE_ID=0 	#OpenSSH Client
AUTO_SETUP_INSTALL_SOFTWARE_ID=1	#Samba Client
AUTO_SETUP_INSTALL_SOFTWARE_ID=20	#vim
AUTO_SETUP_INSTALL_SOFTWARE_ID=105	#OpenSSH Server
AUTO_SETUP_INSTALL_SOFTWARE_ID=130	#Python 3 pip

CONFIG_CPU_GOVERNOR=schedutil
CONFIG_CPU_ONDEMAND_SAMPLE_RATE=25000
CONFIG_CPU_ONDEMAND_SAMPLE_DOWNFACTOR=40
CONFIG_CPU_USAGE_THROTTLE_UP=50

CONFIG_CPU_MAX_FREQ=Disabled
CONFIG_CPU_MIN_FREQ=Disabled

CONFIG_CPU_DISABLE_TURBO=0

CONFIG_PROXY_ADDRESS=MyProxyServer.com
CONFIG_PROXY_PORT=8080
CONFIG_PROXY_USERNAME=
CONFIG_PROXY_PASSWORD=

CONFIG_G_CHECK_URL_TIMEOUT=10
CONFIG_G_CHECK_URL_ATTEMPTS=5
CONFIG_CHECK_CONNECTION_IP=9.9.9.9
CONFIG_CHECK_DNS_DOMAIN=dns9.quad9.net

CONFIG_CHECK_DIETPI_UPDATES=1
CONFIG_CHECK_APT_UPDATES=1
CONFIG_NTP_MODE=2
CONFIG_SERIAL_CONSOLE_ENABLE=0
CONFIG_SOUNDCARD=none
CONFIG_LCDPANEL=none
CONFIG_ENABLE_IPV6=0

CONFIG_APT_RASPBIAN_MIRROR=http://raspbian.raspberrypi.org/raspbian/
CONFIG_APT_DEBIAN_MIRROR=https://deb.debian.org/debian/
CONFIG_NTP_MIRROR=debian.pool.ntp.org

#------------------------------------------------------------------------------------------------------
##### DietPi-Software settings #####
#------------------------------------------------------------------------------------------------------
SOFTWARE_DISABLE_SSH_PASSWORD_LOGINS=0

#------------------------------------------------------------------------------------------------------
##### Dev settings #####
#------------------------------------------------------------------------------------------------------
DEV_GITBRANCH=master
DEV_GITOWNER=MichaIng

#------------------------------------------------------------------------------------------------------
##### Settings, automatically added by dietpi-update #####
#------------------------------------------------------------------------------------------------------
```

Vous pouvez ensuite simplement changer l'`IP` et le `HOSTNAME`, celon votre usages : 
```
AUTO_SETUP_NET_HOSTNAME=control01
AUTO_SETUP_NET_STATIC_IP=192.168.1.200
```


### cmdline.txt

On va profiter et rajouter 3 clés qui serviront pour installer notre cluster Kubernetes - K3s.

Editer le fichier *cmdline.txt* et ajouter à la fin : `group_enable=cpuset cgroup_enable=memory cgroup_memory=1`

Par exemple :

```bash
root=PARTUUID=00858852-02 rootfstype=ext4 rootwait net.ifnames=0 logo.nologo console=serial0,115200 console=tty1 group_enable=cpuset cgroup_enable=memory cgroup_memory=1
```

