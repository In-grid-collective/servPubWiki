# Tmux plugins + logging plugin

Insatll TMUX plugin manager:

```
git clone https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm
```

You need to create a file called `.tmux.conf` in your user home folder to create a custom configuration setting (it doesn't really matter where it is, you will link it in tmux). This command will both create and let you edit the file in your current directory:
``` shell
nano .tmux.conf
```

Add this to the bottom of your conf file:
```  
# List of plugins
set -g @tpm_plugins '           \
   tmux-plugins/tpm             \
   tmux-plugins/tmux-sensible   \
   tmux-plugins/tmux-logging    \
'

# Initialize TMUX plugin manager (keep this line at the very bottom of tmux.conf)
run '~/.tmux/plugins/tpm/tpm'

```

Launch a new tmux session or attach to one that exits, use the [TMUX cheatsheet](https://tmuxcheatsheet.com/) if needed.

For each tmux session you will need to load the custom conf and run the install process for plugins to update the tmux environment.

Once inside your tmux session load the conf file, replace the path between <> so that it looks for your .tmux.conf file in the correct place.

``` shell
tmux source </yourpath/.tmux.conf>
```

Next do `ctrl b` (or your custom tmux prefix), next press `shift i` (capital I). This should log out some text if it's run correctly, the first time you do it it should say your plugins are installed, next time it would log something like this:

```
(LOG OUTPUT NOT CODE)
Already installed "tpm"                                                                       
Already installed "tmux-sensible"                               
Already installed "tmux-logging"

TMUX environment reloaded.      

Done, press ESCAPE to continue. 
```

Press esc. Next you should be ready to start using the logging commands from [Tmux logging](https://github.com/tmux-plugins/tmux-logging).

To start and stop logging do `ctrl b` and then `shift p` (capital p)

You should see a print out at the bottom of the TMUX session showing where the log is being written to. 


### Notes on tmux plugins:

Github repos, with instructions (however the above instructions were refined after much debugging, so reccommended that you follow the ones above):
- [Tmux plugin manager](https://github.com/tmux-plugins/tpm)
- [Tmux logging](https://github.com/tmux-plugins/tmux-logging)
- [Tmux Sensible settings](https://github.com/tmux-plugins/tmux-sensible)

You should be able to find an example tmux configuration file for other configuration settings, most likely at "/usr/share/doc/tmux/". Search for all tmux folders if you can't find that folder:
``` shell
find -name tmux
```

