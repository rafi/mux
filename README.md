mux
===

`mux` is a small helper for managing a pool of `Tmuxfile` project files.

Usage: `mux [project|command]`

### DISCOVER
Running `mux` without any arguments will try to discover
a tmux project file at `./Tmuxfile` and source it.

### PROJECTS
If the project argument is included, `mux` will try to load it from its memory pool.
The memory pool is a folder `mux` keeps with symlinks to your projects.

### COMMANDS

 - `a[ttach]`   Attach to an existing tmux session or create a new one
 - `l[s]`       List available projects in memory pool and tmux sessions
 - `ad[d]`      Add a project to memory pool
 - `r[emove]`   Remove a project from memory pool
 - `h[elp]`     Show this message

### EXAMPLES
#### Listing
See available projects and opened tmux sessions: `$ mux ls`

#### Adding
Discover project's `Tmuxfile`:
```
$ cd /srv/http/foobar
$ mux add
```

Adding a custom tmux project file: `$ mux add /some/where/foobar.sh`

#### Loading and Attaching
Load or attach to current project's session:
```
$ cd /srv/http/foobar
$ mux
```

Load/attach to project's session by name: `$ mux foobar`

Attach to an existing session or create a new one: `$ mux attach`

#### Removing
Remove current project from memory pool: (deletes cached symlink)
```
$ cd /srv/http/foobar
$ mux remove
```

Remove a specific custom tmux project file: `$ mux remove foobar.sh`

#### Tmuxfile Example
Here is a typical `Tmuxfile` example:
```
#!/bin/bash
#
# PROJECT NAME
# tmux workspace

# Allows running this script without 'mux'
[ -z "$name" ] && name=$(basename $(pwd))

tmux new-session -d -s $name -n dev

# Horizontal: Vagrant SSH and PostgreSQL, show schemas and tables
tmux split-window -t $name:1.1 -h -p 48 "vagrant ssh -c 'psql $name'"
tmux send-keys -t $name:1.2 "\\dn \\dt" C-m

# Vertical: ranger
tmux split-window -t $name:1.2 -v -p 35 "ranger"

# Vertical bottom: Vagrant SSH and grunt watch
tmux split-window -t $name:1.1 -v -p 8 "vagrant ssh -c 'cd app && grunt watch"

# Vertical: Watching git log
tmux split-window -t $name:1.1 -v -p 30 "watch -c -n 4 git -c color.diff=always tree"

# Select first pane and run some commands
tmux send-keys -t $name:1.1 "clear && head README* && ll apps && ll" C-m
tmux select-pane -t $name:1.1

tmux attach-session -t $name
```

### LICENSE
The MIT License (MIT)

Copyright (c) 2014 Rafael Bodill

