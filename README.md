# Lavalink Server Setup Guide

This README guide provides a comprehensive tutorial on what **Lavalink** is, how to set up your own Lavalink server, and how to keep it running 24/7 from different environments such as your **PC**, **VPS**, and even your **phone** using **Termux**.

---

## What is a Lavalink Server?

**Lavalink** is a high-performance audio streaming server that is primarily used by Discord bots for managing audio playback. It offloads audio decoding and streaming from your bot to a separate server, providing enhanced performance, scalability, and reliability for music bots.

Lavalink is based on the **Lavaplayer** library, which enables efficient audio streaming from sources such as YouTube, SoundCloud, and more. It communicates with your Discord bot through a specific protocol (using WebSocket or HTTP), enabling it to stream audio without putting excessive load on the bot itself.

### Key Features:
- **High-performance streaming** with low latency.
- Supports a variety of audio formats (MP3, OGG, etc.).
- Can stream audio from platforms like YouTube and SoundCloud.
- Offloads audio processing from your Discord bot, reducing resource consumption.
  
---

## How to Create Your Own Lavalink Server

### Prerequisites:
1. **Java 11 or higher**.
2. **Git** (if you wish to clone the Lavalink repository).
3. **Redis** (optional, for caching).

---

### Step 1: Install Java

Lavalink is built with Java, so you need to have **Java 11** or later installed on your system.

For **Linux** (Ubuntu/Debian):
```bash
sudo apt update
sudo apt install openjdk-11-jdk
```

For **Windows** or **macOS**, download Java from the [AdoptOpenJDK](https://adoptopenjdk.net/) website and install it.

---

### Step 2: Clone the Lavalink Repository

To get the Lavalink server, you can either clone it directly from GitHub or download the precompiled `Lavalink.jar` file.

To clone the repository:
```bash
git clone https://github.com/freyacodes/Lavalink.git
cd Lavalink
```

If you want the precompiled JAR, download it from the [releases page](https://github.com/freyacodes/Lavalink/releases).

---

### Step 3: Build Lavalink (Optional)

If you cloned the repository and want to build Lavalink, run the following command:
```bash
./mvnw clean install
```

This will compile the Lavalink server and prepare it for use.

---

### Step 4: Configure Lavalink

Lavalink uses a `application.yml` file for configuration. Edit this file to set up your Lavalink server:

```yaml
server:
  port: 2333          # Default port for Lavalink
  password: "yoursecretpassword"  # Set a password to connect with your bot
  redis:
    host: localhost    # Optional, for Redis caching (can leave out if not using Redis)
    port: 6379

ytdlOptions:
  youtube_dl:
    max-downloads: 5   # Max YouTube downloads (optional)
```

Make sure to replace `"yoursecretpassword"` with a strong password.

---

### Step 5: Start Lavalink Server

Now, start the Lavalink server by running the following command:
```bash
java -jar Lavalink.jar
```

The server should now be running on port `2333` (or the port you configured).

---

## How to Keep Your Lavalink Server Online 24/7

You can keep your Lavalink server running 24/7 from **PC**, **VPS**, and even from your **phone** using **Termux**.

### 1. Keep Lavalink Running on a PC

If you are running Lavalink from your **PC**, use tools like **screen** or **tmux** to keep it running even if the terminal is closed.

#### Option 1: Using `screen`

1. Install `screen` (if not already installed):
   ```bash
   sudo apt install screen
   ```
   
2. Start a new screen session:
   ```bash
   screen -S lavalink
   ```

3. Run the Lavalink server:
   ```bash
   java -jar Lavalink.jar
   ```

4. Detach from the screen session (without stopping the server):
   Press `Ctrl + A`, then press `D`.

5. To reattach to the session:
   ```bash
   screen -r lavalink
   ```

#### Option 2: Using `tmux`

1. Install `tmux`:
   ```bash
   sudo apt install tmux
   ```

2. Start a new tmux session:
   ```bash
   tmux new -s lavalink
   ```

3. Run Lavalink:
   ```bash
   java -jar Lavalink.jar
   ```

4. Detach from the session:
   Press `Ctrl + B`, then press `D`.

5. To reattach to the tmux session:
   ```bash
   tmux attach -t lavalink
   ```

### 2. Keep Lavalink Running on a VPS

To keep Lavalink running 24/7 on a **VPS**, you can use `systemd`, which will automatically restart Lavalink if it crashes and ensure it starts on reboot.

#### Steps to Set Up Lavalink as a `systemd` Service:

1. Create a `lavalink.service` file in `/etc/systemd/system/`:
   ```bash
   sudo nano /etc/systemd/system/lavalink.service
   ```

2. Add the following content:
   ```ini
   [Unit]
   Description=Lavalink Server
   After=network.target

   [Service]
   User=your_username
   WorkingDirectory=/path/to/lavalink
   ExecStart=/usr/bin/java -jar /path/to/Lavalink.jar
   Restart=always
   RestartSec=10
   LimitNOFILE=4096

   [Install]
   WantedBy=multi-user.target
   ```

3. Reload the systemd configuration:
   ```bash
   sudo systemctl daemon-reload
   ```

4. Start the Lavalink service:
   ```bash
   sudo systemctl start lavalink
   ```

5. Enable Lavalink to start on boot:
   ```bash
   sudo systemctl enable lavalink
   ```

6. Check the status of Lavalink:
   ```bash
   sudo systemctl status lavalink
   ```

### 3. Keep Lavalink Running Using Termux (On Android)

You can also run Lavalink 24/7 on your **Android phone** using **Termux**, which provides a Linux-like environment.

#### Steps to Set Up Lavalink on Termux:

1. Install **Termux** from the Google Play Store or F-Droid.

2. Open Termux and install required packages:
   ```bash
   pkg update
   pkg install openjdk-11
   pkg install git
   ```

3. Clone the Lavalink repository or upload the precompiled `Lavalink.jar`:
   ```bash
   git clone https://github.com/freyacodes/Lavalink.git
   cd Lavalink
   ```

4. Run Lavalink in Termux:
   ```bash
   java -jar Lavalink.jar
   ```

To keep Lavalink running in the background, use **screen** or **tmux** as described above.

---

## Conclusion

By following this guide, you can create your own **Lavalink server** and run it 24/7 from various environments like your **PC**, **VPS**, or even your **Android phone** using **Termux**. Whether you want to deploy it on a powerful VPS for scalability or keep it running on a mobile device, this setup ensures continuous availability for your Discord bot to handle audio streaming.

Let your music bot play seamlessly across your Discord servers with Lavalink!
