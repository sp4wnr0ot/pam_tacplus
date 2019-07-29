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
$ rpm -ivh https://github.com/sp4wnr0ot/pam_tacplus/blob/master/RPMS/pam_tacplus-1.4.1-1.el7.x86_64.rpm
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

### More on server lists:

1. Having more that one TACACS+ server defined for given management group
has following effects on authentication:

 	* if the first server on the list is unreachable or failing
	  pam\_tacplus will try to authenticate the user against the other
	  servers until it succeeds

	* the `first_hit' option has been deprecated

	* when the authentication function gets a positive reply from
	  a server, it saves its address for future use by account
	  management function (see below)

2. The account management (authorization) function asks *only one*
TACACS+ server and it ignores the whole server list passed from command
line. It uses server saved by authentication function after successful
authenticating user on that server. We assume that the server is
authoriative for queries about that user.

3. The session management (accounting) functions obtain their server lists
independently from the other functions. This allows you to account user
sessions on different servers than those used for authentication and
authorization.

	* normally, without the `acct_all' modifier, the extra servers
	  on the list will be considered as backup servers, mostly like
	  in point 1. i.e. they will be used only if the first server
	  on the list will fail to accept our accounting packets.

	* with `acct_all' pam_tacplus will try to deliver the accounting
	  packets to all servers on the list; failure of one of the servers
	  will make it try another one. This is useful when your have several accounting, billing or
	  logging hosts and want to have the accounting information appear
	  on all of them at the same time.

