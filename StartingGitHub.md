# Getting Started with GitHub
Not everyone will need to do each step, just grab what you need.

## What is GitHub?
From the GitHub peeps:
> GitHub is a development platform inspired by the way you work. From open source to business, you can host and review code, manage projects, and build software alongside millions of other developers.

## Register for a GitHub account
Go to [GitHub Registration Page](https://github.com/join?source=header-home)

## Setting up your client machine
Like I said earlier, I use Debian Linux but I've used these commands on my work MacBook as well. I work strictly from my command line, no GUI here.

## Installing Git
For this part I will provide your starting point for Mac and Windows, you'll have to do the rest.

### Debian or Ubuntu based OS:
`sudo apt-get install git-all`

### Mac
Download the installer from [GitHub](http://mac.github.com.)
### Windows
Download the installer from [GitHub](http://windows.github.com)

## SSK Keys
Since I'm working from command-line, I will need to setup SSH Keys.

### Check if you already have an SSH Key
`ls -a ~/.ssh`

If you already have keys, you will see:

`.    ..  id_rsa  id_rsa.pub  known_hosts`

#### Decide if you want a new key for Git use or keep the current one.
I like to have things segregated so I have a specific key for Git use on each of my machines I work from. For using multiple keys, use this [Atlassian Documentation](https://confluence.atlassian.com/bitbucket/configure-multiple-ssh-identities-for-gitbash-mac-osx-linux-271943168.html) for reference.

### Creating a new SSH Key
I use ecdsa for all my keys, you can change that if you need.
* Read a bit on this from [Debian](https://lists.debian.org/debian-devel-announce/2010/09/msg00003.html) and this overview from [SSH](https://www.ssh.com/ssh/keygen/)

#### TLDR
ecdsa - a new Digital Signature Algorithm standarized by the US government, using elliptic curves. This is probably a good algorithm for current applications. Only three key sizes are supported: 256, 384, and 521 (sic!) bits. We would recommend always using it with 521 bits, since the keys are still small and probably more secure than the smaller keys (even though they should be safe as well). Most SSH clients now support this algorithm.


`ssh-keygen -t ecdsa -b 521 -C "email@example.com"`

### Adding your SSH Key to the ssh-agent
#### Start the SSH Agent in the background
`eval "$(ssh-agent -s)"`

#### Add the key
`ssh-add ~/.ssh/id_ecdsa`

#### Autoload Keys into your keychain (for Mac)
`vi ~/.ssh/config`

Paste in the following

```
Host *
 AddKeysToAgent yes
 IgnoreUnknown UseKeychain
 UseKeychain yes
 IdentityFile ~/.ssh/id_ecdsa
```

Next, run the following

`ssh-add -K ~/.ssh/id_ecdsa`


#### List all managed keys
`ssh-add -l`

### Adding your SSH Key to GitHub
#### Install xclip to make copying easier
`sudo apt update && sudo apt install xclip`

#### Copy the key to the clipboard
`xclip -sel clip < ~/.ssh/id_ecdsa.pub`

* Head over to git hub and sign in.
* In the upper right corner, click on your profile picture then click `Settings`
* From the SideBar, click `SSH and GPG keys`
* Click `New SSH key`
* For `Title` I use my computer name so I know which key this is.
* Paste in the key we copied earlier
* Click `Add SSH keys`

### Testing your SSH connection
Run the following in a Terminal

`ssh -T git@github.com`

At the prompt, type `yes` and press enter, you should get the following line:

`Hi username! You've successfully authenticated, but GitHub does not provide shell access.`

### Tell Git who is working
`git config --global user.name "User Name"`

`git config --global user.email "email@example.com"`

* Use `--global` to set the configuration for all projects. If `git config` is used without `--global` and run inside a project directory, the settings are set for the specific project.
