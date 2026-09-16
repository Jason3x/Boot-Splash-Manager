# Boot Splash Manager

Animated boot splash for R36S handhelds running dArkOS.

The splash plays after the kernel logo and before EmulationStation starts. It
can be a video, an animated GIF, a still image, or a PNG sequence. Everything is
managed from a menu on the console itself, with the gamepad.

## How it works

The selected media is decoded once into raw BGRA frames. At boot the player
writes those frames straight to `/dev/fb0`, which starts instantly instead of
loading ffplay and its ~200 shared libraries from the SD card. ffplay stays as a
fallback, and is also what the preview uses, since EmulationStation holds the
DRM master while it runs.

Boot cost on an R36S: about 3 seconds of service time for 3 seconds of
animation, against roughly 10 seconds with ffplay alone.

## Install

Copy `Boot Splash Manager.sh` to your ports folder, for example
`/roms/tools/`, then launch it from EmulationStation and choose
**Install boot splash**. That writes the player to
`/usr/local/bin/bootsplash-anim.sh`, creates the systemd unit, and prepares the
folder tree.

## Menu

| Entry | What it does |
| --- | --- |
| Install boot splash | Writes the player, the service, and the folder tree |
| Select splash | Picks the media, and sets the duration to match it |
| Download PNG sequence | Browses this repository and keeps what you download |
| Splash duration | How long the splash runs, in seconds |
| View splash | Plays it now, without stopping EmulationStation |
| Enable / Disable splash | Keeps everything installed, skips playback |
| Uninstall boot splash | Removes the player and the service, keeps your files |

Selecting a media sets the duration to its length plus half a second, rounded
up. Change it afterwards if you want it to loop or to be cut short.

## Folder tree on the console

Everything lives under `/roms/bootsplash`:

```
random/     pool, one file drawn at random on each boot
sequence/   PNG frames of the current animation
themes/     downloaded themes, kept between changes
.raw/       decoded frames, rebuilt when the selection changes
selected    current selection
duration    splash length in seconds
```

Drop your own videos, GIFs or images at the root of `/roms/bootsplash` and they
show up in **Select splash**. Put several in `random/` if you want a different
one on every boot.

## Contents of this repository

| Folder | What goes in it |
| --- | --- |
| `gif/` | Animated GIFs, one file per animation |
| `mp4/` | Videos: mp4, mkv, webm, avi |
| `sequences/` | One subfolder per animation, holding numbered PNG frames |

The download menu reads these three folders directly, so anything added here
becomes available on the console. Sequence frames are renumbered to `%04d.png`
on download, which means the alphabetical order matches the playing order
whatever the original names were.

## License

MIT
