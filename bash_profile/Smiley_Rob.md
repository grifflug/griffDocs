# Smiley Rob's dotfiles

This is a snapshot of my hidden profiles in my home directory. 

![Smiley Rob's Terminal](http://i.imgur.com/mdnN8tn.png)

## Terminal

Profile is set to **Pro**, with font set to **Monaco 10 Pt**. (with Antialias disabled).

## ~/.bash_profile

Notice that this .bash_profile loads in a collection of other dotfiles.

    #!/bin/bash
	
	for file in ~/.{bash_prompt,exports,aliases,functions,git-completion.bash,profile}; do
  		[ -r "$file" ] && source "$file"
  		echo "Loading $file"
	done
	unset file

	# Add tab completion for SSH hostnames based on ~/.ssh/config, ignoring wildcards
	[ -e "$HOME/.ssh/config" ] && complete -o "default" -o "nospace" -W "$(grep "^Host" ~/.ssh/config | grep -v "[?*]" | cut -d " " -f2)" scp sftp ssh

	PATH=$PATH:$HOME/.rvm/bin # Add RVM to PATH for scripting
	[[ -s "$HOME/.rvm/scripts/rvm" ]] && source "$HOME/.rvm/scripts/rvm" # Load RVM into a shell session *as a function*


## ~/.bash_prompt

This handles my super snazzy `PS1` which tells you which git branch I'm in.

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

## ~/.aliases

Check out that last alias to cd into the right folder, clear the screen, then give me the git status of what is dirty in this branch.
	
	#!/bin/bash
	
	alias BASH_EDIT='subl -nw ~/.bash_profile; echo NOW OPENING .bash_profile in SUBLIME TEXT' # Edit .bash_profile Alias

	# 2503ICT Web Programming
	WEBPROGRAMMING='/Users/iRC/Dropbox/01_University/2503ICT/workspace/auction_app'
	alias CD_AUCTION_APP='cd $WEBPROGRAMMING ; clear ; git st'

## ~/.functions

If you're using OSX, it doesn't have Linux's `ssh-copy-id` command... so I made one.

	#!/bin/bash
	
	# Pass in the username@server
	ssh_copy_id() {
	  cat ~/.ssh/id_rsa.pub | ssh "$@" 'cat >> ~/.ssh/authorized_keys' ;
	}

## ~/.gitconfig

    [core]
      editor = subl -n -w
      excludesfile = /Users/iRC/.gitignore_global
    [push]
      default = simple
    [color]
      ui = true
    [alias]
      lol = log --oneline --graph --decorate
	  st = status -s
	  co = checkout
	  cm = commit -m
	  cam = commit -am
	  cob = !git checkout -b $1 && git branch
	  as = !git add --all && git st
    [credential]
      helper = osxkeychain

## ~/.bashrc

    #!/bin/bash
	
	PATH=$PATH:$HOME/.rvm/bin # Add RVM to PATH for scripting

## ~/.gemrc

    #!/bin/bash
        
	gem: --no-document

## ~/.vimrc

There's more here in real life, except I'm not sure what it all does (I ripped it from someone else).

    syntax on

## /etc/motd

Not really a dotfile, but interesting nevertheless. <br>
`hack || die`. That's my message of the day, always.

	
	 __  __     ______     ______     __  __           __     __           _____     __     ______    
	/\ \_\ \   /\  __ \   /\  ___\   /\ \/ /          /\ \   /\ \         /\  __-.  /\ \   /\  ___\   
	\ \  __ \  \ \  __ \  \ \ \____  \ \  _"-.        \ \ \  \ \ \        \ \ \/\ \ \ \ \  \ \  __\   
	 \ \_\ \_\  \ \_\ \_\  \ \_____\  \ \_\ \_\        \ \_\  \ \_\        \ \____-  \ \_\  \ \_____\ 
	  \/_/\/_/   \/_/\/_/   \/_____/   \/_/\/_/         \/_/   \/_/         \/____/   \/_/   \/_____/ 
	                                                                                                  
	
	
