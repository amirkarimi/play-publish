# Play Publish

Scripts and guides to automatically build and publish a Play application binaries on a Linux server

I used these bash scripts to publish a Play application in a single step. Before we can use the one-step publish script we have to prepare the server and development machines.

> The scripts and commands are written and tested against Ubuntu 14.04

## Server Side

Before everything we need the following packages to be installed on server side:

* git
* JRE 6+ (8+ is required if you use Play 2.4+)

### Copying the files
You need to copy the following files to the server:

* addwebapp
* play-init-script
* addwebapp

Make sure that `addwebapp` and `play-init-script` are in the same directory. 

You will run the following commands in the path you copied the files.

### Setting Up a New Web App

Connect to your server using `ssh` and run the following command in the path you copied the files (where `[your-app-name]` is the name of your application):

```bash
./addwebapp [your-app-name]
```

This command will setup a bare repository for binaries and create some folders as publish destination which is cloned from the bare repository.

You are ready to go!

### Change Application Server Port
Sometimes you deploy more than one application on a server. In this case you have to use different port numbers for each application.

Default port number is 9000 but you can change it in `/etc/init.d/play-[your-app-name]`. Open the file for edit and change the following line (where `[port-no]` is the port number):

```
APP_ARGS="-Dhttp.port=[port-no]"
```

### Remove Web App (Optional)
You can remove the previously setup web app by copying `rmwebapp` file to the server and running the following command:

```bash
./rmwebapp [your-app-name]
```

## Development Side

To be able to publish the binaries to the server you have to setup the publish script on your development machine.

### Copy and Update Publish Script

Put the `publish` script file into your Play application source folder (where `app` folder leaves) and set the following variables:

```
APP_NAME="[app-name]"       # The application name you gave in the previous steps
REMOTE_HOST="[ip or host]"  # SSH IP or host name
REMOTE_PORT="22"            # SSH Port number
REMOTE_USER="[user]"        # SSH user name
```

Make sure the specified user has the shell access without password. You can use `ssh-copy-id` command to obtain this.

### Publish

There you go!

You can publish your Play application by running the publish script:

```
./publish
```

Enjoy ;)
