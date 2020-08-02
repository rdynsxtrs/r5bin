# Apache 2.2.34

## Purpose
This package will update the version of the Apache web server on the ReadyNAS to
version 2.2.34.
This version supports TLS v1.2 and thus will make the web interface of the ReadyNAS
work with modern browsers again.

## Installation

### Through the ReadyNAS web interface (preferred)

The preferred method of installation is through the web interface of the ReadyNAS.
This method will install all needed packages and also automatically updates the 
`httpd.conf` in `/etc/frontview/apache` to only use TLSv1.2 connections.

**Note:** Although this looks like an add-on it **won't show up** in the list of
installed apps after installation. The reason for this is that only system 
packages are updsated. There's no additional functionality that needs configuration
through the web interface.

### Using the command line via SSH

**Prerequisites:** 
* SSH access to the ReadyNAS mus be anbled for this to work. This can be done using the
  `EnableRootSSH` add-on.
* You need an SSH client to access the command line of your ReadyNAS

1) Copy the files from the `/debs` folder to the home directory of the `root` user on
   your ReadyNAS. Either use an SFTP client or a command similar to this on the command
   line:

   ``` bash
   scp debs/*.deb root@<ip_of_your_readynas>:
   ````
   
1) Open an ssh session to your ReadyNAS as user `root`. The password is the same as the one
   you're using to access the admin web interface of your ReadyNAS. On the commandline you'd do:

   ``` bash
   ssh root@<ip_of_your_readynas>
   ```

1) Install the Debian packages:

   ``` bash
   dpkg -i *.deb
   ```

1) Remove the outdated protocols from Apache's confiuguration

   ``` bash
   sed -ri 's/^(SSLProtocol.*)$/\1 -TLSv1 -TLSv1.1/g' /etc/frontview/apache/httpd.conf
   ```

1) Test Apache's configuration

   ``` bash
   apachectl -t -t /etc/frontview/apache/httpd.conf
   ```

1) If there are no erros reported, e.g. the output of step #5 ends with `Syntak ok` restart
   the Apache web server:

   ``` bash
   apachectl -k restart -f /etc/frontview/apache/httpd.conf
   ```
