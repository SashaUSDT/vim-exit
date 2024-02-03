# vim-exit
Below are some simple methods for exiting vim.

```vim
:!ps axuw | grep vim | grep -v grep | awk '{print $2}' | xargs kill -9
```
