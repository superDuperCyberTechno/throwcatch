![throwcatch](https://raw.githubusercontent.com/superDuperCyberTechno/rockatansky/master/header.pn://raw.githubusercontent.com/superDuperCyberTechno/throwcatch/master/header.png)

# throwcatch is a super simple tool made for backing up files to a server.

## What the hell is this?
*throwcatch* is an experimental solution to something that shouldn't be as cumbersome as it is; **backups**. In other words: *throwcatch* is a super simple tool made for backing up files to a server.

### Technicalities
*throwcatch* (more precisely `catch`) manipulates the `/etc/ssh/sshd_config` file as well as sets up a closed home folder where the *throwers* are locked into. All backed up files (called *catches* here on out) are stored in the `/home/[catcher name]/catches` folders.

*throwers* are given restricted access to a sftp process that has been locked down to a minimum of privileges; they can basically only upload files.

The real magic and potentially the biggest security flaw is the way the authentication has been set up; `catchers` are **only** authenticated based on username and IP.

## The software

The software consists of 2 files that need to be run in order for throwcatch to function:

### throw
`throw` is located on the machine that wants to do the backing up. In order for it to function properly, it needs to know 2 things.

* Its own username that was created with `catch` on the backup server.
* The IP or domain name of the backup server (where `catch` is located).

To set it up, simply run the file:

```
./throw
```

Assuming these 2 variables are valid, throw can do its magic.

In order to back up files you simply supply them as arguments to `throw`:

`./throw stone.txt stone2.db stone3.conf rock.sql`

Smack that enter key and these 4 files will be thrown at you catcher, who will take care of your files.

`throw` even supports wildcards, so the above example could be simplified to this:

`./throw stone* rock.sql`

If you need to change the credentials, reset the file like this:

```
./throw --reset
```

That will delete the current credentials, allowing you to set it up once again.

### catch
`catch` is located on the server you want to keep the backups. In order for the server to function properly, it needs to know 2 things:

* The username for the new catcher.
* The new catcher's IP address.

To set it up, simply run the file:

```
./catch
```

When these have been supplied, the server will be ready to catch! You can run `catch` again to add more throwers.

If you need to remove a thrower, run the following command:

```
./catch --remove
```

You will then be guided through the process to remove throwers.
