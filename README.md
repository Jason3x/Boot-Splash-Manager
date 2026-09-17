# Boot Splash Manager — R36S

![Platform](https://img.shields.io/badge/Platform-R36S-blue)
![OS](https://img.shields.io/badge/OS-dArkOSen-green)
![Shell](https://img.shields.io/badge/Bash-Script-yellow)
![License](https://img.shields.io/badge/License-Free-lightgrey)

Play a video, a GIF or an animated PNG sequence while your console boots, right before EmulationStation appears.
Everything is managed from the console itself, with the gamepad — no PC needed.

---

## ✨ Features

- 🎬 **Videos, GIFs, images and PNG sequences** — pick any of them as your splash
- ⚡ **Instant start** — frames are decoded once, then written straight to the screen at boot
- 🎲 **Random mode** — a different splash on every boot
- ☁️ **Built-in downloader** — grab ready-made animations from this repository
- ⏱️ **Automatic duration** — set to the length of your media when you pick it
- 👁️ **Preview** — watch it now, without restarting anything
- 🔇 **Enable / Disable** — keep everything installed, skip playback

---

## 🚀 Installation

1. Copy **`Boot Splash Manager.sh`** to `/roms/tools/` on your SD card
2. Launch it from the **tools** section on your device
3. Choose **Install boot splash**

That's it. Pick a splash, reboot, enjoy.

---

## 📋 Menu

| Entry | What it does |
|-------|--------------|
| **Install boot splash** | Sets everything up |
| **Select splash** | Choose your media, duration is set automatically |
| **Download PNG sequence** | Browse and download animations from this repo |
| **Splash duration** | How long the splash plays, in seconds |
| **View splash** | Watch it right now |
| **Enable / Disable splash** | Turn it off without uninstalling |
| **Uninstall boot splash** | Removes it, keeps your files |

---

## 📂 Your media

Drop your own files in `/roms/bootsplash` and they appear in **Select splash**:

| Folder | What goes in it |
|--------|-----------------|
| `/roms/bootsplash/` | Your videos, GIFs and images |
| `/roms/bootsplash/random/` | Pool for random mode, one drawn per boot |
| `/roms/bootsplash/themes/` | Downloaded animations, kept for reuse |

Supported: `mp4` `mkv` `webm` `avi` `gif` `png` `jpg` `bmp`

---

## ☁️ Repository content

| Folder | What's inside |
|--------|---------------|
| `gif/` | Animated GIFs |
| `mp4/` | Videos |
| `sequences/` | One folder per animation, with numbered PNG frames |

Anything added here shows up in the download menu on the console.
Want to contribute an animation? Open a pull request.

---

## 💡 Tips

**A 640x480 video is lighter and faster than a big GIF:**

```bash
ffmpeg -i source.gif \
  -vf "scale=640:480:force_original_aspect_ratio=decrease,pad=640:480:(ow-iw)/2:(oh-ih)/2" \
  -c:v libx264 -preset veryfast -pix_fmt yuv420p -an output.mp4
```

**White background?** The screen has no transparency, so white stays white. Remove it first:

```bash
ffmpeg -y -f lavfi -i color=c=black:s=640x480 -i source.gif \
  -filter_complex "[1:v]colorkey=0xFFFFFF:0.12:0.0,scale=640:480:force_original_aspect_ratio=decrease[fg];[0:v][fg]overlay=(W-w)/2:(H-h)/2:shortest=1,format=yuv420p" \
  -c:v libx264 -preset veryfast -an output.mp4
```

**Boot feels long?** The splash holds EmulationStation for its whole length — that's the point, it hides the loading. Lower **Splash duration** if you want it shorter.

**Nothing shows up?** Run it by hand to see what happens:

```bash
sudo systemctl stop emulationstation
sudo BOOTSPLASH_DEBUG=1 BOOTSPLASH_JOURNAL=1 /usr/local/bin/bootsplash-anim.sh
sudo systemctl start emulationstation
```

---

## 🙏 Thanks

- [christianhaitian](https://github.com/christianhaitian) for ArkOS and the R36S ecosystem
- [djparentx](https://github.com/djparentx) for dArkOSen

---

## ☕ Support the project

[![Ko-fi](https://img.shields.io/badge/☕_Buy_me_a_coffee-jason3x-red?style=for-the-badge)](https://ko-fi.com/jason3x)
