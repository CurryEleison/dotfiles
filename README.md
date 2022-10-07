# dotfiles
Notes and Scripts I use


## Installations

On Debian some prereqs to make anything work:

- wget curl git gcc python3 python3-pip python3-venv
- make build-essential libssl-dev zlib1g-dev llvm libbz2-dev libreadline-dev libncursesw5-dev xz-utils libxml2-dev libxmlsec1-dev libffi-dev liblzma-dev

Additionally:

- vim (or nvim)
- micro
- git
- zsh
- oh-my-zsh (https://ohmyz.sh/#install)
- jq
- zip unzip
- bottom (top alternative, https://github.com/ClementTsang/bottom#debianubuntu )

On standalones:

- openvpn
- ssh server
- autosuspend (maybe)

On mac

- xcode command line applications


## Zsh

- Change to zsh with `chsh`
- Install oh-my-zsh with  
`sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"`
- Ensure `LC_CTYPE` (or `LANG`) is set to `en_US.UTF-8`
- Install zsh-autosuggestions:  
`git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions`
- Install zsh-syntax-highlighting  
`git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting`

### .zshrc (and oh-my-zsh configuration)

```sh
# Current favorite theme
ZSH_THEME="fishy-asdf"

# Update oh-my-zsh when it's time
zstyle ':omz:update' mode reminder

# YMD format in history command
HIST_STAMPS="yyyy-mm-dd"

# Plugins
plugins=(
        git
        asdf
        awsume
        zsh-asdf-prompt
        zsh-autosuggestions
        zsh-syntax-highlighting
        )

# Alias for python if Debian doesn't set it up
alias python=python3
```

Other interesting plugins: python, sdk, sbt, node, ssh-agent

### Environment

Can go in `~/.zshenv` or `~/.zshrc`

```sh
# Editor (vi when in VS Code terminal, but micro otherwise)
if [[ $TERM_PROGRAM ]]; then
        export VISUAL="vim"
else
        export VISUAL="micro"
fi
export EDITOR="$VISUAL"

# set PATH so it includes user's private bin if it exists
if [ -d "$HOME/bin" ] ; then
    PATH="$HOME/bin:$PATH"
fi

# set PATH so it includes user's private bin if it exists
if [ -d "$HOME/.local/bin" ] ; then
    PATH="$HOME/.local/bin:$PATH"
fi
```

### oh-my-zsh

## vim

Enhanced defaults at https://github.com/tpope/vim-sensible

## micro editor

In `~/.config/micro/settings.json`:
```json
{
    "clipboard": "terminal"
}
```


## Git

In `~/.gitconfig`:
```ini
[user]
        name = Thomas Petersen
        email = petersen@temp.dk
[pull]
        ff = only
```

or alternately `rebase = false` under pull


## Python

Install pipx into `~/.local/` with  
```
python3 -m pip install --user pipx
python3 -m pipx ensurepath
```
Now install pipenv with  
`pipx install pipenv`

The oh-my-zsh plugin for python looks decent.

### asdf

- Download from git: https://asdf-vm.com/guide/getting-started.html#_2-download-asdf
- Enabling oh-my-zsh plugin will ensure paths etc are set up in zsh. Use 
    https://asdf-vm.com/guide/getting-started.html#_3-install-asdf to cover more shells
- Install python plugin with  
`asdf plugin add python`
- Install e.g. v3.8.12 with  
`asdf install python 3.8.12`
- In `~/.tool-versions` add 
```
python system
nodejs system
```
- Add .tool-versions file to development projects as needed

#### asdf-prompt

Install asdf-prompt plugin for oh-my-zsh from https://github.com/CurryEleison/zsh-asdf-prompt


## AWS

AWS cli v2 install from https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html . Do _not_ use the pip package. To install as a userland application download, unpack and then install with:  
`./aws/install --install-dir $HOME/.local/aws --bin-dir $HOME/.local/bin`

Optionally set default region in `~/.zshenv` with  
`export AWS_DEFAULT_REGION=eu-west-1`

### awsume

Install with  
`pipx install awsume`

oh-my-zsh plugin at https://github.com/Sordie/AWSume

## sudo

Can specify visudo editor with  
`Defaults        editor=/usr/bin/vim`

Specify a group sudo with  
`%sudo   ALL=(ALL:ALL) ALL`

Place overrides at _end_ of sudoers file:  
```config
# no password to suspend machine
tpetersen       ALL=(ALL) NOPASSWD: /usr/bin/systemctl suspend
# No password to start and stop VPN
tpetersen       ALL=(ALL) NOPASSWD: /usr/bin/systemctl start openvpn-client@<VPNCONFIG>
tpetersen       ALL=(ALL) NOPASSWD: /usr/bin/systemctl stop openvpn-client@<VPNCONFIG>
```


## docker

Install programs at https://docs.docker.com/engine/install/debian/ 

Configure with https://docs.docker.com/engine/install/linux-postinstall/


## jvm

Have used SDKMAN from https://sdkman.io/ with good results. asdf does have plugins, 
but I haven't been experimenting much. 

There is an asdf plugin at
https://github.com/halcyon/asdf-java which seems maintained as well as a scala plugin.


## node

node is often best managed from asdf

Some nice environment variables to set up for node:

```sh
NPM_PACKAGES="${HOME}/.npm-packages"

export PATH="$PATH:$NPM_PACKAGES/bin"

# Preserve MANPATH if you already defined it somewhere in your config.
# Otherwise, fall back to `manpath` so we can inherit from `/etc/manpath`.
export MANPATH="${MANPATH-$(manpath)}:$NPM_PACKAGES/share/man"
```


## R

Use asdf or https://cran.r-project.org/bin/linux/debian/ to install base.

To stop nagging about saving workspace:  
`alias R='R --no-save'`

### R Packages

You will need `libcurl`. E.g.  
`sudo apt install libcurl4-openssl-dev`

For the Visual studio plugin you probably want `radian`:  
`pipx install radian`


The following are worth installing right away. Take them one at a time 
since installation errors are common.

```R
# For Visual Studio Code plugin
install.packages("languageserver")
install.packages("httpgd")
# For actual analysis :)
install.packages("tidyverse")
```

### Visual Studio Code R plugin

The good plugin is at https://marketplace.visualstudio.com/items?itemName=Ikuyadeu.r .
Once installed you can set up keyboard shortcuts: 
https://github.com/REditorSupport/vscode-R/wiki/Keyboard-shortcuts
and enable `httpgd`:  
`echo "options(vsc.use_httpgd = TRUE)" >> ~/.Rprofile`

### R Markdown

Needs `pandoc` and a copy of $LaTeX$.
https://alexd106.github.io/intro2R/install_rmarkdown.html . (I have not tried to get that going yet.)

## .NET

Installation: https://learn.microsoft.com/en-us/dotnet/core/install/linux-debian


## Fun and Optionalities

### Syntax Highlighting with `less`

With `highlight` (Best option)  

```sh
sudo apt install highlight
export LESS=-FMRXis
export LESSOPEN='| command highlight --force -O truecolor --style aiseered "%s"'
```

You can also just go `export LESS=" -R "` which I think is already the default in Debian.

There is also a nice more elaborate solution at https://superuser.com/a/1332042 

### Ctrl+D in PowerShell

To exit with Ctrl+D from PowerShell do 
```ps1
notepad $profile
```

and in the resulting editor put in the line
```ps1
Set-PSReadlineKeyHandler -Key ctrl+d -Function ViExit
```