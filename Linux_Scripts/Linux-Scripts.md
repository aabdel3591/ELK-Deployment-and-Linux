# **Linux Scripts**


## **A High Stakes Investigation:**

`#! /bin/bash/`

#### $1 must have “ ” because the script is expecting a string.
#### $2 is the date.
#### $3 is the time $4 is AM/PM
#### Time must be formatted with a proceeding 0 where applicable (Ex: 05:00:00 PM)
	
`Echo “The dealer for $1 on $2 at $3 $4 was:”`

#### Output based on game
`if [[ “BlackJack” -- “$1” ]]; then`

	cat $2_Dealer_schedule | grep -E $3, *$4 | awk -F” “ ‘[print $3, $4};

`fi`

`if [[ “Roulette” -- “$1” ]]; then`

	cat $2_Dealer_schedule | grep -E $3, *$4 | awk -F” “ ‘{print $5, $6};

`fi`

`if [[ “Texas_Hold_Em” -- “$1” ]]; then`
	
	cat $2_Dealer_schedule | grep -E $3, *$4 | awk -F” “ ‘{print $7, $8};

`fi`

#### End of script


## **Access Log:**

`#!/bin/bash`

#### Read access log and replace “Incorrect_Password” with “Access_Denied” and save to new file.
`cat LogA.txt | sed s/INCORRECT_PASSWORD/ACCESS_DENIED/ > Update1_Combined_Access_logs.txt`

#### Read access log and save fields 4 and 6 to new file. 
`cat Update1_Combined_Access_logs.txt | awk -F" " '{print $4, $6}' > Update2_Combined_Access_logs.txt`

#### End of script


## **Archiving & Logging Data: Script 1**

`#!/bin/bash`

#### Create /var/backup if it doesn't exist.
`mkdir -p /var/backup`

#### Create new `/var/backup/home.tar`.
`tar cvzf /var/backup/home.tar /home`

#### Moves the file `/var/backups/home.tar.gz` to `/var/backups/home.last.tar.gz`.
`mv /var/backups/home.tar.gz /var/backups/home.last.tar.gz`

#### Creates a gzip-compressed archive of `/home` and saves it to `/var/backups/home.tar.gz`.
`tar cvzf /var/backup/system.tar.gz /home` 	

#### List all files in `/var/backups`, including file sizes, and save the output to `/var/backups/file_report.txt`.
`ls -lh /var/backups > /var/backups/file_report.txt`

#### Print how much free memory your machine has left. Save this to a file called `/var/backups/disk_report.txt`.
`free -h > /var/backups/disk_report.txt`

#### End of script


## **Archiving & Logging Data: Script 2**

`#!/bin/bash`

#### Clean up temp directories
`rm -rf /tmp/*`

`rm -rf /var/tmp/*`

#### Clear apt cache
`apt clean -y`

#### Clear thumbnail cache for sysadmin, instructor, and student
`rm -rf /home/sysadmin/.cache/thumbnails`

`rm -rf /home/instructor/.cache/thumbnails`

`rm -rf /home/student/.cache/thumbnails`

`rm -rf /root/.cache/thumbnails`

#### End of Script


## **Archiving & Logging Data: Script 3**

`#!/bin/bash`

#### Ensure apt has all available updates, upgrade all installed packages, install new packages, and uninstall any old packages that must be removed to install them, and remove unused packages and their associated configuration files.
`sudo apt update -y && apt upgrade -y && apt full-upgrade -y && apt-get autoremove --purge -y`

#### End of script.


## **Archiving & Logging Data: Script 4**

`#!/bin/bash`

#### Free memory output to a free_mem.txt file
`free -h > ~/backups/freemem/free_mem.txt`

#### Disk usage output to a disk_usage.txt file
`du -h > ~/backups/diskuse/disk_usage.txt`

#### List open files to a open_list.txt file
`lsof  > ~/backups/openlist/open_list.txt`

#### Free disk space to a free_disk.txt file
`df -h --output=used,source,pcent > ~/backups/freedisk/free_disk.txt`

#### End of script


## **Bash Scripting & Programming: Script 1**

`#!/bin/bash`

#### Variables.
`nums=$(seq 0 9)`

`states=('Nebraska' 'California' 'Texas' 'Hawaii' 'Washington')`

`ls_out=$(ls)`

`suids=$(find / -type f -perm /4000 2> /dev/null)`

#### For Loops
#### A loop that adds 2 to each number and prints out the result.
`for num in ${nums[@]};`

`do`
	
	new_num=$(($num + 2))
  	echo $new_num

`done`

#### A loop that looks for 'New York'
`for state in ${states[@]};`

`do`

	if [ $state == 'New York' ];
		then
			echo "New York is the best!"
 		else
   			echo "I'm not a fan of New York."
	fi

`done`

#### A `for` loop that prints out each item in your variable that holds the output of the `ls` command.
`for x in ${ls_out[@]};`

`do`

	echo $x

`done`

#### A for loop to print out suids on one line for each entry.
`for suid in ${suids[@]};`

`do`

	echo $suid

`done`

#### End of script


## **Bash Scripting & Programming: Script 2**

`#!/bin/bash`

#### Check if script was run as root. Exit if true.
`if [ $UID -eq 0 ]`

`then`

	echo "Please do not run this script as root."
	exit

`fi`

#### Variables.
`output=$HOME/research/sys_info.txt`

`ip=$(ip addr | grep inet | tail -2 | head -1)`

`suids=$(sudo find / -type f -perm /4000 2> /dev/null)`

`cpu=$(lscpu | grep CPU)`

`disk=$(df -H | head -2)`

#### Lists to use later
`commands=(`

	'date'
	'uname -a'
	'hostname -s'

`)`

`files=(`

	'/etc/passwd'
	'/etc/shadow'
	'/etc/hosts'

`)`

#### Check for research directory. Create it if needed.
`if [ ! -d $HOME/research ]`

`then`

	mkdir $HOME/research

`fi`

#### Check for output file. Clear it if needed.
`if [ -f $output ]`

`then`
	
	> $output

`fi`


#### Start Script

`echo "A Quick System Audit Script" >> $output
echo "" >> $output`

`for x in {0..2} ;`

`do`

	results=$(${commands[$x]})
	echo "Results of "${commands[$x]}" command:" >> $output
	echo $results >> $output
	echo "" >> $output

`done`

#### Display Machine type.
`echo "Machine Type Info:" >> $output`

`echo -e "$MACHTYPE \n" >> $output`

#### Display IP Address info.
`echo -e "IP Info:" >> $output`

`echo -e "$ip \n" >> $output`

#### Display Memory usage.
`echo -e "\nMemory Info:" >> $output`

`free >> $output`

#### Display CPU usage.
`echo -e "\nCPU Info:" >> $output`

`lscpu | grep CPU >> $output`

#### Display Disk usage.
`echo -e "\nDisk Usage:" >> $output`

`df -H | head -2 >> $output`

#### Display who is logged in.
`echo -e "\nWho is logged in: \n $(who -a) \n" >> $output`

`echo -e "\nSUID Files:" >> $output`

#### Display DNS Info.
`echo "DNS Servers: " >> $output`

`cat /etc/resolv.conf >> $output`

#### List SUID files.
`echo -e "\nSUID Files:" >> $output`

`for suid in $suids;`

`do`
	
	echo $suid >> $output
	
`done`

#### List top 10 processes.
`echo -e "\nTop 10 Processes" >> $output`
`ps aux --sort -%mem | awk {'print $1, $2, $3, $4, $11'} | head >> $output`

#### Check the permissions on files.
`echo -e "\nThe permissions for sensitive /etc files: \n" >> $output`

`for file in ${files[@]};`

`do`
	
	ls -l $file >> $output

`done`

#### End of script


## **Bash Scripting & Programming: Script 3**

`#!/bin/bash`

#### System information.

`mkdir ~/research 2> /dev/null`

`echo "A Quick System Audit Script" > ~/research/sys_info.txt`

`date >> ~/research/sys_info.txt`

`echo "" >> ~/research/sys_info.txt`

`echo "Machine Type Info:" >> ~/research/sys_info.txt`

`echo $MACHTYPE >> ~/research/sys_info.txt`

`echo -e "Uname info: $(uname -a) \n" >> ~/research/sys_info.txt`

`echo -e "IP Info: $(ip addr | head -9 | tail -1) \n" >> ~/research/sys_info.txt`

`echo -e "Hostname: $(hostname -s) \n" >> ~/research/sys_info.txt`

`echo "DNS Servers: " >> ~/research/sys_info.txt`

`cat /etc/resolv.conf >> ~/research/sys_info.txt`

`echo -e "\nMemory Info:" >> ~/research/sys_info.txt`

`free >> ~/research/sys_info.txt`

`echo -e "\nCPU Info:" >> ~/research/sys_info.txt`

`lscpu | grep CPU >> ~/research/sys_info.txt`

`echo -e "\nDisk Usage:" >> ~/research/sys_info.txt`

`df -H | head -2 >> ~/research/sys_info.txt`

`echo -e "\nWho is logged in: \n $(who -a) \n" >> ~/research/sys_info.txt`

`echo -e "\nSUID Files:" >> ~/research/sys_info.txt`

`sudo find / -type f -perm /4000 >> ~/research/sys_info.txt`

`echo -e "\nTop 10 Processes" >> ~/research/sys_info.txt`

`ps aux --sort -%mem | awk {'print $1, $2, $3, $4, $11'} | head >> ~/research/sys_info.txt`

#### End of script
