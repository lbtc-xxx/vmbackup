vmbackup
========

This Ant script aims to automate daily cold backup of a virtual machine running on VMware Fusion to Time Capsule.

Before use, edit variables in build-props.xml to suit your environment and requirement.

Prerequisites
-------------

Requires JRE and Apache Ant (Tested with version 1.9.4).

Main targets
------------

These targets are intended to use regularly.

- stop-backup-start
	1. Stop the virtual machine and wait for completion
	2. Mount the backup destination directory in Time Capsule
	3. Copy the virtual machine to mounted directory
	4. Unmount the directory
	5. Start the virtual machine
- purge
	1. Mount the backup destination
	2. Purge older backups that aged than ${prop.preserves}. see build-props.xml for detail
	3. Unmount the directory

Crontab example
---------------

My crontab example which runs purge and backup at every next morning of weekday (means every AM 07:02 of Tuesday - Saturday) as follows:

	ANT_HOME=/Users/kyle/apache-ant-1.9.4
	JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0_31.jdk/Contents/Home
	JAVA_CMD=$JAVA_HOME/bin/java
	2 7 * * 2-6 $ANT_HOME/bin/ant -f /Users/kyle/build.xml purge stop-backup-start 2>&1 | logger -p local0.notice

How backup destination will be
------------------------------

	$ tree -L 2 /Volumes/TimeCapsule/Finance
	/Volumes/TimeCapsule/Finance
	├── 20150129_180011
	│   └── CentOS\ (64\ bit).vmwarevm
	├── 20150129_222220
	│   └── CentOS\ (64\ bit).vmwarevm
	└── 20150131_060244
		└── CentOS\ (64\ bit).vmwarevm
		
	6 directories, 0 files
