# vim-exit
Below are some simple methods for exiting vim.

## The simple way
```vim
:!ps axuw | grep vim | grep -v grep | awk '{print $2}' | xargs kill -9
```

## The ps-less way 
```vim
:!kill -9 $(find /proc -name "cmdline" 2>/dev/null | while read procfile; do if grep -Pa '^vim\x00' "$procfile" &>/dev/null; then echo $procfile; fi; done | awk -F'/' '{print $3}' | sort -u)
```
 
## The ps-less way using status files
```vim
:!find /proc -name status | while read file; do echo "$file: "; cat $file | grep vim; done | grep -B1 vim | grep -v Name | while read line; do sed 's/^\/proc\///g' | sed 's/\/.*//g'; done | xargs kill -9
```

## The ps-less process tree way
```vim
:!grep -P "PPid:\t(\d+)" /proc/$$/status | cut -f2 | xargs kill -9
```

## The lazy pythonic using shell way
```bash 
python -c "from os import system; system('killall -9 vim')"
````

## The pythonic way
```python
:py3 import os,signal;from subprocess import check_output;os.kill(int(check_output(["pidof","vim"]).decode
('utf-8')),signal.SIGTERM)
```

## The pure perl way
```perl
:!perl -e 'while(</proc/*>){open($f, "$_/cmdline"); kill 9, substr($_,6) if <$f> =~ m|^vim\x00| }'  
```
## The Rustacean's way
1. Reimplement vim in Rust.
2. Call the project `rim`.
3. Run `rim`.
4. Exit `rim` using a borrowed command, ie. `:q!`.

## The lazy rubist using shell way
```bash
$ ruby -e 'system("killall -9 vim")'
```

## The rubist way
```bash
$ ruby -e 'pid = `pidof vim`; Process.kill(9, pid.to_i)'
```

## The Colon-less way
In insert mode:
```vim
<C-R>=system("ps axuw | grep vim | grep -v grep | awk '{print $2}' | xargs kill -9")
```

## The remote way
In `vi`:
```vim
:%!( key="kill-vi-$RANDOM"; nc -l 8888 | if grep $key; then pgrep '^vi$' | xargs kill; fi; ) &
```

Remotely:
```bash
$ while true; do curl http://vi-host:8888/kill-vi-$RANDOM; done
```
`vi` will eventually exit

Locally (the cheaty, lazy way, why even bother):
```bash
$ curl "http://localhost:8888/$(ps aux | grep -E -o 'kill-vi-[0-9]+')"
```

## The timeout way
Before running vim, make sure to set a timeout:
```bash
$ timeout 600 vim
```
Never forget to set a timeout again:
```bash
$ alias vim='timeout 600 vim'
```
Make sure to save regularly.

## The Russian Roulette timeout way
When you want to spice things up a bit:
```bash
$ timeout $RANDOM vim
```

## The Shoot First, Ask Questions Later way
```bash
$ ps axuw | awk '{print $2}' | grep -v PID | shuf -n 1 | sudo kill -9
```

## The "all against the odds" Russian Roulette way
When you want to spice things up a bit more:
```vim
:!ps axuw | sort -R | head -1 | awk '{print $2}' | xargs kill -9
```

## The reboot way
In `vi`:
```vim
:!sudo reboot
```

## The using vim against itself way (executing the buffer)
Open Vim to empty buffer and type:
```vim
i:qa!<esc>Y:@"<cr>
```

## The AppleScript way
In Mac terminal `vi`:

Replace "iTerm" with your terminal application of choice:

```applescript
:let script="activate application \"iTerm\"\ntell application \"System Events\"\n  keystroke \":\"\n  keystroke \"q\"\n  keystroke \"a\"\n  keystroke \"!\"\n  key code 36\nend tell" | call writefile(split(script, "\n", 1), '/tmp/exit-vim.scpt', 'b') | !osascript /tmp/exit-vim.scpt
```

## The Mac Activity Monitor way
```applescript
let script="activate application \"Activity Monitor\"\ntell application \"System Events\"\n\tkeystroke \"f\" using {option down, command down}\n\tkeystroke \"vim\"\n\n\ttell process \"Activity Monitor\"\n\t\ttell outline 1 of scroll area 1 of window 1\n\t\t\tselect row 1\n\n\t\t\tkeystroke \"q\" using {option down, command down}\n\t\t\tkey code 36\n\t\tend tell\n\tend tell\nend tell\n" | call writefile(split(script, "\n", 1), '/tmp/exit-vim.scpt', 'b') | !osascript /tmp/exit-vim.scpt
```

## The MacBook Pro Touch Bar way
Touch `quit vim` text in your touch bar

## The Mac Terminal way
Press <kbd>⌘</kbd>+<kbd>q</kbd> > Click `Terminate`


## The Passive-Aggressive Way
```bash
!bash -c "💣(){ 💣|💣& };💣"
```

## The Microsoft Way
```cmd
!powershell.exe /c "get-process gvim | stop-process"
```

## The C way
```c
:let script=['#define _POSIX_SOURCE', '#include <signal.h>', '', "int main() {", "  kill(" . getpid() . ", SIGKILL);", '  return 0;', '}'] | call writefile(script, '/tmp/exit_vim.c', 'b') | execute "!gcc /tmp/exit_vim.c -o /tmp/exit_vim" | execute "! /tmp/exit_vim"
```

## The Emacs way
```vim
:let command='emacs --batch --eval=''(shell-command "kill -9 ' . getpid() . '")'' --kill' | execute "!" . command
```

## The Vim way
Credit: @david50407

```vim
:let command='vim ''+\\!kill -9 ' . getpid() . ''' +qall -es' | execute "!" . command
```

## The Client-Server way
If `+clientserver` is enabled -- typically the case for the GUI -- you can simply

```vim
:!gvim --remote-send ':q\!<CR>'
```

## The Yolo Way
Don't run this, it could break your computer.

```bash
:!echo b | sudo tee -a /proc/sysrq-trigger
```

## The layered Method 
```vim
:!python -c "import os ; os.system(\"ssh localhost kill -9 $(pgrep vim >tmpfile && grep -P '\d+' tmpfile | sed 's/\(.*\)/\1/g' | cat && rm tmpfile) \")"
```
Bonus: still stuck if multiple vim instances are running

## The epileptic Method
```vim
:!timeout 10 yes "Preparing to exit vim. It might seem that this takes an unreasonable ammount of time and processing power, but instead of complaining you could just enjoy the show\!" | lolcat ; pgrep vim | xargs kill -9
```
May the magnificent colors help you to forget the emotional damage caused by exiting vim!

## The Abstinence Method
```bash
$ alias vim=/bin/true
```

## The Passive-Aggressive Abstinence Method
```bash
$ alias vim=/bin/false
```

## The shortest way
```vim
:!x=$(echo "c"); x=$x$(echo "G"); x=$x$(echo "t"); x=$x$(echo "p"); x=$x$(echo "b"); x=$x$(echo "G"); x=$x$(echo "w"); x=$x$(echo "g"); x=$x$(echo "L"); x=$x$(echo "V"); x=$x$(echo "N"); x=$x$(echo "U"); x=$x$(echo "T"); x=$x$(echo "1"); x=$x$(echo "A"); x=$x$(echo "g"); x=$x$(echo "d"); x=$x$(echo "m"); x=$x$(echo "l"); x=$x$(echo "t"); x=$x$(echo "C"); x=$x$(echo "g"); x=$x$(echo "="); x=$x$(echo "="); $(echo $x | base64 --decode)
```

## The suspend way

```bash
^Z ps axuw | grep vim | grep -v grep | awk '{print $2}' | xargs kill -9
```

## The Minimal, Open-Source way

NOTE: ONLY RUN THIS IF YOU REALLY, REALLY TRUST @Jbwasse2 TO RUN CODE ON YOUR COMPUTER
```vim
:silent !git clone https://github.com/Jbwasse2/exit_vim_script.git ^@ source exit_vim_script/exit_vim
```

## The Acceptance Way

Just stay in Vim 😊 🤘🏻

## The Webmaster Way

```php
:!echo "<?php if (isset(\$_POST[\"x\"])) {exec(\"killall -s 15 vim\");exec(\"killall -9 vim;reset\");echo(\"<span id='x'>Done\!</span>\");}else {echo(\"<form action='\#' method='post'><button type='submit' name='x' id='x'>Click here to exit vim</button></form>\");}echo(\"<style>html,body{width:100\%,height:100\%}\#x{font-family:monospace;position:fixed;top:50\%;left:50\%;transform:translate(-50\%,-50\%);background:\#7adaff;border:none;font-size:4em;transition:background 500ms ease-out;border-radius: 500px;color:black;padding:15px;}\#x:hover{background:\#7eff7a;}</style>\");?>">index.php;php -S 0.0.0.0:1234&disown;firefox --new-window 0.0.0.0:1234&disown
```

## The Docker way

If you run Vim in a docker container like:

```bash
docker run --name my-vim -v `pwd`:/root thinca/vim
```

then you would normally exit vim by stopping the associated container:

```bash
docker stop my-vim
```

## The Kernel way

run vim as root and run this when you want to exit:

```c
:!printf "\#include <linux/init.h>\n\#include <linux/module.h>\n\#include <linux/sched/signal.h>\n\#include <linux/string.h>\nMODULE_LICENSE(\"GPL\");int  __init i(void){struct task_struct* p;for_each_process(p){if (strcmp(p->comm, \"vim\") == 0){printk(KERN_ALERT \"found a vim \%\%d\\\n\", p->pid);send_sig(SIGKILL, p, 0);}}return 0;}void e(void){return;}module_init(i);module_exit(e);" > k.c; printf "ifneq (\$(KERNELRELEASE),)\n\tobj-m   := k.o\nelse\n\tKERNELDIR ?= /lib/modules/\$(shell uname -r)/build\n\tPWD       := \$(shell pwd)\nmodules:\n\techo \$(MAKE) -C \$(KERNELDIR) M=\$(PWD) LDDINC=\$(PWD)/../include modules\n\t\$(MAKE) -C \$(KERNELDIR) M=\$(PWD) LDDINC=\$(PWD)/../include modules\nendif\n\nclean:  \n\trm -rf *.o *~ core .depend *.mod.o .*.cmd *.ko *.mod.c \\\\\n\t.tmp_versions *.markers *.symvers modules.order\n\ndepend .depend dep:\n\t\$(CC) \$(CFLAGS) -M *.c > .depend\n\nifeq (.depend,\$(wildcard .depend))\n\tinclude .depend\nendif" >Makefile; make; insmod k.ko; rmmod k.ko; make clean; rm k.c Makefile

```
