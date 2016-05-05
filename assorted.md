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
* pidof and pgrep: shows PID of processes
* pkill crond = kill $(pidof crond)
* processe nice range: -20..+19 (highest..lowest)
* ps -l: shows nice
* nice -2 command = nice -n 2 command
* "nice -n" is better. It is not ambiguous
* renice -n <prio> -p <pid>  
* killing processes: kill pkill(pgrep) killall
* check if pkg <foo> is installed: yum list installed foo
* crontabs: /etc/crontab /etc/cron.d /var/spool/cron (users)
* /etc/crontab shows crontab format
* /var/spool/cron crontabs does not have the user field
* 
