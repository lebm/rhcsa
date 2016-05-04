# Assorted tips

* &> -> redir both stdin and stderr (> FILE 2>&1)
* |& -> pipes both stdin and stderr (2>&1 |)
* unset V: undefine variable V
* Command Substitution: $(COMMAND) or \`COMMAND\`
* $(cat FILE) = $(< FILE) (but the second is faster)
* !! repeat the last command in history
* echo ~+ : shows current dir
* echo ~- : shows previous dir
* grep -w expr file: match lines were "expr" is a word
* "..." does not quote \\, $ or \'
* 
