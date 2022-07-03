# ssh-config
Secure your SSH with ~/.ssh/config

This guide aims to give you an understanding of your SSH-configuration. It contains useful information about finding your default configuration. As well as best practices to setup your own sane defaults. Beginning users should be able to walk through this guide. If not, please open an Issue on this Github page. Your feedback is very much appreciated and will probably help new users as well.

This guide focusses primarily on OpenSSH. Which comes bundled with BSD by default (which also means: Any Apple computer).
<br> Other users would be using: Portable OpenSSH (e.g: when you rented an Ubuntu VPS and your provider has SSH pre-installed).

The minimum tested version of OpenSSH is 6.9

<br>
<br>

## Your SSH version

<br>

Find your SSH version by running the following command in your terminal: `ssh -V`
<br> When you have multiple installations of ssh on your system; You want to make sure you are using the expected one. You can verify this by running the command: `which ssh` and verify that the output shows you the expected path.

<br>
<br>

## Default configuration

<br>

### OS X / macOS

On an Apple computer. The default configuration file is stored in: `/etc/ssh/ssh_config`.
Some useful information and the default properties, written in comments, can be found there.
You probably do not want to change anything here. You want to write your own [custom configuration](#custom-configuration) in your home directory.

Open this directory with Finder by running the following command in your terminal: `open /etc/ssh`.
You can read the file by running the following command: `vim /etc/ssh/ssh_config`.
When using vim, the file will open in readonly mode. No accidental changes will happen. Close the file by typing: `:q!` then press enter.

<br>

### Ubuntu

...

<br>
<br>

## Custom configuration

<br>

...

<br>
<br>

## Configuration walkthrough

<br>

Disables resuming of a suspended SSH session (CVE-2016-0777 / CVE-2016-0778).
```bash
UseRoaming no
```

<br>

Always use Public Key authentication. Never use Password authentication.
```bash
# Disables tunneled clear text passwords.
PasswordAuthentication no
ChallengeResponseAuthentication no

# Enables public key authentication.
PubkeyAuthentication yes
```

<br>

Disables identities offered by ssh-agent.
```bash
IdentitiesOnly yes
```

<br>

The cipher is used to encrypt the data transfer.
```bash
Ciphers chacha20-poly1305@openssh.com,aes256-gcm@openssh.com,aes256-ctr
```

<br>

A MAC is used for non-AEAD encryption, cfr: aes256-ctr.
```bash
MACs hmac-sha2-512-etm@openssh.com,hmac-sha2-512
```

<br>

The type of key, offered by the remote location. Stored in your known_hosts file.
```bash
# OpenSSH >= 7.2
HostKeyAlgorithms ssh-ed25519,rsa-sha2-512,rsa-sha2-256

# OpenSSH < 7.2
HostKeyAlgorithms ssh-ed25519,ssh-rsa
```

<br>

KexAlgorithms are used when establishing a connection (and rekeying).
```bash
# OpenSSH >= 8.5
KexAlgorithms sntrup761x25519-sha512@openssh.com,curve25519-sha256,diffie-hellman-group-exchange-sha256

# OpenSSH < 8.5
KexAlgorithms curve25519-sha256@libssh.org,diffie-hellman-group-exchange-sha256
```
When you use the older `diffie-hellman-group-exchange-sha256`; You want to (re)generate the moduli. More info can be found at the bottom of the section [key exchange in this document](https://stribika.github.io/2015/01/04/secure-secure-shell.html#key-exchange).

<br>
<br>

## References

<br>

- https://www.openssh.com/releasenotes.html
- https://anongit.mindrot.org/
- https://ssh-comparison.quendi.de/impls/openssh.html
- https://stribika.github.io/2015/01/04/secure-secure-shell.html
- https://cert.europa.eu/static/WhitePapers/CERT-EU-SWP_16-002_Weaknesses%20in%20Diffie-Hellman%20Key%20v1_0.pdf
- https://www.qualys.com/2016/01/14/cve-2016-0777-cve-2016-0778/openssh-cve-2016-0777-cve-2016-0778.txt
- http://undeadly.org/cgi?action=article&sid=20160114142733
- https://superuser.com/a/1225476
- https://blog.twogate.com/entry/2020/07/30/benchmarking-ssh-connection-what-is-the-fastest-cipher
- https://soatok.blog/2020/07/12/comparison-of-symmetric-encryption-methods/
