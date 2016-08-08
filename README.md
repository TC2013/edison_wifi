 Instructions:

 Install where you want to run it from, in this example my username is edison and the location is: /home/edison/src/edison_wifi
 
 cd ~/src
 
 git clone https://github.com/TC2013/edison_wifi
 
 cd edison_wifi
 
 chmod +x wifi.sh  (or whatever
 You may need to modify line 39 of wifi.sh to your location if different than /home/edison/src/edison_wifi
 Add to crontab using sudo crontab -e
 
 Run Every 5 mins - Seems like ever min is over kill unless
 this is a very common problem.  If once a min change */5 to *
 once every 2 mins */5 to */2 ...  
 Add the following line to cron:
 
 */5 * * * * cd /home/edison/src/edison_wifi/ && ./wifi.sh  #(change directories to wherever you store wifi.sh)
 


The following line is helpful in not having to enter your password to run commands:

sudo visudo and append "edison ALL=(ALL) NOPASSWD: ALL" at the end without quotes

___________________________________________________________________________________ 
This final crontab (not sure it works very well) script is meant to automatically switch your pi/edison over to your home network (preferred network) if available. It isn't very reliable, but I'm keeping it in for now in case someone can figure out how to make it work more reliably.

You will need to run the following as sudo crontab -e
 REMOTE_HOST="google.com" # will be used to test network connectivity
 YOUR_HOME_NETWORK=ssid_name
 
 */15 * * * * ( (wpa_cli status | grep $YOUR_HOME_NETWORK > /dev/null && echo already on $YOUR_HOME_NETWORK) || (wpa_cli scan > /dev/null && wpa_cli scan_results | egrep $YOUR_HOME_NETWORK > /dev/null && wpa_cli select_network $(wpa_cli list_networks | grep $YOUR_HOME_NETWORK | cut -f 1) && echo switched to $YOUR_HOME_NETWORK && sleep 15 && (for i in $(wpa_cli list_networks | grep DISABLED | cut -f 1); do wpa_cli enable_network $i > /dev/null; done) && echo and re-enabled other networks) ) 2>&1 | logger -t wifi-select


