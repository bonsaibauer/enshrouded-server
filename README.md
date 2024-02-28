# Setting Up an Enshrouded Dedicated Server on Ubuntu: A Beginner's Guide

Embark on an adventure in the mystical world of Embervale with your own dedicated Enshrouded server. This guide will walk you through setting up a dedicated server for Enshrouded on Ubuntu, ensuring you and your friends can enjoy this survival action RPG to its fullest.

## Enshrouded: A Vast World of Survival and Magic

Enshrouded is an immersive survival action RPG set in a vast, voxel-based open world. Players must navigate through dangerous terrains, craft items for survival, and battle against various creatures. The game supports cooperative play for up to 16 players, allowing for a shared adventure in the magical world of Embervale.

## Preparing Your Environment

## Prerequisites

- Ubuntu 22.04 (While these steps are written for Ubuntu, they should also work if you wanted to set up an Enshrouded server on other Linux distributions such as Debian.)
- sudo privileges
- ufw settings (Please note that the Enshrouded dedicated server uses ports 15636 and 15637 by default. You must port forward both ports to allow outside access to your server. Additionally, if you are using a firewall such as UFW you will need to allow these ports.)

### System Update

Ensure your Ubuntu system is up-to-date to avoid any compatibility issues.

```bash
sudo apt update
sudo apt upgrade -y
```

### Install Required Packages

Install essential packages for setting up your server.

```bash
sudo apt install software-properties-common lsb-release wget
```

## Wine Installation

We'll use Wine to run the Enshrouded server application, designed for Windows, on Ubuntu.

### Create Keyring Directory

This directory stores the Wine GPG key.

```bash
sudo mkdir -pm755 /etc/apt/keyrings
```

### Download Wine GPG Key

Securely download the Wine repository GPG key.

```bash
sudo wget -O /etc/apt/keyrings/winehq-archive.key https://dl.winehq.org/wine-builds/winehq.key
```

### Add Wine Repository

Fetch the latest Wine version by adding its repository.

```bash
sudo wget -NP /etc/apt/sources.list.d/ https://dl.winehq.org/wine-builds/$(lsb_release -is | tr '[:upper:]' '[:lower:]')/dists/$(lsb_release -cs)/winehq-$(lsb_release -cs).sources
```

You can add a new architecture by using the following command.
```bash
sudo dpkg --add-architecture i386
```
### Update and Install Wine

Refresh your package list and install Wine.

```bash
sudo apt update
sudo apt install --install-recommends winehq-staging
```

We need a few extra packages for Wine to work with the Enshrouded dedicated server on Ubuntu.


```bash
sudo apt install cabextract winbind screen xvfb
```

Next, we must add the multiverse repository to our Ubuntu device. This is because SteamCMD is a closed-source tool that is not included in the standard repositories.


```bash
sudo add-apt-repository multiverse
```


## SteamCMD and Enshrouded Server Installation

### SteamCMD Setup

Install SteamCMD, required to download the Enshrouded dedicated server.

```bash
sudo apt update
sudo apt install steamcmd
```
### Creating a User to Run the Server on Ubuntu
As the SteamCMD tool is not meant to be run by the root user, we must create one. 
- Use the following command to create a user called “enshrouded” on your system.

```bash
sudo useradd -m enshrouded
sudo -u enshrouded -s
cd ~
```

### Server Installation

Use SteamCMD to install the Enshrouded dedicated server.

```bash
/usr/games/steamcmd +@sSteamCmdForcePlatformType windows +force_install_dir /home/enshrouded/enshroudedserver +login anonymous +app_update 2278520 +quit
```

## Running and Managing Your Server

### Starting the Server

We can start the Enshrouded server on our Ubuntu device using the command below. You will notice that we must use “wine64” at the start to ensure we launch the server using Wine.

```bash
wine64 ~/enshroudedserver/enshrouded_server.exe
```
Once your server has started, the following two lines should appear within the terminal. While you can connect to the server now, you will likely want to adjust the configuration file.

```bash
[Session] 'HostOnline' (up)!
[Session] finished transition from 'Lobby' to 'Host_Online' (current='Host_Online')!
```

### Configuring the Enshrouded Dedicated Server on Ubuntu

After running the Enshrouded Dedicated Server on Ubuntu, it will automatically create a configuration file.

- You can edit this configuration file to control the number of players and set a password for the server.
- Set the server ip (important!), server name, password, and slot count as desired.

```bash
nano ~/enshroudedserver/enshrouded_server.json
```

### Creating a Service
A service file helps ensure that your server restarts if it crashes and also means it will automatically start when your server powers on.

```bash
sudo nano /etc/systemd/system/enshrouded.service
```

Add the following configuration:

```ini
[Unit]
Description=Enshrouded Server
Wants=network-online.target
After=network-online.target

[Service]
User=enshrouded
Group=enshrouded
WorkingDirectory=/home/enshrouded/
ExecStartPre=/usr/games/steamcmd +@sSteamCmdForcePlatformType windows +force_install_dir /home/enshrouded/enshroudedserver +login anonymous +app_update 2278520 +quit
ExecStart=/usr/bin/wine64 /home/enshrouded/enshroudedserver/enshrouded_server.exe
Restart=always

[Install]
WantedBy=multi-user.target
```

### Enable and Start the Service

Ensure your server starts with your system.

```bash
sudo systemctl enable enshrouded
sudo systemctl start enshrouded
```
### Disable and Stop the Service

```bash
sudo systemctl disable enshrouded
sudo systemctl stop enshrouded
```

## Conclusion

By following this guide, you've set up your own Enshrouded dedicated server on Ubuntu, ready to host your adventures in the mystical world of Embervale. Gather your friends and start your journey in this captivating survival action RPG.
