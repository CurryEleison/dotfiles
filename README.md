# dotfiles
Notes and Scripts I use


## Installations

On Debian some prereqs to make anything work:

- wget, curl, git, gcc, make, python3, python3-pip

Additionally:

- vim (or nvim)
- micro
- git
- zsh
- oh-my-zsh (https://ohmyz.sh/#install)

On standalones:

- openvpn
- ssh server
- autosuspend (maybe)


## Zsh

- Change to zsh with `chsh`
- Install oh-my-zsh with  
`sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"`
- Ensure `LC_CTYPE` (or `LANG`) is set to `en_US.UTF-8`
- Install zsh-autosuggestions:  
`git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions`
- Install zsh-syntax-highlighting  
`git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting`

### .zshrc

```sh
# Current favorite theme
ZSH_THEME="fishy"

# Update oh-my-zsh when it's time
zstyle ':omz:update' mode reminder

# YMD format in history command
HIST_STAMPS="yyyy-mm-dd"

# Plugins
plugins=(
        git
        asdf
        ssh-agent
        zsh-autosuggestions
        zsh-syntax-highlighting
        )

# Alias for python if Debian doesn't set it up
alias python=python3
```
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

Install pipenv into `~/.local/` with  
`pip install pipenv`

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


## AWS

AWS cli v2 install from https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html

Optionally set default region in `~/.zshenv` with  
`export AWS_DEFAULT_REGION=eu-west-1`

### awsume

...

## sudo


Can specify visudo editor with  
`Defaults        editor=/usr/bin/micro`

Specify a group sudo with  
`%sudo   ALL=(ALL:ALL) ALL`

Place overrides at _end_ of sudoers file
