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
