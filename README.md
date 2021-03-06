# ProcessCap
## What Is it?
This script is intended to be run in a cron, and will kill a specified process when its memory consumption goes over a certain amount.

I originally wrote this to address a bug in OS X Mavericks (10.9.2) whereby the `com.apple.IconServicesAgent` would eat up lots of memory, and never free it. This script can be used to periodically kill the process when its memory consumption gets out of control, but I'm sure there are other uses as well.

(note: It seems the `com.apple.IconServicesAgent` issue has been resolved in Yosemite, but I’ve no doubt this script might be useful for something else!)

## Setup
To install:

```shell
git clone git://github.com/heliomass/ProcessCap.git
```

## Usage
To see the full range of commands:

```shell
process_cap --help
```

To kill `com.apple.IconServicesAgent` when its memory consumption reaches 40Mb:

```shell
process_cap --process_name com.apple.IconServicesAgent --process_cap 40
```

If you have [Growl](http://growl.info) installed and also [Growl Notify](http://growl.info/downloads#generaldownloads) as well, you can have ProcessCap generate a Growl notification when it kills the process, for example:

```shell
process_cap --process_name com.apple.IconServicesAgent --process_cap 40 --use_growl
```

To set up a cron, use something such as:

```
*/10 * * * * /bin/bash -c "export PATH=$PATH:/usr/local/bin; $HOME/bin/process_cap --process_name com.apple.IconServicesAgent --process_cap 40 --use_growl >> $HOME/Logs/process_cap.log 2>&1"
```

This will do the following:

• Run the script every 10 minutes (assumes the script lives at `$HOME/bin/process_cap`. If not, change this)

• Kill `com.apple.IconServicesAgent` when it reaches 40Mb

• Provide a Growl notification each time a process is killed

• Log its activity in `$HOME/Logs/process_cap.log` (change this or create the directory with: `mkdir $HOME/Logs`)

(note that the `export PATH=$PATH:/usr/local/bin` command is only needed if you're using the `--use_growl` switch, as this is the default location of the `growlnotify` command)
