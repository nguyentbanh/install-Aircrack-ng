# Install Aircrack-ng, Sniffing by Airport, and using Aircracking-ng on Mac

<h2>1. Install HomeBrew</h2> 
<p>at https://brew.sh by terminal. Paste that at a Terminal prompt.</p>

<blockquote><p>/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"</p></blockquote>

To uninstall Homebrew use this:
<blockquote><p>ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/uninstall)"
</p></blockquote>

<h2>2. Install the aircrack-ng and create necessary links:</h2>

  With the homebrew installed, use this command to install aircrack-ng:
  
<blockquote><p>brew install aircrack-ng</p></blockquote>

  Link the folder from Homebrew "/usr/local/Cellar/" to the "/usr/local/bin/" in order to use the program directly on terminal. The number 1.3 can be replaced to the version you install. 
  
<blockquote><p>sudo ln -s /usr/local/Cellar/aircrack-ng/1.3/bin/aircrack-ng /usr/local/bin/aircrack-ng</p></blockquote>

  Other tool that will be necessary is the airport. So we will create a other link to use directly on terminal. Airport is already inside Mac OS. We use Airport to scan the wifis, disconnect to wifis, turn on mornitor mode and sniffing.

<blockquote><p>sudo ln -s /System/Library/PrivateFrameworks/Apple80211.framework/Versions/Current/Resources/airport /usr/local/bin/airport</p></blockquote>

<h2>3. Scanning wifis and disconnect to network:</h2>

  While we turn the wifi card to the monitor mode, we can not use the scanning function normally. The purpose of airport is scanning the wifis nearby and show information about that network, such as: SSID, BSSID, channel, RSSI...
Use the following command to check:

<blockquote><p>airport -s</p></blockquote>

  Now in order to turn wifi card to mornitor mode, we have to disconnect with any network is connecting. You can do it by manual go to Network Preferences then unselect on Automatically join this network. Do this until it does not connect to any network. Or you just type the following command:

<blockquote><p>airport -z</p></blockquote>

  To check it working or not, by simply see the wifi simple on the menu bar on the top. Or type:

<blockquote><p>ifconfig</p></blockquote>

  and check the line status: inactive

<h2>4. Sniff the traffic on channel</h2>

  Sniff the channel selected.

<blockquote><p> airport "interface" sniff "channel" </p></blockquote>

  <interface> is the name of the wifi card. Mine is "en0". Get this from 
ifconfig
  <channel> is the channel of the wifi network target. Get this from
airport -s
    
  The sniffing action create a file airportSniff*.cap. You can see it at:

<blockquote><p>ls /tmp/airportSniff*.cap</p></blockquote>

  The longer the sniffing the better data we have.

Then we will pass this file to aircrack-ng

<h2>5. Using Aircrack-ng</h2>

<blockquote><p>aircrack-ng -1 -a 1 -b "BSSID" "cap_file" -w "wordlist"</p></blockquote>

      -1            : run only 1 try to crack key with PTW  
      -a <amode>    : force attack mode (1/WEP, 2/WPA-PSK)
      -b <bssid>    : target selection: access point's MAC
      <cap_file>    : the sniffing file we have from part 4 airportSniff*.cap
      -w <wordlist> : path to wordlist(s) filename(s). This optional but a good wordlist have greater chance to succeed 

<h2>6. Turn off mornitor mode</h2>

  Using the ps program displays the currently-running processes

<blockquote><p>ps -ax | grep -a airport.*sniff</p></blockquote>

find the airport <interface> sniff <channel> line and get the four number on the left
 
<blockquote><p>sudo kill -9 "the-four-number"</p></blockquote> 

<h3>Reference</h3>
<p>https://www.aircrack-ng.org/doku.php?id=newbie_guide</p>
<p>https://www.aircrack-ng.org/doku.php?id=install_aircrack#installing_on_mac_osx</p>
<p>https://maymay.net/blog/2010/12/05/one-minute-mac-tip-sniffing-wi-fi-traffic-and-capturing-packets-with-the-built-in-airport-utility/</p>






