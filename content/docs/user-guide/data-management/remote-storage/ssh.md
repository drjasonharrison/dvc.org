# SSH and SFTP

<!--
## SSH
-->

<details>

### Click to learn about SSH and SFTP.

[SSH] (Secure Shell) is a protocol that uses encryption to secure a connection
with a remote computer, which lets you safely transfer files to and from it
(like [`scp`]), among other features. Other operations can be used on top of
SSH, like FTP (simple file transfer protocol) which becomes secure or [SFTP].

[ssh]: https://www.ssh.com/academy/ssh
[sftp]: https://www.ssh.com/academy/ssh/sftp-ssh-file-transfer-protocol
[`scp`]: https://www.ssh.com/academy/ssh/scp

</details>

DVC will act as an SSH/SFTP client, which means that the remote storage should
be located in an [SSH server]. Use `dvc remote add` to define the remote by
setting a name and valid [SSH URL] (which may include some auth info. like user
name or port):

```cli
$ dvc remote add -d myremote ssh://user@example.com:2222/path
```

[ssh server]: https://www.ssh.com/academy/ssh/server
[ssh url]: https://tools.ietf.org/id/draft-salowey-secsh-uri-00.html#sshsyntax

<admon type="warn">

DVC requires both SSH and SFTP access to work with SSH remote storage. Check
that you can connect both ways with tools like [`ssh`] and `sftp` (GNU/Linux).  
Note that your server's SFTP root might differ from its physical root (`/`).

[`ssh`]: https://www.ssh.com/academy/ssh/command

</admon>

By default, authentication credentials (user name, password or private key,
etc.) not found in the URL are loaded from [SSH configuration]. You can also set
them directly with DVC.

[ssh configuration]: https://www.ssh.com/academy/ssh/config

## Custom authentication

2 parameters that are commonly included in an SSH URL are user name and
sometimes port. These can be set (or overridden) as follows:

```cli
$ dvc remote modify myremote user myuser
$ dvc remote modify myremote port 2222
```

Order in which DVC picks these values when defined in multiple places:

1. Value set in these `user`/`port` params (DVC-specific config)
2. User/port embedded in the `url`, if any (e.g. `ssh://user@example.com:2222`)
3. `User`/`Port` defined for the host in SSH config
4. Default values: Current system user; Standard SSH port 22

<admon type="warn">

The `dvc remote modify --local` flag is needed to write sensitive user info to a
Git-ignored config file (`.dvc/config.local`) so that no secrets are leaked
through Git. See
[Configuration](/doc/user-guide/project-structure/configuration#config-file-locations).

</admon>

Using a private key is usually the recommended way to authenticate an SSH
connection, and it should be saved in a key file. You can set its path as shown
below. Often these require a passphrase to use as well: You can set up DVC to
ask for it each time, or set it directly.

```cli
$ dvc remote modify --local myremote keyfile /path/to/keyfile
# and (if needed)
$ dvc remote modify myremote ask_passphrase true
# or
$ dvc remote modify --local myremote passphrase mypassphrase
```

Another popular way to authenticate an SSH connection is with a simple password.
It can be set directly or you can set up DVC to ask for it when needed:

```cli
$ dvc remote modify --local myremote password mypassword
# or
$ dvc remote modify myremote ask_password true
```

## More configuration parameters

- `url` - modify the remote location ([scroll up](#amazon-s3) for details)

- `allow_agent` - whether to use [SSH agents] (`true` by default). Setting this
  to `false` is useful when `ssh-agent` is causing problems, e.g. "No existing
  session" errors.

- `gss_auth` - use Generic Security Service auth if available on host (for
  example, [with Kerberos]). `false` by default

  <admon type="warn">

  Using GSS requires `paramiko[gssapi]`, which is only supported currently by
  the DVC pip package (installed with `pip install 'dvc[ssh_gssapi]'`).

  </admon>

- `jobs` - change the default number of processes for remote storage
  synchronization operations (see the `--jobs` option of `dvc push`, `dvc pull`,
  `dvc get`, `dvc import`, `dvc update`, `dvc add --to-remote`, `dvc gc -c`,
  etc.). The default is `4 * cpu_count()`. Setting this to a lower value may be
  useful when encountering a "Can't create any SFTP connections" error.

  <admon type="info">

  When encountering a "Can't create any SFTP connection" errors, you may need to
  specify a lower `jobs` value. Using a value of `1` will generally resolve all
  of these errors, at the cost of performance. When troubleshooting these types
  of errors, we recommend starting with a value of `1`, and then try increasing
  the value until you have determined the maximum `jobs` value which works for
  your particular client + SSH server configuration.

  </admon>

[ssh agents]: https://www.ssh.com/academy/ssh/agent
[with kerberos]:
  https://en.wikipedia.org/wiki/Generic_Security_Services_Application_Program_Interface#Relationship_to_Kerberos
