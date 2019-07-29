This package has been successfully used with free [tac_plus](http://www.shrubbery.net/tac_plus/) TACACS+ server on variety of operating systems.

### Taccplus for RHEL 7 

```
Fixed the Makefile's to supress fail issues in compilation over RHEL 7.6 x64_64
taking off  
AM_CFLAGS = -Wall -Wextra -Werror

```
Raul Leite <rleite@redhat.com>

```
** Very important to mention I already did the RPM's build for RHEL as weel in the repo like:
- https://github.com/sp4wnr0ot/pam_tacplus/tree/master/RPMS

```

```
To install 'pam_tacplus-1.4.1-1' on RHEL  7.6 x86_64
$ wget https://github.com/sp4wnr0ot/pam_tacplus/raw/master/RPMS/pam_tacplus-1.4.1-1.el7.x86_64.rpm
$ rpm -ivh pam_tacplus-1.4.1-1.el7.x86_64.rpm
```


### Example configuration:
```
#%PAM-1.0
auth       required     /lib/security/pam_tacplus.so debug server=1.1.1.1 secret=SECRET-1
account	   required	/lib/security/pam_tacplus.so debug secret=SECRET-1 service=ppp protocol=lcp
account    sufficient	/lib/security/pam_exec.so /usr/local/bin/showenv.sh
password   required	/lib/security/pam_cracklib.so
password   required	/lib/security/pam_pwdb.so shadow use_authtok
session    required	/lib/security/pam_tacplus.so debug server=1.1.1.1 server=2.2.2.2 secret=SECRET-1 secret=SECRET-2 service=ppp protocol=lcp
```

