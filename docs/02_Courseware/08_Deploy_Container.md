## Overview
Our custom image has now been created and is currently sitting in our local repository. Let's instantiate a container based on that image.

## Start a Container
To start a container from our image is very simple.  The only thing we need to remember is exposing the internal port to the host.

```ps
docker run -d -p 8080:80 --name 'web_8080' test/simpleweb 
docker run -d -p 8081:80 --name 'web_8081' test/simpleweb
docker run -d -p 8082:80 --name 'web_8082' test/simpleweb
```

We've started 3 separated instances of our web server. We've bound the web server's internal port 80 to three host ports (e.g 8080-8082). We've also supplied meaningful names to our containers.  We can reference those containers by the names we've specified for easier management.  For example, we can restart or stop a container using it's name instead of the container id.

Check the running images:
```ps
docker ps
```

You should see something like the following:
```ps
CONTAINER ID        IMAGE               COMMAND                  CREATED              STATUS              PORTS                    NAMES
3d1929c8e1b5        test/simpleweb      "C:\\ServiceMonitor..."  3 seconds ago        Up 2 seconds        0.0.0.0:8082->80/tcp     web_8082
323a65fa5143        test/simpleweb      "C:\\ServiceMonitor..."  11 seconds ago       Up 10 seconds       0.0.0.0:8081->80/tcp     web_8081
7d4fee5c8f89        test/simpleweb      "C:\\ServiceMonitor..."  About a minute ago   Up 59 seconds       0.0.0.0:8080->80/tcp     web_8080
```

Notice that all three containers are running, but, as we've specified, are bound to different ports and have custom names.

For practice, restart `web_8081`:
```ps
docker restart web_8081
```

Executing the command, may take a second. After it completes, check the running images again. You should now see that the uptime for `web_8081` is less than the other two containers.

We have now successfully created three container instances running our custom image.

## View the Container Websites
Before we attempt to expose our sites to the outside world, let's make sure that we can access them locally on the VM.

  1. Open a web browser on the virtual server and try to navigate to `http://localhost:8080` (our `web_8080` container).  Oops. Is seems we received an error.  What did we do wrong?  Let's investigate.

  2. Look again at the output of the `docker ps` command.

  3. Notice the _Ports_ column.  Our external port is not mapped to the loopback address (e.g. `127.0.0.1` or `localhost`).  Long story short, this is due to a way Windows maps its network interfaces.

  4. We need to get the actual, virtual IP address of the container.  To do this, type the following at the PowerShell prompt changing the container name for each running container:
  ```ps
  docker inspect --format '{{ .NetworkSettings.Networks.nat.IPAddress }}' web_8080
  ```

  5. Now, let's use the returned IP instead of the `localhost` to load our website.  In the browser change the URL to `http://<web_8080's virtual IP address:8080>` (e.g. http://172.26.67.126:8080).  This should display our _Hello World_ sample web site.