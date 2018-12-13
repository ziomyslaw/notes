# Run Visual Studio Code from Docker on Windows

## X Server on Windows - VcXsrv (or XMing)

### VcXsrv
Install VcXsrv server.
``` ps
choco install -y vcxsrv
```

Setup multiwindow with no access control
``` ps 
$env:path="$env:ProgramFiles\VcXsrv;$env:path"
vcxsrv -multiwindow -ac
```

### XMing

Install XMing server.
``` ps
choco install -y xming
```

Setup multiwindow with no access control
``` ps
xming :0 -ac -clipboard -multiwindow
```

## VS Code  

Create dockerfile 
``` 
FROM ubuntu:16.04
RUN apt-get update && apt-get install -y code
CMD /usr/bin/code
```

Build image

``` ps
docker build -t code .
```

Run container
``` ps
set-variable -name DISPLAY -value host_ip:0.0 //host_ip = windows machine's network adapter ip address
docker run -it --rm -e DISPLAY=$DISPLAY code
```

tip for linux
```
docker run -it \
    --rm -e DISPLAY=$DISPLAY \
    -v /tmp/.X11-unix:/tmp/.X11-unix \
    -v $XAUTHORITY:/home/developer/.Xauthority \ 
    --net=host \
    code
```


Links:
* [visual-studio-code-served-from-docker](http://blog.ctaggart.com/2016/05/visual-studio-code-served-from-docker.html)
* [windows-10-docker-host-display-gui-application-from-linux-container](https://stackoverflow.com/questions/40024892/windows-10-docker-host-display-gui-application-from-linux-container?rq=1)
* [docker-containers-on-the-desktop](https://blog.jessfraz.com/post/docker-containers-on-the-desktop/)
