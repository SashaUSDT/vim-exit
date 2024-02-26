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

