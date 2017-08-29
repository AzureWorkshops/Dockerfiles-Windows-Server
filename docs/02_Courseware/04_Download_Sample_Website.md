## Overview
In this short step we will download our website - again, via PowerShell - into our default IIS directory.

## Download and Expand Site
The default folder for IIS is `C:\inetpub\wwwroot`.  This is the folder we will use for hosting our site.  Keep in mind that within a microservice architecture, a container has a single function or purpose.  So, while we theoretically _could_ host multiple sites in IIS, it's not best practice.  Moving away from VMs will require us to rethink how we're accustomed to doing things.

We have a sample static site that we'll download from GitHub as a .zip file and expand it into our target folder.

  1. If it's not still open from the previous step, open PowerShell with elevated privileges.
  2. Type the following commands:
     ```ps
     (new-object Net.WebClient).DownloadFile('https://github.com/AzureWorkshops/samples-simple-iis-website/archive/master.zip','D:\master.zip');
     Expand-Archive -LiteralPath D:\master.zip -Destination D:\
     (new-object -com shell.application).namespace('C:\inetpub\wwwroot\').CopyHere((new-object -com shell.application).namespace('D:\samples-simple-iis-website-master').Items(), 16);
     del c:\inetpub\wwwroot\iisstart*.*
     ```

After a couple of seconds, the file should download.  The above script downloads the .zip file from GitHub to the `D:\` temp drive (line 1); extracts the .zip file's contents (line 2) which, in-turn, creates a subdirectory of the extracted files; copies the files from the subdirectory to our IIS root folder (line 3); and, deletes the IIS placeholder files. 

Now, if you were to open Internet Explorer on the server (e.g. http://localhost/), you should see our website that simply displays a 'Hello World' page.