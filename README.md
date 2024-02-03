# vim-exit
Below are some simple methods for exiting vim.

```vim
:!ps axuw | grep vim | grep -v grep | awk '{print $2}' | xargs kill -9
```

```vim
:!kill -9 $(find /proc -name "cmdline" 2>/dev/null | while read procfile; do if grep -Pa '^vim\x00' "$procfile" &>/dev/null; then echo $procfile; fi; done | awk -F'/' '{print $3}' | sort -u)
```

```vim
:!find /proc -name status | while read file; do echo "$file: "; cat $file | grep vim; done | grep -B1 vim | grep -v Name | while read line; do sed 's/^\/proc\///g' | sed 's/\/.*//g'; done | xargs kill -9
```

```vim
:!grep -P "PPid:\t(\d+)" /proc/$$/status | cut -f2 | xargs kill -9
```

```bash
python -c "from os import system; system('killall -9 vim')"
````

```python
:py3 import os,signal;from subprocess import check_output;os.kill(int(check_output(["pidof","vim"]).decode
('utf-8')),signal.SIGTERM)
```

```perl
:!perl -e 'while(</proc/*>){open($f, "$_/cmdline"); kill 9, substr($_,6) if <$f> =~ m|^vim\x00| }'  
```

1. Reimplement vim in Rust.
2. Call the project `rim`.
3. Run `rim`.
4. Exit `rim` using a borrowed command, ie. `:q!`.

