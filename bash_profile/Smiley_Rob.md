# Smiley Rob's .Profiles

This is a snapshot of my hidden profiles that

## ~/.bash_profile

I'm especially impressed with the `PS1` for showing my git branch. And if you're using OSX, it doesn't have Linux's `ssh-copy-id` command - so I made one.

    # EDIT .BASH_PROFILE ALIAS
    alias BASH_EDIT='subl -nw ~/.bash_profile; echo NOW OPENING .bash_profile in SUBLIME TEXT'
    export EDITOR='subl -w'

    # TERMINAL CURSOR
    function parse_git_dirty {
      git diff --no-ext-diff --quiet --exit-code &> /dev/null || echo "*"
    }
    function parse_git_branch {
      git branch --no-color 2> /dev/null | sed -e '/^[^*]/d' -e "s/* \(.*\)/(\1$(parse_git_dirty))/"
    }
    export CLICOLOR=1
    export PS1="\n\e[0;36m\w\[\033[00m\] \$(parse_git_branch)\n\e[0;32m\u\e[0;00m \$ "

    # SSH
    alias SSHGRIFFLUG='ssh rob@grifflug.com'
    alias SSHLOCALMACBIAN='ssh rob@192.168.1.2'
    alias SSHLOCALPI='ssh pi@192.168.1.3'
    alias SSHDWARF='ssh s2187099@dwarf.ict.griffith.ed.au'
    ssh_copy_id() {
      cat ~/.ssh/id_rsa.pub | ssh "$@" 'cat >> ~/.ssh/authorized_keys' ;
    }

    # 2503ICT WEB PROGRAMMING
    alias CD_AUCTION_APP='cd /Users/iRC/Dropbox/01_University/2503ICT/workspace/auction_app'

    # MISC
    export PATH=/usr/local/bin:$PATH
    [[ -s "$HOME/.rvm/scripts/rvm" ]] && source "$HOME/.rvm/scripts/rvm" # Load RVM into a shell session *as a function*

## ~/.bashrc
    PATH=$PATH:$HOME/.rvm/bin # Add RVM to PATH for scripting

## ~/.gemrc
    gem: --no-document

## ~/.gitconfig
    [user]
      name = Robert j Chatfield
      email = robert.chatfield@griffithuni.edu.au
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
      cam = commit -am
      as = !git add --all && git st
    [credential]
      helper = osxkeychain
    [difftool "sourcetree"]
      cmd = opendiff \"$LOCAL\" \"$REMOTE\"
      path =
    [mergetool "sourcetree"]
      cmd = /Applications/SourceTree.app/Contents/Resources/opendiff-w.sh \"$LOCAL\" \"$REMOTE\" -ancestor \"$BASE\" -merge \"$MERGED\"
      trustExitCode = true

## ~/.vimrc
    syntax on
