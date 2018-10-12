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
