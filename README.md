This is a mirror of http://www.vim.org/scripts/script.php?script_id=2407

Contains a set of files useful to edit Matlab files in vim.

### Installation
* Clone the repository or install using a plugin manager
* To use `mlint` as code checker (linter): 
First locate `mlint`. The location of the binary `matlab` can be found using `whereis matlab`.  My output is `matlab: /usr/local/bin/matlab`. A quick `ls -l` shows that it is actually a soft link to `/usr/local/MATLAB/R2020a/bin/matlab`. The binary `mlint` must be nearby. Turns out, `mlint` is in the location `/usr/local/MATLAB/R2020a/bin/glnxa64/mlint`.
Create a soft link in `/usr/local/bin/` of `mlint` using
```
cd /usr/local/bin/
ln -s /usr/local/MATLAB/R2020a/bin/glnxa64/mlint mlint
```
Now, `mlint` can be invoked from the terminal like this: `mlint filename.m`.

Therefore, `$HOME/.vim/bundle/vim-matlab/compiler/mlint.m` can access it. 
Now, after opening a `.m` file, we can issue `:make` and then `:copen` to see error messages. Optionally, we can combine these options together and another line to the file `ftplugin/matlab.vim`
```
map <silent> <Leader>ll :make<CR>:copen<CR>
```
This launches the errors when we press `\ll`.

* Enable matlab jump of cursor between `for` and associated `end` using `%`:
`matchit` has been added to the main vim version since v6. To enable it add to .vimrc:
```
if !exists('g:loaded_matchit') && findfile('plugin/matchit.vim', &rtp) ==# ''
  runtime! macros/matchit.vim
endif
```
See more help with `:h matchit`.
Now, jump between `for/end` (for example) by pressing `%`.

### Details

Included is :
1) Syntax highlighting
2) Correct setting to use the matchit.vim script (extension of the % command to match if/end, for/end,... blocks)
3) Correct indentation
4) Integration of mlint (Matlab code checker) with the :make command
5)Tag support
6) Help file


1) Syntax highlighting

syntax/matlab.vim : Updates the matlab.vim syntax file provided in the vim distribution :
- highlights keywords dealing with exceptions : try / catch / rethrow
- highlights keywords dealing with class definitions : classdef / properties / methods / events
- highlights most Matlab functions

2) Correct settings in order to use the matchit.vim script 

The matchit.vim extends the % matching and enables to jump through matching groups such as "if/end" or "swicth/end" blocks (see :help matchit in vim)

ftplugin/matlab.m provides the suitable definition for b:match_words in order to jump between if/end, classdef/end, methods/end, events/end, properties/end, while/end, for/end, switch/end, try/end, function/end blocks

3) Correct indentation
indent/matlab.vim : Updates the matlab.vim indention file provided in the vim distribution.
This script provides a correct indentation for :
- switch / end, try / catch blocks
- classdef / methods / properties / events
- mutli-line (lines with line continuation operator (...))

This script has been tested with the Matlab R2008a release on many files and the result of indentation compared to the one provided by the Matlab Editor (with 'indent all functions' option set)

NOTE : to work correctly, this script need the matchit.vim (vimscript#39) to be installed.

4) Integration of mlint (Matlab code checker) with the :make command

compiler/mlint.m provides the settings to use mlint (Matlab code ckecker) and puts the messages reported in the quickfix buffer.

Whenever you want to check your code, just type :make and then :copen and vim opens a quickfix buffer which enables to jump to errors (using :cn, :cp or Enter to jump to the error under the cursor : see :help quickfix in vim)

5)Tag support
The .ctags file (in the matlab.tar.gz) defines the Matlab language so that the exuberant ctags (http://ctags.sourceforge.net ) can construct the tag file : you can now jump to tags (using CTRL-] (or CTRL-$ if using Windows) and go back again (CTRL-T)
See also :help tags in vim.


These scipts have been tested using gvim 7.2 and Matlab R2008a on Windows.




