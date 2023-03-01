# The recommended SSH config file
We recommend that in order to use ssh to connect to gadi and/or accessdev, you should use this file as the basis for your ssh configuration file, located in `.ssh/config` on your Desktop/Laptop:

Note that the `UseKeychain` keyword only works on MacOS and must be omitted on other operating systems.

```
Host *
    AddKeysToAgent yes
    UseKeychain yes

Host gadi accessdev
    HostName %h.nci.org.au
    User YOUR NCI USERNAME
    ForwardX11 yes
    ForwardX11Trusted yes

Host gadi
    ForwardAgent yes

Host accessdev
    PubkeyAcceptedAlgorithms +ssh-rsa
    HostkeyAlgorithms +ssh-rsa
```

The explanation of the keywords:
- `Host` is a section header declaring for which connections which settings should be used. `Host *` means that these options should be used for all ssh connections. Then we have 3 specific sections, one for both **gadi** and **accessdev** and then one each for them individually.
- `AddKeysToAgent` means that decrypted ssh keys should automatically be added to the running agent. That way you are no longer asked for the decryption key while the agent is running (usually until reboot).
- `UseKeychain` is only available on Macbooks. Using this option stores the unencrypted key in the MacOS key chain which is automatically unlocked when you log into your computer. That way you never have to enter the passphrase again.
- `HostName` the full name of the host. `%h` in this case gets replaced with the short form, for example when running `ssh gadi`, this will equate to `HostName gadi.nci.org.au`
- `User` this is the only setting you need to change. If your NCI username is `aaa123`, then this line should read `User aaa123`.
- `ForwardX11` and `ForwardX11Trusted` are keywords that allow for programs with graphical user interfaces to be launched on the remote machine, and their interface being displayed on your computer. If you are not using a Linux/Unix system, you will have to use a local X11 program like [XQuartz](https://xquartz.org) for Mac or [Xming](http://sourceforge.net/projects/xming/) for Windows.
- `ForwardAgent` allows your local agent to supply the key for ssh connections going out from the remote computer. E.g. when you run `ssh gadi` and then on gadi run `ssh accessdev`, the agent on your desktop/laptop will answer the challenge and allow you to log in without having to enter a passphrase.
- `PubkeyAcceptedAlgorithms` and `HostKeyAlgorithms` are two settings that become unfortunately relevant as the cryptographic capabilities of accessdev can't keep up with modern environment. These algorithms are too old and vulnerable, and are no longer supported on modern computers by default. These options enable them again.

## SSH Key Types
If you create ssh keys, the default type is **RSA**, and accessdev understands that. If you use the more modern **ED25519** key type, however, you will not be able to use it with accessdev. You can create a different key pair specifically for accessdev, in which case I would add these lines:

```
Host *
    ...
    IdentityFile ~/.ssh/id_ed25519

...
Host accessdev
    ...
    IdentityFile ~/.ssh/id_rsa
```
so that by default you're using your better ed25519 key, but for accessdev you're using the older rsa key instead.
