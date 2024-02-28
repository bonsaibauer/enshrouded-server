# Setting Up an Enshrouded Dedicated Server on Ubuntu: A Beginner's Guide

Embark on an adventure in the mystical world of Embervale with your own dedicated Enshrouded server. This guide will walk you through setting up a dedicated server for Enshrouded on Ubuntu, ensuring you and your friends can enjoy this survival action RPG to its fullest.

## Enshrouded: A Vast World of Survival and Magic

Enshrouded is an immersive survival action RPG set in a vast, voxel-based open world. Players must navigate through dangerous terrains, craft items for survival, and battle against various creatures. The game supports cooperative play for up to 16 players, allowing for a shared adventure in the magical world of Embervale.

## Preparing Your Environment

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

### Update and Install Wine

Refresh your package list and install Wine.

```bash
sudo apt update
sudo apt install --install-recommends winehq-staging
```

## SteamCMD and Enshrouded Server Installation

### SteamCMD Setup

Install SteamCMD, required to download the Enshrouded dedicated server.

```bash
sudo apt install steamcmd
```

### Server Installation

Use SteamCMD to install the Enshrouded dedicated server.

```bash
/usr/games/steamcmd +@sSteamCmdForcePlatformType windows +force_install_dir /home/yourusername/enshroudedserver +login anonymous +app_update 2278520 +quit
```

## Running and Managing Your Server

### Starting the Server

Start your Enshrouded dedicated server using Wine.

```bash
wine64 ~/enshroudedserver/enshrouded_server.exe
```

### Creating a Service

For long-term server management, create a systemd service.

### Enable and Start the Service

Ensure your server starts with your system.

```bash
sudo systemctl enable enshrouded
sudo systemctl start enshrouded
```

## Conclusion

By following this guide, you've set up your own Enshrouded dedicated server on Ubuntu, ready to host your adventures in the mystical world of Embervale. Gather your friends and start your journey in this captivating survival action RPG.
