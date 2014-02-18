# Rhys' .files

![Rhys' Terminal](http://i.imgur.com/8Vagc2o.png)

## Terminal

I use [Source Code Pro](http://sourceforge.net/projects/sourcecodepro.adobe/) (medium, size 12) for my font have 85% opacity with the [Solarized Colorscheme for Gnome Terminal](https://github.com/sigurdga/gnome-terminal-colors-solarized) for that sweet colour palet.


## ~/.bashrc

I have only modified the `force_color_prompt=yes` line and the terminal prompt PS1 line so I will only show those sections + some aliases


###PS1
I found that often my terminal prompt was longer than I needed it to be so I wrote this little hack to give me a variable prompt length.
```
bashrcpl=$(<.bashrcpl)
if [ $bashrcpl = "0" ] || [ "$(whoami)" = root ]; then
    if [ "$color_prompt" = yes ]; then
        if [ "$(whoami)" = root ]; then
            PS1='${debian_chroot:+($debian_chroot)}\[\033[0;31m\]\u\[\033[0;32m\]@\[\033[0;36m\]\h\[\033[0;32m\]:\[\033[01;34m\]\w\[\033[0;31m\]➤\[\033[1;31m\]➤\[\033[0;32m\]➤\[\033[01;34m\] '
        else
            PS1='${debian_chroot:+($debian_chroot)}\[\033[1;31m\]\u\[\033[0;32m\]@\[\033[0;36m\]\h\[\033[0;32m\]:\[\033[01;34m\]\w\[\033[0;31m\]➤\[\033[1;31m\]➤\[\033[0;32m\]➤\[\033[01;34m\] '
        fi

    else
        PS1='${debian_chroot:+($debian_chroot)}\u@\h:\w\$ '
    fi
else
    PS1='${debian_chroot:+($debian_chroot)}\[\033[0;32m\]➤\[\033[01;34m\] '
fi
```

This block of code gives me the variable length terminal prompt and the styping for the terminal prompt. The first line effectively does a cat on .bashrcpl and assigns that to bashrcpl, the rest is fairly self explanatory.


```
# Alias to turn short terminal prompt on or off
alias shortp='echo "1" > .bashrcpl & source ~/.bashrc &> /dev/null'
alias longp='echo "0" > .bashrcpl & source ~/.bashrc &> /dev/null'
```

These aliases write a ``0`` or ``1`` respectively to the .bashrcpl file and reload the .bashrc piping most output to /dev/null.

## ~/.ghci

``:set prompt "\ESC[1;31mλ \ESC[m"``

This gives me a minimalist orange lambda