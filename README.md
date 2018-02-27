![throwcatch](https://raw.githubusercontent.com/superDuperCyberTechno/rockatansky/master/header.pn://raw.githubusercontent.com/superDuperCyberTechno/throwcatch/master/header.png)

## throwcatch is a super simple tool made for backing up files to a server.

The software consists of 2 files:

### throw
`throw` is located on the machine that wants to do the backing up. In order for it to function properly, it needs to know 2 things.

* `thrower` is the user that was created with `catch` on the backup server.
* `catcher` is the IP or domain name of the backup server (where `catch` is located).

To set it up, simply run the file 

```
./throw

```

... and you will be prompted for the name of the thrower and the URL/Ip of the catcher.

Assuming these 2 variables are valid, throw can do its magic.

In order to back up files you simply supply them as arguments to `throw`:

`./throw stone.txt stone2.db stone3.conf rock.sql`

Smack that enter key and these 4 files will be thrown at you catcher, who will take care of your files.

`throw` even supports wildcards, so the above example could be simplified to this:

`./throw stone* rock.sql`

### catch
`catch` is installed on the server you want to keep the backups. but you need to run it in order for the server to be able to catch stuff:

```
./catch
```

Now, you will be prompted to give `catch` 2 variables: 

* `thrower` is the user that was created with `catch` on the backup server.
* `catcher` is the IP or domain name of the backup server (where `catch` is located).

A name of a thrower and the IP address of said thrower. Throwers are authorized solely based on their username and their IP's.

When these have been supplied, the server will be ready to catch!
