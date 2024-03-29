![throwcatch](https://raw.githubusercontent.com/superDuperCyberTechno/throwcatch/master/header.png)

# What the hell is this?
*throwcatch* is an experimental solution to something that shouldn't be as cumbersome as it is; **backups**. In other words: *throwcatch* is a super simple tool made for backing up files to a server.

The mnemonic is simple: *throw* files, the server will *catch* them.

It is written in bash.

Feel free to contribute.

### Technicalities
*throwcatch* (more precisely `catch`) manipulates the `/etc/ssh/sshd_config` file as well as sets up a closed home folder where the *throwers* are locked into. All backed up files (called *catches* here on out) are stored in the `/home/[catcher name]/catches` folders.

*throwers* are given restricted access to a `sftp` process that has been locked down to a minimum of privileges; they can basically only upload files. All files that have been *thrown* to the *catcher* will have a UUID appended to the file name to avoid collisions. That also means that identical files can co-exist.

**A word of warning**: As stated before, this is experimental and made mostly for fun. - The real magic and **potentially the biggest security flaw** is the way the authentication has been set up; *throwers* are **only** authenticated based on username and IP. I would love some feedback on the risks here since I am not a security expert. Use this at your own risk.

## The software

The software consists of 2 files that need to be run in order for throwcatch to function:

### `throw`
`throw` is located on the machine that wants to do backups. In order for it to function properly, it needs to know 2 things.

* Its own username that was created with `catch` on the backup server.
* The IP or domain name of the catch/backup server (where `catch` is located).

Assuming these 2 variables are valid, throw can do its magic.

To set it up, simply run the file:

```
./throw
```

In order to back up files you simply supply them as arguments to `throw`:

`./throw stone.txt stone2.db stone3.conf rock.sql`

Smack that enter key and these 4 files will be thrown at your catcher, who will then take care of your files.

`throw` even supports wildcards, so the above example could be simplified to this:

`./throw stone* rock.sql`

If you need to change the credentials, reset the file like this:

```
./throw -r
```

That will delete the current credentials, allowing you to set it up once again.

### `catch`
`catch` is located on the server where you want to keep the backups. In order for the server to catch/receive files properly, we need to add a *thrower* by supplying 2 variables:

* The username for the new *thrower*.
* The new *thrower*'s IP address.

To set it up, simply run the file (needs to be done with root privileges):

```
./catch
```

When these have been supplied, the server will be ready to catch files! You can run `catch` again to add more *thrower*s.

If you need to remove a *thrower*, run the following command:

```
./catch -r
```

You will then be guided through the process to remove *thrower*s.



## Installation
To install `throw` simply run the following command:

```
wget https://raw.githubusercontent.com/superDuperCyberTechno/throwcatch/master/throw && chmod +x throw
```

To install `catch` simply run the this command:

```
wget https://raw.githubusercontent.com/superDuperCyberTechno/throwcatch/master/catch && chmod +x catch
```

*throwcatch* has only been tested to work on Ubuntu but since it's written in Bash, it should be pretty distro agnostic.

## Other stuff
### Backup expiry
If you want to have backups expire you can add the following cron job on your backup server:

```
0 0 * * * find /home/*/catches -mtime +10 -type f -delete
```

Every 24 hours (at midnight), this will delete all files caught by the server if they are 10 (or more) days old. You can edit `10` to anything you find acceptable. __Beware: _Any_ folder in your users' home directory called `catches` will be purged as well when the above cron job runs.__

### Firewall
As SFTP piggybacks on SSH, you should only allow SSH connections on your backup server (assuming that's all you need). - This will also conveniently allow you to log into the server using SSH. This can be achieved using `ufw`:

```
ufw allow ssh && ufw enable
```

Your backup server will now deny all incoming connections except SSH (which includes SFTP).

You could (and probably should) lock it down to you own IP address, __but make sure that you have a static IP address. - If you don't, you could lock yourself out from the server and lose all backups:__

```
ufw allow from [ip address] to any port 22
```

Replace `[ip address]` with your own IP address.
__This needs to be done with all your _thrower_ IP addresses as well, otherwise they will not be able to throw files to the server.__
