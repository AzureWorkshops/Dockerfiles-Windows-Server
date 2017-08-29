## Overview
Now that we have our Dockerfile, let's build our image from it.

## Build the Docker Image
Once we have our Dockerfile, building the image is pretty simple.

From the PowerShell window, type the following:
```ps
Get-Content "c:\users\localadmin\desktop\Dockerfile.txt" | docker build -t test/simpleweb -
```

This will build an image using `test/simpleweb` as the repository name. We are using PowerShell's `Get-Content` command to read the contents of our previously created Dockerfile and then pipe them to Docker's build command.

Due to the size of Windows Server, the initial build will take some time because it must download the base image first.  Watch how Docker will step through our Dockerfile to build our image. Keep in mind while you watch this process that each step in our Dockerfile constitutes a layer in our image.  We'll see the results of this below.

## Check Your Images
From the command prompt, type the following:
```ps
docker images
```

You should see something similar to:
```ps
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
test/simpleweb      latest              9f4ec58ca830        3 minutes ago       11.1GB
microsoft/iis       latest              4f803ffceb53        37 hours ago        10.6GB
```

Our image has been built using the specified repository name. You'll also notice that the `microsoft/iis` image has been downloaded.  This is because the build process required Windows Server with IIS in order to build our image.  Now that our image has been built, you could delete the `microsoft/iis` image if you wanted to.  Finally, when looking at the image sizes, you'll see that our image is 500MB larger due installation Microsoft.NET, ASP.NET and other dependencies. 

Be aware that the Nano Server image is only 1.07GB compared to the full Windows Server at 10.6GB which makes Nano Server more ideal for containers.  It's really not the best scenario to use Windows Server in production as downloading 10.6GB and deploying that across your enterpise could consume a lot of bandwidth.  For production, it's best to opt for Nano Server.  But, again, Nano Server requires a little more preparation that extends a tad further beyond the scope of this workshop.

## View Image History
What if we wanted to see how our image is constructed? Or, what if we wanted to see exactly how much disk space each layer of our image required? We could find this out by checking the image's history.

```ps
docker image history test/simpleweb
```

When you run the above command, you see each command along from our Dockerfile along with it's layer id and the space requirements, if any.

We've now built a custom image based on a Dockerfile. We can use our custom image to deploy containers locally. Or, we could upload our image to a central repository so that others could leverage our image's functionality.