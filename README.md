# docker-slic3r-prusa3d

Containerized Slic3r (Prusa3D version/fork) - Dockerfile and supporting script

The header of the Dockerfile has more documentation, but to grab and run this (built on the Docker hub):

`docker run -v /tmp/.X11-unix:/tmp/.X11-unix -v $PWD:/Slic3r/3d:z -v slic3rSettings:/home/slic3r -e DISPLAY=$DISPLAY --rm keyglitch/docker-slic3r-prusa3d`

* Your current directory will be mounted into the container at /Slic3r/3d (change the :z option as appropriate to :rw/:ro).

* Settings are persisted into the slic3rSettings volume.

* SELinux might block access to X by slic3r inside the container. Look in /var/log/messages or /var/log/audit/audit.log to see if this is happening (and for the relevant commands to fix).

* Alternatively, there might be a plain permission error upon trying to access X. Try running `xhost local:root` to fix (this is a temporary fix and must be reapplied when restarting the host).

Sample error example (it\'s pretty cryptic, sigh):

    No protocol specified
    19:13:54: Error: Unable to initialize GTK+, is DISPLAY set properly?
    Failed to initialize wxWidgets at /Slic3r/slic3r-dist/lib/site_perl/5.22.0/Slic3r/GUI/2DBed.pm line 10.
    Compilation failed in require at /Slic3r/slic3r-dist/lib/site_perl/5.22.0/Slic3r/GUI/2DBed.pm line 10.
    BEGIN failed--compilation aborted at /Slic3r/slic3r-dist/lib/site_perl/5.22.0/Slic3r/GUI/2DBed.pm line 10.
    Compilation failed in require at /Slic3r/slic3r-dist/lib/site_perl/5.22.0/Slic3r/GUI.pm line 9.
    BEGIN failed--compilation aborted at /Slic3r/slic3r-dist/lib/site_perl/5.22.0/Slic3r/GUI.pm line 9.
    Compilation failed in require at (eval 87) line 1.

### Building the latest release locally

If the hub version is ever out of date (or for any other reason), it is possible to build an image locally. `getLatestSlic3rRelease.sh` will automatically try to get the latest non-AppImage version through the GitHub API during the build process.

Building:

    $ docker build -t docker-slic3r-prusa3d .
    Sending build context to Docker daemon 73.73 kB
    Step 1 : FROM debian:stretch
    [ .. truncated ..]