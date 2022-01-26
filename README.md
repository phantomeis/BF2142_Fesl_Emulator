#Battlefield 2142 Statistics Emulator Server


before you read on, read my guide for Windows OSwith pictures, ready-packed zip download and tutorials on the databases here:
https://prmp.boards.net/thread/10/setup-ranked-lan-server

######
fesl_login_server.exe
######
```
FESL/GS login server emulator v.1.0 (BETA 06)
by Andrew Vasiliev
e-mail: vasiliev1979@yandex.ru
Based on "Simple TCP proxy/datapipe" and "GS login server emulator" sources
by Luigi Auriemma
e-mail: aluigi@autistici.org
web:    aluigi.org
Usage: %s [options]
..Options:.
-x port   port to listen connections (18300 default)
-v        verbose output
-l        1 - logfile on, 0 - logfile off (default). FOR DEBUG
-restore_srv  enable restore account and password to email
-dbhost   host MySQL DB
-dbname   DB name
-dbuser   DB user
-dbpass   DB password
-p        increase process priority
-d DIR    dump the content of the connections in various tcpdump-like cap files
```
######



## Client
1. Install Battlefield 2142
2. Patch to version 1.50 and then 1.51
3. Replace original bf2142.exe with bf2142.exe from [cracked_exe] folder (for your version, i recommend 1.51) 
(This is not cracked, but IP addresses inside the exe are chnaged for easier edits later on!)
4. Depending in your Battlefield 2142patch version you either do:

Patch version 1.25
You need to edit hosts.ics ("С:\Windows\System32\drivers\etc\hosts.ics") and add next lines (сhange your.external.ip to your server's external ip):

```
your.external.ip bf2142-pc.fesl.ea.com
your.external.ip gpcm.gamespy.com
your.external.ip stella.available.gamespy.com
your.external.ip eapusher.dise.se
127.0.0.1 stella.prod.gamespy.com #do not change
your.external.ip stella.ms5.gamespy.com
your.external.ip ea.com
your.external.ip gamespy.com
your.external.ip messaging.ea.com
your.external.ip fesl.ea.com
your.external.ip gpsp.gamespy.com
your.external.ip gamestats.gamespy.com
your.external.ip stella.ms5.gamespy.com
your.external.ip eapusher.dice.se
```

Patch version 1.51
You need to edit bf2142.exe with hex-editor, search and change (by hand, not replace) the IP address 192.168.1.3 to your server's IP.
**Mind the / (slash) that appears sometimes! Do not forget to end the IP with that slash!**

(**NOTE**: If you are left with the unallocated space after correcting IP, set dots and change the bit-values
of the dots to 00!)


Now we change the hidden IP:
First, reverse your IP-Addresses numbers, convert them to HEX and paste them at hex-address 0301a8c0

For example my IP 192.168.178.20 reversed becomes 20.178.168.192

Now we need to convert every number divided by the dots into HEX-code.
Use www.browserling.com/tools/ip-to-hex for hexcode conversion or the windows calculator.

Using calculator:
Start your calculator in programer mode by pressing ALT+3, select „DEC“ on the left panel.
Type the first numbers of your IP (for example 192) and then change the calculator mode to „HEX“ on the left side. You get „C0“.
Do this for each number.

Example:
My IP 192.168.172.20 reversed becomes 20.172.168.192 , its numbers are   20     172     168     192   which each get converted, the result being 14   ac   a8   c0
Write the HEX-code like this: 14aca8c0
In your HEX-Editor open a "find & replace"-window and search for  0301a8c0   and replace with your HEX-code (14aca8c0 in my case).


=========

## Server

### Fesl Login Server (GameSpy Emulator)

**NOTE**: To simplify the work, use the AMP package, for example XAMPP, skip step 2. then.

1. Install OpenSSL (>= 1.0.0). Latest version you can download [here](https://www.openssl.org/source/) .
2. Install MySQL server (latest version [here](http://dev.mysql.com/downloads/mysql/)) .
3. Import database bf2142.sql to Mysql server.
4. Edit _launch.bat. Change dbuser, dbpass, dbname.

**NOTE**: MySQL should work only on localhost! Don't change dbhost from 127.0.0.1!

6.Edit hosts.ics ("С:\Windows\System32\drivers\etc\hosts.ics") and add next line (сhange your.external.ip to your server's ip):

```
your.ip bf2142-pc.fesl.ea.com
your.ip gpcm.gamespy.com
your.ip stella.available.gamespy.com
your.ip eapusher.dise.se
127.0.0.1 stella.prod.gamespy.com #dont change this
your.ip stella.ms5.gamespy.com
your.ip ea.com
your.ip gamespy.com
your.ip messaging.ea.com
your.ip fesl.ea.com
your.ip gpsp.gamespy.com
your.ip gamestats.gamespy.com
your.ip stella.ms5.gamespy.com
your.ip eapusher.dice.se
your.ip stella.master.gamespy.com
```

7.Start _launch.bat

**NOTE**: You can change License Agreement in license.txt, but however, due to the fact that BF2142 does not know a line break, the text will be like a one-liner.

### fesl_login_server.exe COmmandline arguments

FESL/GS login server emulator v.1.0 (BETA 06)
by Andrew Vasiliev
e-mail: vasiliev1979@yandex.ru

Based on "Simple TCP proxy/datapipe" and "GS login server emulator" sources
by Luigi Auriemma
e-mail: aluigi@autistici.org
web:    aluigi.org

Usage: %s [options]
..Options:.
-x port   port to listen connections (18300 default).
-v        verbose output.
-l        1 - logfile on, 0 - logfile off (default). FOR DEBUG.
-restore_srv  enable restore account and password to email.
-dbhost   host MySQL DB.
-dbname   DB name.
-dbuser   DB user.
-dbpass   DB password.
-p        increase process priority.
-d DIR    dump the content of the connections in various tcpdump-like cap files.



### WebServer

**NOTE**: Stats requires PHP >= 5.3.8

1. Unzip folder "web" to your localhost folder (**WARNING**: Stats system won't work at another location!)
2. Open ./include/_ccconfig.php and change $db_host, $db_name, $db_user, $db_pass to yours, which you installed in Fesl Login (step 4)
3. Open your database in MySQL (phpmyadmin/ or another utility), open table "servers" and add two new entries (local and external IP) and key "authorised" 1.
4. Open your PHP config file (php.ini) and change 

```
error_reporting = E_NONE
display_errors = Off
```


### GameServer

1. Download BF2142 serverfiles
2. Extract "python" folder to the main folder with server. Agree with overwriting.
3. Patch BF2142_w32ded.exe with lpatch.exe ("exe_patch" folder). **NOTE**: 1.25 patch with fesl_1.25.lpatch, 1.51 - fesl.lpatch
4. Configure your server gameplay settings and start BF2142_w32ded.exe.
