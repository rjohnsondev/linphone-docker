Linphone Docker image for Ubuntu 19.10
======================================

So linphone is a pain. The bundled versions are horribly out of date, and
building it from source is an extercise in futility. The sheer number of build
dependencies to build would make anyone hesitate.

Encapsulating this kind of mess is exactly what docker was designed for.
However, there are a few gotchas to get things like X11 and Pulse to work
correctly. Thankfully, some projects like
https://github.com/TheBiggerGuy/docker-pulseaudio-example are there to guide
us.

Also note that simply downloading all of the modules and submodles takes a
long, long time. BUILDKIT is a godsend here.

So: here it is, a docker image that will build linphone, keeping your host
machine cleanish and with only minor breakage!

Here are some of the magic incantations. To build:

`DOCKER_BUILDKIT=1 docker build . -t rjohnsondev/linphone`

And to run:

```
docker run \
    --env="DISPLAY" \
    -e QT_QUICK_BACKEND="software" \
    -e Qt5_DIR="/usr/lib/x86_64-linux-gnu/qt5/mkspecs/features/data/cmake" \
    -e PATH='/usr/lib/x86_64-linux-gnu/qt5/bin':$PATH \
    -v /tmp/.X11-unix:/tmp/.X11-unix \
    -v "/run/user/1000/gdm/Xauthority:/run/user/1000/gdm/Xauthority:rw" \
    -v "/run/user/1000/pulse:/run/user/1000/pulse" \
    rjohnsondev/linphone

```

Things that don't work:
 * Editing of Accounts & the About dialog use 3d effect that obscure the vital
   edit box and widgets when openned.
