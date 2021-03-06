# NTP operation
## ubuntu 14.04 [refer](https://askubuntu.com/questions/929805/timedatectl-ntp-sync-cannot-set-to-yes)

* query timedate status
  ```bash
  $ timedatectl
         Local time: Fri 2018-10-12 01:26:37 PDT
    Universal time: Fri 2018-10-12 08:26:37 UTC
          Timezone: Asia/Hong_Kong (PDT, -0700)
       NTP enabled: yes
   NTP synchronized: yes
   RTC in local TZ: no
        DST active: yes
   Last DST change: DST began at
                    Sun 2018-03-11 01:59:59 PST
                    Sun 2018-03-11 03:00:00 PDT
   Next DST change: DST ends (the clock jumps one hour backwards) at
                    Sun 2018-11-04 01:59:59 PDT
                    Sun 2018-11-04 01:00:00 PST
  ```
* query directly by ntpdate

  ```bash
  $ sudo service ntp stop
  $ sudo ntpdate -v ntp-server-address
  12 Oct 01:29:56 ntpdate[31083]: ntpdate 4.2.6p5@1.2349-o Wed Oct  5 12:35:26 UTC 2016 (1)
  12 Oct 01:30:03 ntpdate[31083]: adjust time server 171.68.38.65 offset -0.043101 sec
  $sudo service ntp start
  ```
  
* add ntp server

  * disabled unused ntp server
  
    ```
    #server 0.ubuntu.pool.ntp.org
    #server 1.ubuntu.pool.ntp.org
    #server 2.ubuntu.pool.ntp.org
    #server 3.ubuntu.pool.ntp.org
    ```

    ```bash
    $ sudo service stop ntp
    $ sudo vi /etc/ntp.conf
    # add like "server my-ntp-server"
    $ sudo service start ntp
    # query current server used
    $ ntpq -pn
    $ ping my-ntp-server

    $ grep ntpd /var/log/syslog 
    ```
  
  * query from ntpd status
  
    ```
    $ ntpdc -c sysinfo
    system peer:          xxxx
    system peer mode:     client
    leap indicator:       00
    stratum:              2
    precision:            -22
    root distance:        0.15753 s
    root dispersion:      1.02357 s
    reference ID:         [xxx.xxx.xx.xx]
    reference time:       df6add60.88f88ed1  Fri, Oct 12 2018  1:44:16.535
    system flags:         auth monitor ntp kernel stats
    jitter:               0.000000 s
    stability:            0.000 ppm
    broadcastdelay:       0.000000 s
    authdelay:            0.000000 s
    ```
  
* fix apt install issue
  
  * issues
    * The following signatures were invalid: BADSIG
    * libopts25 ntp The following packages cannot be authenticated!
    
  * solution
    ```bash
    $ sudo apt-get clean 
    $ cd /var/lib/apt 
    $ sudo mv lists lists.old 
    $ sudo mkdir -p lists/partial 
    $ sudo apt-get clean 
    $ sudo apt-get update
    ```
 * fix ipv6 issue
  * disable ipv6 , [refer](https://askubuntu.com/questions/440649/how-to-disable-ipv6-in-ubuntu-14-04)
  
    To disable ipv6, you have to open /etc/sysctl.conf using any text editor and insert the following lines at the end:

      ```
      net.ipv6.conf.all.disable_ipv6 = 1
      net.ipv6.conf.default.disable_ipv6 = 1
      net.ipv6.conf.lo.disable_ipv6 = 1

      ```
    If ipv6 is still not disabled, then the problem is that sysctl.conf is still not activated.
    so need

      ```
      $ sudo sysctl -p
      ```

     You will see this in the terminal:

      ```
      net.ipv6.conf.all.disable_ipv6 = 1
      net.ipv6.conf.default.disable_ipv6 = 1
      net.ipv6.conf.lo.disable_ipv6 = 1
      ```
      
      After that, if you run:

      ```bash
      $ cat /proc/sys/net/ipv6/conf/all/disable_ipv6
      ```

      It will report:

      ```
      1
      ```
      If you see 1, ipv6 has been successfully disabled.
