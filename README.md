 Instructions:

 o Install where you want to run it from like /home/edison/src/edison_wifi
 
 cd ~/src
 
 mkdir edison_wifi
 
 git clone https://github.com/TC2013/edison_wifi
 
 cd edison_wifi
 
 o chmod 0755 /home/edison/src/edison_wifi/wifi.sh
 
 o Add to crontab using crontab -e
 
 Run Every 5 mins - Seems like ever min is over kill unless
 this is a very common problem.  If once a min change */5 to *
 once every 2 mins */5 to */2 ...

 */5 * * * * ~/src/edison_wifi/wifi.sh
 
 YOUR_HOME_NETOWRK=ssid_name  #change ssid_name to your primary network you want to use.  This will switch to that network if it is available.
 
 */15 * * * * ( (wpa_cli status | grep $YOUR_HOME_NETWORK > /dev/null && echo already on $YOUR_HOME_NETWORK) || (wpa_cli scan > /dev/null && wpa_cli scan_results | egrep $YOUR_HOME_NETWORK > /dev/null && wpa_cli select_network $(wpa_cli list_networks | grep $YOUR_HOME_NETWORK | cut -f 1) && echo switched to $YOUR_HOME_NETWORK && sleep 15 && (for i in $(wpa_cli list_networks | grep DISABLED | cut -f 1); do wpa_cli enable_network $i > /dev/null; done) && echo and re-enabled other networks) ) 2>&1 | logger -t wifi-select

sudo visudo and append edison ALL=(ALL) NOPASSWD: ALL at the end
