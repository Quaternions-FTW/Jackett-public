## Jackett Public

This project is a fork of Jackett/Jackett based on version 0.6.45.0, the latest version supporting public trackers and modified to be able to run 2 instances at the same time.
* One with old publics trackers
* The official one with new private trackers
If you want to work on this fork and add others, feel free!

## How-To

You can easily get 2 applications folders with different names (Jackett and Jackett-public for example)
But by default, it will use the same config folder: /home/{username}/.config/Jackett
This fork will use: /home/{username}/.config/Jackett-public

* Install this application into /opt/Jackett-public
* Copy /home/{username}/.config/Jackett to /home/{username}/.config/Jackett-public
* Remove the indexers folder
* Update the config to change port
* Update the config to change API Key and Instance ID (just add a custom one)
* Copy your launching service and update it too
* Reboot

## Supported Systems
* Linux and OSX using Mono 4 (v3 should work but you may experience crashes).

## Supported Trackers
 * [AlphaRatio](https://alpharatio.cc/)
 * [AnimeBytes](https://animebytes.tv/)
 * [AnimeTorrents](http://animetorrents.me/)
 * [Avistaz](https://avistaz.to/)
 * [BakaBT](http://bakabt.me/)
 * [bB](http://reddit.com/r/baconbits)
 * [BeyondHD](https://beyondhd.me/)
 * [BIT-HDTV](https://www.bit-hdtv.com)
 * [BitMeTV](http://www.bitmetv.org/)
 * [BlueTigers](https://www.bluetigers.ca/)
 * [BTN](http://broadcasthe.net)
 * [Demonoid](http://www.demonoid.pw/)
 * [EuTorrents](https://eutorrents.to/)
 * [FileList](http://filelist.ro/)
 * [FrenchTorrentDb](http://www.frenchtorrentdb.com/)
 * [Freshon](https://freshon.tv/)
 * [HD-Space](https://hd-space.org/)
 * [HD-Torrents.org](https://hd-torrents.org/)
 * [Immortalseed.me](http://immortalseed.me)
 * [IPTorrents](https://iptorrents.com/)
 * [MoreThan.tv](https://morethan.tv/)
 * [NextGen](https://nxtgn.org/)
 * [pretome](https://pretome.info)
 * [PrivateHD](https://privatehd.to/)
 * [RARBG](https://rarbg.to/)
 * [RuTor](http://rutor.org/)
 * [SceneAccess](https://sceneaccess.eu/login)
 * [SceneTime](https://www.scenetime.com/)
 * [Shazbat](www.shazbat.tv/login)
 * [ShowRSS](https://showrss.info/)
 * [Strike](https://getstrike.net/)
 * [T411](http://www.t411.io/)
 * [TehConnection](https://tehconnection.eu/) 
 * [The Pirate Bay](https://thepiratebay.se/)
 * [TorrentBytes](https://www.torrentbytes.net/)
 * [TorrentDay](https://torrentday.eu/)
 * [TorrentLeech](http://www.torrentleech.org/)
 * [TorrentShack](http://torrentshack.me/)
 * [Torrentz](https://torrentz.eu/)
 * [TV Chaos UK](https://tvchaosuk.com/)

## Compiling jackett-public

These instructions are to compile on a windows host

* Install chocolatey pkg manager

* Install build dependancies
	* `choco install -y sourcetree mono visualstudio2015community innosetup 7zip`

* Add 'C:\Program Files (x86)\Inno Setup 5' to your system's PATH environment variable
	* Log out your user, log back in again to refresh $PATH (or kill / restart explorer.exe)
	* Open a cmdline and check that 'iscc.exe' is found / can be executed

* Use sourcetree to 'git clone' this repo raspdealer/Jackett

* Open Visual Studio
	* Open the src/ subfolder, select 'Jackett.sln' project file
	* Go to Build menu --> Build Solution

* Open a new cmd prompt, cd into git Jackett repo folder

* run `Build-mono.bat`
	* This generates some product files, including Output/setup.exe, and 'build.mono' folder

* Rename 'build.mono' folder --> 'Jackett-public'

* Right click in explorer --> Zip --> 'Add to archive'
	* Select 'tar' archive format
	* Name: 'Jackett-public.Binaries.Mono.tar'

* Copy file 'Jackett-public.Binaries.Mono.tar' to linux host
	* Run `gzip Jackett-public.Binaries.Mono.tar`

* Git tag the latest commit which was build from
	* Upload to github 'releases' page, add file under new version had just git-taged


## Install Jackett on Debian-based Linux (Ubuntu 14.XX+, Debian, etc) - Not to be used with other Jackett branches simultaneously

1. Open "terminal" application.
2. Install libcurl and bzip for unpacking Jackett

`sudo apt-get install libcurl4-openssl-dev bzip2 -y`

3. Download the latest Jackett release, I have automated grabbing the newest release but if the code below doesn’t work check [here](https://github.com/dreamcat4/Jackett-public/releases) to get the download (ending in .tar.gz)

`jackettver=$(wget -q https://github.com/dreamcat4/Jackett-public/releases/latest -O - | grep -E \/tag\/ | awk -F "[><]" '{print $3}')
wget -q https://github.com/dreamcat4/Jackett-public/releases/download/$jackettver/Jackett-public.Binaries.Mono.tar.gz`

4. Unpack the Jackett release (adjust the filename if you downloaded a newer version)

`tar -xvf Jackett*`

5. Make the Jackett installation folder

`sudo mkdir /opt/jackett`

6. Move the unzipped Jackett installation

`sudo mv Jackett-public/* /opt/jackett`

7. Change ownership of Jackett to your main user

`sudo chown -R username:username /opt/jackett`

8. Test running Jackett which runs on port 9117.

`mono /opt/jackett/JackettConsole.exe`

9. Type in your web browser or click [http://localhost:9117](http://localhost:9117)
10. Kill the Jackett process by typing Ctrl+C in terminal so we can start it automatically on boot using an init.d script

## Autostart Jackett on Debian/Ubuntu 14.XX+ - Jackett init.d Script

1. Create the Jackett init.d startup script

`sudo nano /etc/init.d/jackett`

2. Paste the Jackett init.d script (thank you Wesley), adjust your RUN_AS value to your main user

```
#! /bin/sh
### BEGIN INIT INFO
# Provides: Jackett
# Required-Start: $local_fs $network $remote_fs
# Required-Stop: $local_fs $network $remote_fs
# Should-Start: $NetworkManager
# Should-Stop: $NetworkManager
# Default-Start: 2 3 4 5
# Default-Stop: 0 1 6
# Short-Description: starts instance of Jackett
# Description: starts instance of Jackett using start-stop-daemon
### END INIT INFO

############### EDIT ME ##################
# path to app
APP_PATH=/opt/jackett

# user
RUN_AS=htpcguides

# path to mono bin
DAEMON=$(which mono)

# Path to store PID file
PID_FILE=/var/run/jackett/jackett.pid
PID_PATH=$(dirname $PID_FILE)

# script name
NAME=jackett

# app name
DESC=Jackett

# startup args
EXENAME="JackettConsole.exe"
DAEMON_OPTS=" "$EXENAME

############### END EDIT ME ##################

JACKETT_PID=`ps auxf | grep $EXENAME | grep -v grep | awk '{print $2}'`

test -x $DAEMON || exit 0

set -e

#Look for PID and create if doesn't exist
if [ ! -d $PID_PATH ]; then
	mkdir -p $PID_PATH
	chown $RUN_AS $PID_PATH
fi

if [ ! -d $DATA_DIR ]; then
	mkdir -p $DATA_DIR
	chown $RUN_AS $DATA_DIR
fi

if [ -e $PID_FILE ]; then
	PID=`cat $PID_FILE`
		if ! kill -0 $PID > /dev/null 2>&1; then
		echo "Removing stale $PID_FILE"
		rm $PID_FILE
	fi
fi

echo $JACKETT_PID > $PID_FILE

case "$1" in
start)
if [ -z "${JACKETT_PID}" ]; then
	echo "Starting $DESC"
	rm -rf $PID_PATH || return 1
	install -d --mode=0755 -o $RUN_AS $PID_PATH || return 1
	start-stop-daemon -d $APP_PATH -c $RUN_AS --start --background --pidfile $PID_FILE --exec $DAEMON -- $DAEMON_OPTS
	else
	echo "Jackett already running."
fi
;;
stop)
echo "Stopping $DESC"
echo $JACKETT_PID > $PID_FILE
start-stop-daemon --stop --pidfile $PID_FILE --retry 15
;;

restart|force-reload)
echo "Restarting $DESC"
start-stop-daemon --stop --pidfile $PID_FILE --retry 15
start-stop-daemon -d $APP_PATH -c $RUN_AS --start --background --pidfile $PID_FILE --exec $DAEMON -- $DAEMON_OPTS
;;
status)
# Use LSB function library if it exists
if [ -f /lib/lsb/init-functions ]; then
	. /lib/lsb/init-functions
if [ -e $PID_FILE ]; then
	status_of_proc -p $PID_FILE "$DAEMON" "$NAME" && exit 0 || exit $?
	else
	log_daemon_msg "$NAME is not running"
	exit 3
fi

else
# Use basic functions
if [ -e $PID_FILE ]; then
	PID=`cat $PID_FILE`
if kill -0 $PID > /dev/null 2>&1; then
	echo " * $NAME is running"
	exit 0
fi
else
	echo " * $NAME is not running"
	exit 3
fi
fi
;;
*)
N=/etc/init.d/$NAME
echo "Usage: $N {start|stop|restart|force-reload|status}" >&2
exit 1
;;
esac

exit 0
```

3. Make the Jackett init.d script executable

`sudo chmod +x /etc/init.d/jackett`

4. Update your system to use the Jackett init.d script

`sudo update-rc.d jackett defaults`

5. Now start the Jackett service

`sudo service jackett start`

6. Now Jackett will automatically start on boot-uo. Check that it is running by typing in your web browser or click [http://localhost:9117](lhttp://localhost:9117)
