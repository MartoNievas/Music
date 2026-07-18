# Musualizer

A music visualizer application built with Raylib that provides real-time audio visualization with an integrated file browser and playback controls.

## Features

- Real-time audio visualization
- System audio (loopback) visualization mode
- Built-in file browser for music selection
- Comprehensive keyboard shortcuts
- Hot reload support for development
- Fullscreen mode support

## Quick Start

### Building the Application

```bash
$ make
$ ./build/music
```

### Hot Reloading (Development Mode)

For development with hot reloading enabled:

```bash
$ export HOTRELOAD=1
$ export LD_LIBRARY_PATH="./build/:$LD_LIBRARY_PATH"
$ make
$ ./build/music
```

## Keyboard Shortcuts

### General Playback

| Key | Action |
|-----|--------|
| `SPACE` | Play / Pause |
| `M` | Mute / Unmute Toggle |
| `N` | Next Track in Playlist |
| `P` | Previous Track in Playlist |
| `F` | Toggle Fullscreen Mode |
| `S` | Toggle System Audio Mode |

### System Audio Mode

Press `S` to switch between the playlist and visualizing whatever the system is currently playing (any application, not just this player), captured from the default audio sink's monitor source via `parec`.

While in this mode, no other playback shortcut works except pausing the visualization itself:

| Key | Action |
|-----|--------|
| `SPACE` | Pause / Resume the visualization |
| `S` | Return to the playlist |

Fullscreen is switched on automatically while visualizing system audio. Whatever fullscreen state you had before is restored when you go back to the playlist.

This mode requires PulseAudio or PipeWire (with `pipewire-pulse`) and the `parec` / `pactl` command-line tools available on the system. See Requirements.

### Internal File Browser

Access the file browser by pressing `O` or clicking the folder icon. It also opens automatically the first time you click on the empty-state screen.

| Key | Action |
|-----|--------|
| `O` | Toggle File Browser |
| `BACKSPACE` | Navigate to Parent Directory (Go Up) |
| `ESC` | Close Browser |
| `MOUSE WHEEL` | Scroll through file list |
| `LEFT CLICK` | Select track or enter folder |

The browser opens by default in the desktop's configured music folder (resolved via `xdg-user-dir MUSIC`), falling back in order to `~/Music`, `~/Música`, `~/Musica`, `$HOME`, and finally the current directory.

## Requirements

- [Clang](https://clang.llvm.org/) - the Makefile invokes it directly (`CC = clang`)
- [Raylib](https://www.raylib.com/) - graphics and audio library
- POSIX-compatible system (Linux/macOS)
- For System Audio Mode: PulseAudio or PipeWire (`pipewire-pulse`), plus the `parec` / `pactl` command-line tools (commonly packaged as `pulseaudio-utils` / `libpulse`)

## References

This project is built upon the following resources:

- [nob.h](https://github.com/tsoding/nob.h/blob/main/nob.h) - Build system
- [Raylib](https://www.raylib.com/) - Graphics and audio library
- [Musializer](https://github.com/tsoding/musializer/blob/9d822424be0d555ab70d4e9356ba26e3e52b1916/src/musializer.c) - Original inspiration
- [FFT Implementation in C](https://github.com/muditbhargava66/FFT-implementation-in-C/tree/main/fft) - Fast Fourier Transform algorithm

## Project Structure

```
.
├── build/              # Compiled binaries
├── Makefile            # Build rules (standard and hot-reload modes)
├── resources/          # Fonts, icons and shaders
├── src/                # Source files
└── README.md           # This file
```

## Development

The application supports hot reloading during development. When `HOTRELOAD=1` is set, you can modify the code and rebuild without restarting the application.

## Changelog

- Added System Audio Mode (`S`): visualize the system's audio output via the default sink's monitor source, instead of only files loaded into the playlist.
- Restricted input while in System Audio Mode to just pausing the visualization, since there is no track to seek, mute, or skip.
- Replaced the OS-native "select file" dialog with the built-in file browser on the initial empty-state click; the native dialog silently fell back to a broken console prompt on systems without `zenity` / `kdialog` installed.
- Fixed the default browsing folder: it now resolves the desktop's actual configured music folder instead of a hardcoded, often nonexistent path.
- Fixed a crash when toggling System Audio Mode with no track loaded.
- Fixed the fullscreen state not being restored after leaving System Audio Mode.
- Fixed the visualization bars reserving space for a UI bar that never appears while in System Audio Mode.

## License

Please refer to the original project licenses for the components used in this application.

## Contributing

Contributions are welcome! Please feel free to submit pull requests or open issues for bugs and feature requests.

---

**Note:** This is a music visualization application. Make sure you have the appropriate audio files and codecs installed on your system for optimal playback.
