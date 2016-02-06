+++
date = 2016-02-05T20:57:18-05:00
draft = false
title = Logging with Docker and Loggly
+++







In part 2 of this series, I'm going to explore using Loggly to aggregate my Docker container logs.

Now I'm partial to taking the path to least resistance, and as such I'm going to follow Loggly's own documentation on using the [Docker Logging Driver](https://www.loggly.com/docs/docker-logging-driver/).

From what I understand about using Docker on a Mac, the docker-machine would be where I would have to SSH into in order to perform step 1 (Configure Syslog Daemon).

SSH'ing into my default docker-machine via `docker-machine ssh default` and attempting to execute `sudo bash configure-linux.sh -a SUBDOMAIN -u USERNAME` leaves me with an error informing me: `bash: command not found`. Okay, let's install bash then.

The default docker machine flavour of linux on mac is [Tiny Core Linux](http://www.tinycorelinux.net), so we can use their `tcl` command to bring up their package installer.

```
$ tce
$ s // to search
$ bash // to search for bash
$ 2 // to install bash.tcz
$ i // to confirm installation
```

Now that bash is installed, let's try to execute the configuration script again:

```
sudo bash configure-linux.sh -a SUBDOMAIN -u USERNAME
```

And we get...

```
Loggly account or subdomain: XXX
Username is set
Please enter Loggly Password:
```

Great! It's asking me for more. Enter the password and....

```
INFO: Initiating Configure Loggly for Linux.
configure-linux.sh: line 626: base64: command not found
configure-linux.sh: line 626: echo: write error: Broken pipe
ERROR: Base64 decode is not supported on your Operating System. Please update your system to support Base64.
```

Well that's no good. After searching for WAY to long on how to install base64 on TCL I came up empty.

Unfortunately it seems as though the Docker Logging Driver for Loggly just isn't an option for me.

Next up I'll try to [manually configure rsyslog](https://www.loggly.com/docs/rsyslog-manual-configuration/) instead of using the setup script.