# Smiley Rob's dotfiles

This is a snapshot of my hidden profiles in my home directory.

![Smiley Rob's Terminal](http://i.imgur.com/mdnN8tn.png)

## Terminal

Profile is set to **Pro**, with font set to **Monaco 10 Pt**. (with Antialias disabled).

## ~/.dotfiles Directory

I have created a directory called `~/.dotfiles` which contains symlinks to these files. This makes it easier to access just dotfiles and not your whole home folder.

For a script on how to generate one yourself, visit [my gist](https://gist.github.com/rjchatfield/9133503).

## ~/.bash_profile

Notice that this .bash_profile loads in a collection of other dotfiles.

``` Bash
#!/bin/bash

declare -a DOTFILES_ARR=(~/.bash_prompt ~/.exports ~/.aliases ~/.functions ~/.git-completion.bash ~/.profile)

for file in ${DOTFILES_ARR[@]}; do
  echo "Loading $file"
  [ -r $file ] && source $file
done
unset file

# Add tab completion for SSH hostnames based on ~/.ssh/config, ignoring wildcards
[ -e "$HOME/.ssh/config" ] && complete -o "default" -o "nospace" -W "$(grep "^Host" ~/.ssh/config | grep -v "[?*]" | cut -d " " -f2)" scp sftp ssh

PATH=$PATH:$HOME/.rvm/bin # Add RVM to PATH for scripting
[[ -s "$HOME/.rvm/scripts/rvm" ]] && source "$HOME/.rvm/scripts/rvm" # Load RVM into a shell session *as a function*
```

## ~/.bash_prompt

This handles my super snazzy `PS1` which tells you which git branch I'm in.

``` Bash
#!/bin/bash

# @gf3’s Sexy Bash Prompt, inspired by “Extravagant Zsh Prompt”
# Shamelessly copied from https://github.com/gf3/dotfiles

default_username='rob'

if [[ $COLORTERM = gnome-* && $TERM = xterm ]] && infocmp gnome-256color >/dev/null 2>&1; then
    export TERM=gnome-256color
elif infocmp xterm-256color >/dev/null 2>&1; then
    export TERM=xterm-256color
fi

if tput setaf 1 &> /dev/null; then
    tput sgr0
    if [[ $(tput colors) -ge 256 ]] 2>/dev/null; then
        MAGENTA=$(tput setaf 9)
        ORANGE=$(tput setaf 172)
        GREEN=$(tput setaf 190)
        PURPLE=$(tput setaf 141)
        WHITE=$(tput setaf 256)
    else
        MAGENTA=$(tput setaf 5)
        ORANGE=$(tput setaf 4)
        GREEN=$(tput setaf 2)
        PURPLE=$(tput setaf 1)
        WHITE=$(tput setaf 7)
    fi
    BOLD=$(tput bold)
    RESET=$(tput sgr0)
else
    MAGENTA="\033[1;31m"
    ORANGE="\033[1;33m"
    GREEN="\033[1;32m"
    PURPLE="\033[1;35m"
    WHITE="\033[1;37m"
    BOLD=""
    RESET="\033[m"
fi


function git_info() {
    # check if we're in a git repo
    git rev-parse --is-inside-work-tree &>/dev/null || return

    # quickest check for what branch we're on
    branch=$(git symbolic-ref -q HEAD | sed -e 's|^refs/heads/||')

    # check if it's dirty (via github.com/sindresorhus/pure)
    dirty=$(git diff --quiet --ignore-submodules HEAD &>/dev/null; [ $? -eq 1 ]&& echo -e "*")

    echo $WHITE" on "$PURPLE$branch$dirty
}

# Only show username/host if not default
function usernamehost() {
   	if [ $USER != $default_username ]; then echo "${MAGENTA}$USER ${WHITE}at ${ORANGE}$HOSTNAME $WHITEin "; fi
}

# iTerm Tab and Title Customization and prompt customization
# http://sage.ucsc.edu/xtal/iterm_tab_customization.html

# Put the string " [bash]   hostname::/full/directory/path"
# in the title bar using the command sequence
# \[\e]2;[bash]   \h::\]$PWD\[\a\]

# Put the penultimate and current directory
# in the iterm tab
# \[\e]1;\]$(basename $(dirname $PWD))/\W\[\a\]

PS1="\n\[\e]2;$PWD\[\a\]\[\e]1;\]$(basename "$(dirname "$PWD")")/\W\[\a\]${BOLD}\$(usernamehost)\[$GREEN\]\w\$(git_info)\[$WHITE\]\n\$ \[$RESET\]"
```

## ~/.aliases

Check out that last alias to cd into the right folder, clear the screen, then give me the git status of what is dirty in this branch.

``` Bash
#!/bin/bash

alias BASH_EDIT='subl -nw ~/.dotfiles ; echo NOW OPENING ~/.dotfiles in SUBLIME TEXT' # Edit .bash_profile

# 2503ICT Web Programming
WEBPROGRAMMING='/Users/iRC/Dropbox/01_University/2503ICT/workspace/auction_app'
alias CD_AUCTION_APP='cd $WEBPROGRAMMING ; clear ; git st'

# tree is the new ls
alias tree='tree -ACL 2 --dirsfirst'

# Detect which `ls` flavor is in use
if ls --color > /dev/null 2>&1; then # GNU `ls`
    colorflag="--color"
else # OS X `ls`
    colorflag="-G"
fi

# Always use color output for `ls`
alias ls="command ls ${colorflag}"
export LS_COLORS='no=00:fi=00:di=01;34:ln=01;36:pi=40;33:so=01;35:do=01;35:bd=40;33;01:cd=40;33;01:or=40;31;01:ex=01;32:*.tar=01;31:*.tgz=01;31:*.arj=01;31:*.taz=01;31:*.lzh=01;31:*.zip=01;31:*.z=01;31:*.Z=01;31:*.gz=01;31:*.bz2=01;31:*.deb=01;31:*.rpm=01;31:*.jar=01;31:*.jpg=01;35:*.jpeg=01;35:*.gif=01;35:*.bmp=01;35:*.pbm=01;35:*.pgm=01;35:*.ppm=01;35:*.tga=01;35:*.xbm=01;35:*.xpm=01;35:*.tif=01;35:*.tiff=01;35:*.png=01;35:*.mov=01;35:*.mpg=01;35:*.mpeg=01;35:*.avi=01;35:*.fli=01;35:*.gl=01;35:*.dl=01;35:*.xcf=01;35:*.xwd=01;35:*.ogg=01;35:*.mp3=01;35:*.wav=01;35:'

# `cat` with beautiful colors. requires Pygments installed.
#                              sudo easy_install Pygments
alias CAT='pygmentize -O style=monokai -f console256 -g'

# Hide/show all desktop icons (useful when presenting)
alias hidedesktop="defaults write com.apple.finder CreateDesktop -bool false && killall Finder"
alias showdesktop="defaults write com.apple.finder CreateDesktop -bool true && killall Finder"

# Show/hide hidden files in Finder
alias showhidden="defaults write com.apple.finder AppleShowAllFiles -bool true && killall Finder"
alias hidehidden="defaults write com.apple.finder AppleShowAllFiles -bool false && killall Finder"

# Apache Derby DB
alias ij='java org.apache.derby.tools.ij'

# UMLet
alias umlet='open /Applications/Umlet/umlet.jar'

# Reload the shell (i.e. invoke as a login shell)
alias reload="exec $SHELL -l"

# Lock the screen (when going AFK)
alias afk="/System/Library/CoreServices/Menu\ Extras/User.menu/Contents/Resources/CGSession -suspend"

# IP addresses
alias ip="dig +short myip.opendns.com @resolver1.opendns.com"
alias localip="ipconfig getifaddr en1"

# Many flavours of sudo
alias yolo='sudo'
alias please='sudo'
```

## ~/.functions

If you're using OSX, it doesn't have Linux's `ssh-copy-id` command... so I made one.

``` Bash
#!/bin/bash

# Pass in the username@server
ssh_copy_id() {
	cat ~/.ssh/id_rsa.pub | ssh "$@" 'cat >> ~/.ssh/authorized_keys' ;
}

# Pass in the name of the master branch you wish to push to orgin
GIT_PUSH_SHUFFLE() {
  local currentBranchName=`git branch --no-color 2> /dev/null | sed -e '/^[^*]/d' -e "s/* \(.*\)/\1/"`
  local otherBranchName=$@
  echo You are in branch: \'$currentBranchName\'
  git co $otherBranchName
  echo Merging \'$currentBranchName\' with \'$otherBranchName\'
  git merge $currentBranchName
  echo Pushing to \'origin\'
  git push origin
  git co $currentBranchName
}

# Edit dotfiles Alias
dotfile_edit() {
  subl -nw ~/.bash_profile
  for file in ~/.{bash_prompt,exports,aliases,functions,git-completion.bash,profile}; do
    [ -r "$file" ] && source "$file"
  done
  unset file
  echo NOW OPENING dotfiles in SUBLIME TEXT
}

# Get OS X Software Updates, and update installed Ruby gems, Homebrew, npm, and their installed packages
update() {
  echo
  echo -- UPDATE: OS X Software --
    sudo softwareupdate -i -a
  echo
  echo -- UPDATE: Homebrew --
    brew update
    brew upgrade
    brew cleanup
  echo
  echo -- UPDATE: NPM --
    npm update npm -g
    npm update -g
  echo
  echo -- UPDATE: Gems --
    sudo gem update --system
    sudo gem update
}
```

## ~/.gitconfig

```
[core]
	editor = subl -n -w
	excludesfile = /Users/iRC/.gitignore_global
[push]
	default = simple
[color]
	ui = true
[alias]
    #log
    lol = log --oneline --graph --pretty='format:%C(auto)%d %s%Creset %>|(60)%C(dim)(%ar) %h%Creset'
    changes = log --stat --graph --pretty='format:%C(green bold)%s%Creset %n%C(dim)%h (%ar) [%cn]%Creset%C(auto)%d%Creset%n'
    summary = shortlog --reverse --pretty='format:%C(dim)(%ar)%Creset%C(auto)%d %s%Creset'
    st = status -s
    co = checkout -B

    as = !git add --all && git st

    # commit
    cm = commit -m                  # commit with message
    cam = commit -am                # commit all with message
    amend = commit --amend          # ammend your last commit
    ammend = commit --amend         # ammend your last commit

    # reset
    unstage = reset HEAD            # remove files from index (tracking)
    uncommit = reset --soft HEAD^   # go back before last commit, with files in uncommitted state

    # diff
    last = diff HEAD^                 # diff last committed change

    # via http://blog.apiaxle.com/post/handy-git-tips-to-stop-you-getting-fired/
    snapshot = !git stash save "snapshot: $(date)" && git stash apply "stash@{0}"
    snapshots = !git stash list --grep snapshot
[credential]
	helper = osxkeychain
```

## ~/.bashrc

``` Bash
#!/bin/bash

PATH=$PATH:$HOME/.rvm/bin # Add RVM to PATH for scripting
```

## ~/.gemrc

``` Bash
#!/bin/bash

gem: --no-document
```

## ~/.vimrc

There's more here in real life, except I'm not sure what it all does (I ripped it from someone else).

```
syntax on
```

## /etc/motd

Not really a dotfile, but interesting nevertheless. <br>
`hack || die`. That's my message of the day, always.

```
 __  __     ______     ______     __  __           __     __           _____     __     ______
/\ \_\ \   /\  __ \   /\  ___\   /\ \/ /          /\ \   /\ \         /\  __-.  /\ \   /\  ___\
\ \  __ \  \ \  __ \  \ \ \____  \ \  _"-.        \ \ \  \ \ \        \ \ \/\ \ \ \ \  \ \  __\
 \ \_\ \_\  \ \_\ \_\  \ \_____\  \ \_\ \_\        \ \_\  \ \_\        \ \____-  \ \_\  \ \_____\
  \/_/\/_/   \/_/\/_/   \/_____/   \/_/\/_/         \/_/   \/_/         \/____/   \/_/   \/_____/
```
