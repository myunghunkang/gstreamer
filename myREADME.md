
## Mandatory on the macOs
export GIO_EXTRA_MODULES=/Library/Frameworks/GStreamer.framework/Libraries/gio/modules/
export PATH=/Library/Frameworks/GStreamer.framework/Versions/1.0/bin:$PATH
export GST_DEBUG_DUMP_DOT_DIR=/Users/sarah/vscode/gstreamer/myDot/

### vscode
> dot preview : cmd+shift+v

## 1. Build Gstreamer Source
### basic rule
> cd /Users/sarah/vscode/gstreamer\
> meson setup build\
> meson compile -C build

### option configuration and resetup
> meson configure build -DFFmpeg:libv4l2=enabled -Dgst-plugins-bad:v4l2codecs=enabled -Dgst-plugins-good:v4l2=enabled -Dgst-plugins-good:v4l2-gudev=enabled\
meson setup build --reconfigure

## 2. Download Gstreamer SDK
### 2-1. SDK installation
> https://gstreamer.freedesktop.org/download/#macos\
macOS Universal (X86_64 & ARM64) 1.22 release (current stable version)\
> 1.22.4 runtime installer\
> 1.22.4 development installer
### Tell gio where to find the gio modules
> export GIO_EXTRA_MODULES=/Library/Frameworks/GStreamer.framework/Libraries/gio/modules/
### Verify
> gst-launch-1.0 playbin uri=https://www.freedesktop.org/software/gstreamer-sdk/data/media/sintel_trailer-480p.webm

> gst-launch-1.0 playbin uri=https://gstreamer.freedesktop.org/data/media/sintel_trailer-480p.webm

> gst-launch-1.0 -v souphttpsrc location=https://www.freedesktop.org/software/gstreamer-sdk/data/media/sintel_trailer-480p.webm ! matroskademux ! vp8dec ! videoconvert ! videoscale ! autovideosink

> gst-launch-1.0 -v souphttpsrc location=https://gstreamer.freedesktop.org/data/media/sintel_trailer-480p.webm ! decodebin ! autovideosink

> media files are here https://gstreamer.freedesktop.org/data/media/

### 2-2. Setup to compile and link
> https://gstreamer.freedesktop.org/documentation/installing/on-mac-osx.html?gi-language=c
#### Tell pkg-config where to find the .pc files
> $ export PKG_CONFIG_PATH=/Library/Frameworks/GStreamer.framework/Versions/1.0/lib/pkgconfig

#### We will use the pkg-config provided by the GStreamer.framework
> $ export PATH=/Library/Frameworks/GStreamer.framework/Versions/1.0/bin:$PATH

#### Compile
> $ clang -c main.c -o main.o `pkg-config --cflags gstreamer-1.0`

#### Link
> $ clang -o main main.o `pkg-config --libs gstreamer-1.0`



### How to inspect what parser, decoder are used for the title

> gst-discoverer-1.0 https://gstreamer.freedesktop.org/data/media/sintel_trailer-480p.webm -v

### How to inspect wether parser, decorder are installed in the system

> gst-inspect-1.0 vp8dec

### How to debug
#### Referrence
> https://gstreamer.freedesktop.org/documentation/tutorials/basic/debugging-tools.html?gi-language=c
> https://developer.ridgerun.com/wiki/index.php/GStreamer_Debugging

#### gst-launch
gst-launch-1.0 --gst-debug-help

GST_DEBUG=WARN,vp8dec:DEBUG gst-launch-1.0 playbin uri=https://gstreamer.freedesktop.org/data/media/sintel_trailer-480p.webm

GST_DEBUG_FILE=/tmp/gst.log GST_DEBUG_DUMP_DOT_DIR=/tmp GST_DEBUG=WARN gst-launch-
1.0 playbin uri=https://media.w3.org/2010/05/sintel/trailer.mp4 -v


