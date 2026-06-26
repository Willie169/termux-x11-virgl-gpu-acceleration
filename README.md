# termux-x11-virgl-gpu-acceleration

This repo is about installing and running VirGL server in Termux for GPU hardware acceleration in Termux and Proot environments on it with Termux:X11.

## Termux-X11

Termux-X11 can be installed by

<ol>
<li>Run in Termux:
<pre><code>pkg update
pkg install x11-repo -y
pkg install termux-x11-nightly -y
</code></pre>
</li>
<li>Download and install APK from <a href="https://github.com/termux/termux-x11">https://github.com/termux/termux-x11</a> release.</li>
</ol>

If you encounter `Make sure an X server isn't already running(EE)` error, close Termux and wait a few seconds, or **Force stop** and **Clear cache** Termux and Termux:X11 apps in Android system settings.

Termux:X11 can be started by running
```
termux-x11 :0
```
or in background
```
termux-x11 :0 &
```
where `0` can be replaced with other numbers.

## VirGL Server Installation

Run in Termux:
```
pkg update
pkg install x11-repo tur-repo -y
pkg install mesa-vulkan-icd-freedreno mesa-demos mesa-zink virglrenderer-mesa-zink -y
```

## VirGL Server

VirGL server can be started by running in Termux:
```
MESA_NO_ERROR=1 MESA_LOADER_DRIVER_OVERRIDE=zink MESA_GL_VERSION_OVERRIDE=4.3COMPAT MESA_GLES_VERSION_OVERRIDE=3.2 GALLIUM_DRIVER=zink ZINK_DESCRIPTORS=lazy virgl_test_server --use-egl-surfaceless --use-gles
```
or in background
```
MESA_NO_ERROR=1 MESA_LOADER_DRIVER_OVERRIDE=zink MESA_GL_VERSION_OVERRIDE=4.3COMPAT MESA_GLES_VERSION_OVERRIDE=3.2 GALLIUM_DRIVER=zink ZINK_DESCRIPTORS=lazy virgl_test_server --use-egl-surfaceless --use-gles &
```

You may add
```
MESA_NO_ERROR=1 MESA_LOADER_DRIVER_OVERRIDE=zink MESA_GL_VERSION_OVERRIDE=4.3COMPAT MESA_GLES_VERSION_OVERRIDE=3.2 GALLIUM_DRIVER=zink ZINK_DESCRIPTORS=lazy virgl_test_server --use-egl-surfaceless --use-gles
```
in your `~/.bashrc` to start it on Termux start automatically.

## VirGL Rendering

To run a command with VirGL rendering, first start TermuxvX11 and VirGL server in Termux, and then, for native Termux, run the command with
```
GALLIUM_DRIVER=zink MESA_GL_VERSION_OVERRIDE=4.3 <command>
```
and for proot-distro, `--shared-tmp` option must be used when logging in, and run the command with
```
DISPLAY=<display> GALLIUM_DRIVER=zink MESA_GL_VERSION_OVERRIDE=4.3 <command>
```
where `<display>` is the display of Termux:X11, e.g., `:0`.

You may also run or add to `~/.bashrc`:
```
export GALLIUM_DRIVER=zink
export MESA_GL_VERSION_OVERRIDE=4.3
```

## References

- <https://ivonblog.com/en-us/posts/termux-virglrenderer>
- <https://github.com/LinuxDroidMaster/Termux-Desktops/issues/68>
