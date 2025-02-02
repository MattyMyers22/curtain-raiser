# SSH-KEYGEN

> Generate keys for secure shell access to a server,
> a discussion specifically for cs-south-sound projects.
## Help

The `man` command will provide some hints,
```bash
man ssh-keygen
```
See the reference section below for online man pages

> Note there is no command line help argument like -h that returns hints

## Create private and public keys on the client

```bash
ssh-keygen -f ~/.ssh/id_mykeyname-rsa -t rsa -b 4096 -C "myusername"
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
```

In the directory `~/.ssh/` expect two (or more) files named:
```bash
id_mykeyname-rsa      (secret - do not share it)
id_mykeyname-rsa.pub  (public and not secret)
```

Allow user entrance to the directory with the `cd` command for example.
```bash
chmod 700 $HOME/.ssh
```

Minimize permissions on the keys to read/write for the user only.
( Avoids the error message: WARNING: UNPROTECTED PRIVATE KEY FILE! )
```bash
chmod 600 id_mykeyname-rsa
chmod 600 id_mykeyname-rsa.pub
```

Confirm the changes.
```bash
ls -al ~/.ssh
```

The importance of defining a key name is the expectation to be working
on more than one server which will require separate unique keys.
Name your key something that reminds you of your project or the server.

The algorithm chosen is [rsa](https://en.wikipedia.org/wiki/RSA_(cryptosystem)) with an encryption number of bits 4096.
Examine the generated keys as follows:

```bash
cat ~/.ssh/id_mykeyname-rsa
cat ~/.ssh/id_mykeyname-rsa.pub
```

Finally, note that the comment `myusername` is added to the end of the key.
It may not be necessary, but some applications may expect the username.
Additionally it may be helpful for the server administrator to know to
whom the key belongs.

## Share the public key

> Only the public key, never the private

* With care, copy and paste is possible.  Make the screen font small enough to copy all the symbols including the start and end notes. Paste where requested.
* Submit directly to a server where you already have a password with:

  ```bash
  ssh-copy-id -i ~/.ssh/id_mykeyname-rsa.pub username@url
  #OR
  ssh-copy-id -i ~/.ssh/id_mykeyname-rsa.pub username@ipaddress
  ```

## Client configuration

> Since we are expecting to employ multiple keys for several projects a configuration file will be needed.

On the client computer write the config file by hand at the command prompt

`vim ~/.ssh/config`

For example here is a template:

```bash
Host uniqueServerName
    HostName topLevelDomain.com
    User username
    Port 22
    Identityfile ~/.ssh/id_mykeyname-rsa.pub
    HostKeyAlgorithms ssh-rsa
```
Replace the contents of each variable with names specific to your
server.  Also note that the port may be customized by your
administrator to something other than 22.  Check with your
administrator or the server specifications. If you are the admin;
good luck!


## Connecting to the server

Since the server connecting information has been typed into the ssh
configuration file, only the Host alias is needed.  For example:

```bash
ssh uniqueServerName
#OR
sftp uniqueServerName
```
If a passphrase was entered during key creation, that phrase will be
requested in response to the ssh request.

## Reference

1. ssh.com [academy](https://www.ssh.com/academy/ssh/keygen)
2. [man7](https://www.man7.org/linux/man-pages/man1/ssh-keygen.1.html)
3. [linux.die.net](https://linux.die.net/man/1/ssh-keygen)
4. [wikipedia](https://en.wikipedia.org/wiki/Ssh-keygen)
5. example [rsa-algorithm](https://justcryptography.com/rsa-algorithm/)
6. git-to-use-ssh-key [so](https://stackoverflow.com/questions/23546865/how-to-configure-command-line-git-to-use-ssh-key)
7. on MS Windows try [git bash](https://gitforwindows.org/) or [wsl](https://learn.microsoft.com/en-us/windows/wsl/)
