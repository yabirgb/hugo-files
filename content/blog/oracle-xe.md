+++
title = 'Using Oracle DB in Ubuntu without dying'
date = '2018-12-21'
author = 'Yábir García'
tags = ['oracle', 'university']
+++

## Introduction

At my subject of fundaments of databases we use Oracle as database (yes I hate 
it). I don't have a windows machine and installing all the stuff 
from oracle to make my exercises is pure pain to me. So this is a guide to have 
fun while installing it.

What we are going to use is docker to put in a container Oracle XE and 
we'll install Oracle SQL Developer. 

You need an account in the Oracle system to download this tool.

## Install dependencies

We need to install docker. A guide for your system can be found 
in [it's offcicial webpage](https://docs.docker.com/install/linux/docker-ce/ubuntu/).
At the side nav you can find your distro.

Probably you'll need to add your user to the docker group. As described 
[here](https://techoverflow.net/2017/03/01/solving-docker-permission-denied-while-trying-to-connect-to-the-docker-daemon-socket/)
, you can do it with 

    sudo usermod -a -G docker $USER

(you will need to log-out and log-in back or reboot your computer so the changes
takes effect. If you used snap to install docker follow [this steps](https://askubuntu.com/a/990058) )

Once you have docker installed we'll use a docker image for the Oracle XE.
The one I used is [this from wnameless](https://hub.docker.com/r/wnameless/oracle-xe-11g/).

What you need to do is

    docker pull wnameless/oracle-xe-11g

this will download the container and then run it

    docker run -d -p 49161:1521 wnameless/oracle-xe-11g


The last one will run the database listening on port 49161.
As described in the container page the details for the db are

    hostname: localhost
    port: 49161
    sid: xe
    username: system
    password: oracle

This method is clean and your computer won't be filled with dependencies
and things that at least I'm not going to use soon (or atleast I don't want).

To install the Oracle SQL developer I followed [this guide](https://askubuntu.com/questions/458554/how-to-install-sql-developer-on-ubuntu-14-04)
where what you need to do is

1. Install java if you don't have it

        sudo add-apt-repository ppa:webupd8team/java
        sudo apt-get update
        sudo apt-get install oracle-java8-installer
        sudo update-alternatives --config java

2. Download SQL Developer from the [official page](http://www.oracle.com/technetwork/developer-tools/sql-developer/downloads/index.html)
3. Extract files

        sudo unzip sqldeveloper-*-no-jre.zip -d /opt/
        sudo chmod +x /opt/sqldeveloper/sqldeveloper.sh

4. Create a symbolic link to the app

        sudo ln -s /opt/sqldeveloper/sqldeveloper.sh /usr/local/bin/sqldeveloper

5. Edit the start script at `/opt/sqldeveloper/sqldeveloper.sh` so it contains

        #!/bin/bash
        unset -v GNOME_DESKTOP_SESSION_ID
        cd /opt/sqldeveloper/sqldeveloper/bin && bash sqldeveloper $*

6. Run sqldeveloper

        sqldeveloper
    
You can create a icon in your launcher too but we aren't not using this again so.

You can stop the docker container using the `stop` option. 
To list your containers use `docker ps`.

After this you can configure SQL Developer to connect on port `49161` with
the credentials provided and you 

Note: If the cursos is not showing in the SQL Developer go to 

    Tools > Preferences > Code Editor > Fonts 

and chage the font size (I use `14`)
