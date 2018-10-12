# NTP operation
## ubuntu 14.04

* service start/stop
  ```bash
  sudo service ntp stop
  sudo ntpd -gq ← here you can see it's getting sync
  sudo service ntp start
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
