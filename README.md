# ssh-config
Secure your SSH with ~/.ssh/config

This guide aims to give you an understanding of your SSH-configuration. It contains useful information about finding your default configuration. As well as best practices to setup your own sane defaults. Beginning users should be able to walk through this guide. If not, please open an Issue on this Github page. Your feedback is very much appreciated and will probably help new users as well.

This guide focusses primarily on OpenSSH. Which comes bundled with BSD by default (which also means: Any Apple computer).
<br> Other users would be using: Portable OpenSSH. The VPS you rent from your provider, which has SSH installed by default; Will probably use OpenSSH. In this case, the Portable version (because Ubuntu is Linux and not BSD). Read more in [your SSH version](#your-ssh-version).

<br>
<br>

## Your SSH version

<br>

Find your SSH version by running the following command in your terminal: `ssh -V`
<br> To find the full location of the path to the ssh executable, run: `which ssh`
<br> When you have multiple installations of ssh on your system; You want to make sure you are using the expected one. You can verify this by running the command: `which ssh` and verify the output shows you the expected path.

<br>
<br>

## Default configuration

<br>

Use these as a reference. Do not change anything in the default configuration files. Unless you really know what you are doing and have a specific reason or requirement to do so. In most cases you want to keep your [custom configuration](#custom-configuration) in your home directory.

<br>

### OS X / macOS

On an Apple computer. The default configuration files are stored in: `/Volumes/Macintosh HD/private/etc/ssh/`
<br> For your convenience, there is already a symlink in place. You can easily access this directory by navigating to: `/etc/ssh`

Open this directory with Finder by running the following command in your terminal: `open /etc/ssh`
<br> The files that contain your default configuration are: `sshd_config` and `ssh_config`
<br> You can also read this files by running the following commands: `vim /etc/ssh/ssh_config` and `vim /etc/ssh/sshd_config`
<br> When using vim, the file will open in readonly mode. No accidental changes will happen. Close the file by typing: `:q!` then press enter.
<br> These files contain a lot of useful information and show you exactly which properties are set on your system. Note that these properties override the defaults that are shown in the official manual of OpenSSH. Apple adds some changes and has clearly written all information within these files. Some software installation will also append configuration to these files (e.g: XQuartz). These configuration files can only be changed when you provide your password. The default configuration files will not change unattended.

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

### Use these by default, unless you are required to do otherwise.

You want to define these properties at the top of your `~/.ssh/config` file.
<br> Then, these properties cannot be overridden in any of your defined hosts.
<br>
<br>

Hotfix [CVE-2016-0777 / CVE-2016-0778](https://www.qualys.com/2016/01/14/cve-2016-0777-cve-2016-0778/openssh-cve-2016-0777-cve-2016-0778.txt) (affects OpenSSH versions 5.4 through 7.1).
<br> More info on [OpenSSH OpenBSD Journal](http://undeadly.org/cgi?action=article&sid=20160114142733).
```bash
# Disables resuming of a suspended SSH session (CVE-2016-0777 / CVE-2016-0778).
UseRoaming no
```

<br>

Always use Public Key authentication. Never use Password authentication.
```bash
# Disables tunneled clear text passwords.
PasswordAuthentication no
ChallengeResponseAuthentication no
# Enables public key authentication, should be enabled by default.
# PubkeyAuthentication yes
```

<br>

Only use identity keys configured in ssh_config files, even if ssh-agent offers more identities.
```bash
# Disables identities offered by ssh-agent.
IdentitiesOnly yes
```

<br>

### Consider these options and adapt based on your use-case.

You should consider defining the next properties at the top of your configuration file.
Their value is a list of items. When the first option is unavailable, the next in line will be used.
Therefor, you want to define the best option that is available on your system as the first item.
Consider adding items to the list when you need to support others for legacy systems.
<br>
<br>

KexAlgorithms are used when establishing a connection (and rekeying).
<br> You probably want to use the `curve25519-sha256@libssh.org` key exchange algorithm.
<br> When unavailable; The diffie-hellman-group-exchange-sha256 is a slow but secure alternative.
<br> The [curve25519-sha256@libssh.org](https://www.libssh.org/2013/11/03/openssh-introduces-curve25519-sha256libssh-org-key-exchange/) is different from curve25519-sha256.
<br> Read more about [Diffie-Hellman weaknesses](https://cert.europa.eu/static/WhitePapers/CERT-EU-SWP_16-002_Weaknesses%20in%20Diffie-Hellman%20Key%20v1_0.pdf) (page 5). Note that the 'CERT-EU Security Whitepaper 16-002' dates from 2016. In todays standards, 2048-bit should be considered weak as well.
```bash
# Used when establishing a connection (and rekeying).
KexAlgorithms curve25519-sha256@libssh.org,diffie-hellman-group-exchange-sha256
```

<br>
<br>

## Sources and interesting reads

<br>

- https://ssh-comparison.quendi.de/impls/openssh.html


