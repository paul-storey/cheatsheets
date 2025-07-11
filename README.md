# airflow
## Configuring tasks
The 2 ways I have found for injecting configuration values into DAG tasks are 
1. trigger the DAG with a configuration object
2. use Variables
### Trigger the DAG with a configuration object
```
airflow dags trigger --conf '{"conf1": "value1"}' example_parameterized_dag
```
This can also be done using the UI.

In the code:
```
parameterized_task = BashOperator(
    task_id="parameterized_task",
    bash_command="echo value: {{ dag_run.conf['conf1'] }}",
    dag=dag,
)
```
Note: The parameters from dag_run.conf can only be used in a template field of an operator.

### Use Variables
Airflow provides an API which the task writer can use to retrieve a variable value.
The variables can be defined using the Airflow UI.
This is similar to the way that named connections (e.g. to databases) can be created using the UI.
```
from airflow.models import Variable

foo = Variable.get("foo")
```

# bash
## Example of using an array
```
#!/bin/bash
files=("111.xml" "1110.xml" "1111.xml" "1112.xml" "1113.xml")
for file in "${files[@]}"
do
    echo ${file}
done
```
## Pulling in values from a file
```
#!/bin/bash
input="temp/my.file"
while IFS= read -r line
do
    echo "${line}"
done < "${input}"
```

# curl
to include response headers: `-i`

to follow redirects: `-L`

verbose: `-v`

output to file: `--output my.file`

add a header: `-H "accepting-encoding: deflate, br"`

# docker
## Bring services down and removal all trace of them
```
docker-compose down --volumes --remove-orphans
```
## Get last 5 minutes of logging activity for a container
```
docker logs --since 5m my-container
```
## Get the IP address of a container
```
docker inspect mynginx | grep IPAddress
```
## Stop all containers
```
docker stop $(docker ps -q)
```
## Remove stopped containers
```
docker rm --volumes $(docker ps --filter status=exited -q)
```
Use `--volumes` to remove associated anonymous volumes.

Alternatively
```
docker system prune
```

# GIMP
## Highlight part of an image
1. Open the image in GIMP
2. Use the select tool to select a region
3. Select the Bucket Fill Tool
4. Select the foreground colour
5. Specify 30 as the Opacity for the Bucket Fill Tool Options.
6. Click within the region
# git
## Delete a local branch
```
git branch -d name-of-branch
```
## Delete remote branch
```
git push origin --delete new-branch
```
## Disable escaping of unicode multibyte characters in path names
```
git config --global core.quotePath false
```
## Diff before commit
if the file has not yet been added...
```
git diff PATH_TO_FILE
```
if the file is already staged
```
git diff --cached PATH_TO_FILE
```
## Diff for review
```
git log -p
```
## difftool
`git difftool` allows a side-by-side comparison.

One option is to use vimdiff, although configuring the diff in vim can be tricky.
```
git difftool --tool=vimdiff 848916a
````
This uses 2 panes. The left shows the version at the specified commit. The right shows HEAD.

Alternatively, install [meld](http://meldmerge.org/), edit `.gitconfig` as follows:
```
# Add the following to your .gitconfig file.
[diff]
    tool = meld
[difftool]
    prompt = false
[difftool "meld"]
    cmd = meld "$LOCAL" "$REMOTE"
```
and then
```
git difftool 848916a
```
## Undo all unstaged modifications
For more recent versions of git: `git restore .`

For older versions of git: `git checkout -- .`

## Push a (new) local branch to the remote
The following command creates a remote branch and creates an association between the local and the remote branch. This means that one can subsequently simply run 'git push' or 'git pull' without having to care about specifying branch)
```
git push -u origin new-branch
```
# ISO dates
1BC is `+0000` and 2BC is `-0001`

# JSON
How to pretty print JSON on the command line
```
curl "http://host/my.json" | python3 -m json.tool
```

# nc
connect to port 42 on the specified host, giving up after 5 seconds
```
nc -w 5 host.example.com 42
```
# netstat
for TCP: `-t`

to see the process id (pid): `-p`

switch off hostname resolution: `-n` (i.e. *numeric*)

netstat example on Windows:
```
netstat -aon | find /i ":25"
```
netstat isn't always available on Linux. Try:
```
sudo ss -tunlp
```

# Poetry
To make a poetry project out of an existing project
```
poetry init
```
If `poetry install` gives you a 'keyring' prompt (*An application wants to create a new keyring called Default. Choose the password you want to use for it.*), this appears to be a [known issue](https://github.com/python-poetry/poetry/issues/1917) and may be circumvented as follows:
```
export PYTHON_KEYRING_BACKEND=keyring.backends.null.Keyring
```

# Python
## Debug FastAPI
Add this line of code:
```
import pdb; pdb.set_trace()
```
## Break into pdb on exception
```
try:
    stuff()
except Exception:
    import pdb; pdb.post_mortem()
```
## Run pydoc
```
python -m pydoc -b
```
## [Embedding an interactive shell](https://stackoverflow.com/questions/5597836/embed-create-an-interactive-python-shell-inside-a-python-program)
```
import code
variables = globals().copy()
variables.update(locals())
shell = code.InteractiveConsole(variables)
shell.interact()
```
## reload a module
```
from importlib import reload
reload(math)
```

# redhat
## enable HTTP traffic
```
sudo firewall-cmd --permanent --zone=public --add-service=http
sudo firewall-cmd --reload
```
## enable traffic on a particular port
```
sudo firewall-cmd --zone=public --add-port=6363/tcp --permanent
```
## use alternatives to configure python3
```
sudo alternatives --install $(which python3) python3 $(which python3.8) 1
sudo alternatives --config python3
```

# screen

`screen -S session_name` Start screen with the specified name, in a new shell

`Ctrl+a` `c` Create a new window (with shell).

`Ctrl+a` `S` Split current region horizontally into two regions.

`Ctrl+a` `|` Split current region vertically into two regions.

`Ctrl+a` `tab` Switch the input focus to the next region.

`Ctrl+a` `X` Close the current region.

`Ctrl+a` `d` Detach

`screen -r` to resume

`screen -ls` to list sessions

# sed
sed example using character classes and references to matched groups
```
sed 's/.*\/\([[:alnum:]_]*\.xml\):.*id="\(manuscript_[[:digit:]]*\)".*/\1:\2/'
```

# solr
Delete all docs
```
curl -X POST -H 'Content-Type: application/json' --data-binary '{"delete":{"query":"*:*" }}' http://localhost:8983/solr/my_collection/update
```

# ssh
To enable password-based ssh connection, `sudo vi /etc/ssh/sshd_config` and change `PasswordAuthentication` from 'no' to 'yes'. Then `sudo systemctl restart sshd`.

To generate SSH key pair:
```
ssh-keygen -t rsa -b 2048
```

To copy a public key to another machine:
```
ssh-copy-id user@target
```
(This will prompt for password for access to target). The effect of this command is to append the public key to `authorized_keys`.

Port forwarding example:
```
ssh -L 8008:127.0.0.1:8008 user@host
```

Test ssh connection:
```
ssh -v -T git@gitlab.bodleian.ox.ac.uk
```
From the output it is possible to see which SSH keys are being offered to the host.
Sometimes an SSH key is not offered. This can be configured using `~/.ssh/config`.
Example:
```
Host gitlab.bodleian.ox.ac.uk
    IdentityFile C:\Users\paulb/.ssh/id_ed25519_bod_gitlab
    IdentitiesOnly yes
    AddKeysToAgent yes
```

Get the fingerprint of a public key
```
ssh-keygen -l -E md5 -f ./id_ed25519.pub
```

# sudo
Add a new user
```
sudo adduser yoda
```
Define a password for this user
```
sudo passwd yoda
```
Add user to the sudo group
```
sudo usermod -aG wheel yoda
```
To enable sudo without needing to provide a password, `sudo visudo`, uncomment the line which follows the comment *same thing without a password*.

# tcpdump
## Use -A for ascii output
``` 
tcpdump -A -i any  port 8080
```
## Another example
```
tcpdump -i any -nntX port 80
```
Refer to [Daniel Miessler's tutorial](https://danielmiessler.com/study/tcpdump/).

## Test if packets are arriving from a particular source
```
tcpdump -i any src 192.168.60.5
```
This is useful to verify whether or not it's the host firewall that's causing
connection rejections - because tcpdump still 'sees' the arriving packets,
despite the fact the firewall is going to reject them.

# tmux
- `Ctrl-B %` for a vertical split (one shell on the left, one shell on the right)
- `Ctrl-B"` for a horizontal split (one shell at the top, one shell at the bottom)
- `Ctrl-B o` to make the **o**ther shell active
- `Ctrl-B ?` for help
- `Ctrl-B d` detach from Tmux, leaving it running in the background (use tmux attach to reenter)
- `Ctrl-B x` to close the current pane
- `Ctrl-B {` to rotate panes

To start a new tmux server, use `tmux -L [name]`. This is useful if you've already got tmux running in one terminal and you want to run tmux in a second terminal but, in the second case, you don't want your processes to inherit the environment that they would inherit were they to use the tmux server associated with the first.

## tmux session management
```
tmux new-session -s my-session-name
```
```
tmux list-sessions
```
```
tmux attach-session -t my-session-name
```
To detach: `Ctrl-B` then `D`

# top
`M` - sort by memory usage

`P` - sort by CPU usage

`E` - change unit in which memory usage is reported

`c` - show full paths of processes

`m` - cycle through alternative displays for memory usage

`t` - do the same for CPU usage

# vagrant
Generate a new Vagrantfile:
```
vagrant init
```
List available boxes
```
vagrant box list
```
Add a box. 

First, [find a box](https://app.vagrantup.com/boxes/search). Then use the following command to add it.
```
vagrant add "centos/7"
```
Provision and start VMs
```
vagrant up
```
Stop the VMs
```
vagrant halt
```
Stop and get rid of everything:
```
vagrant destroy
```
To list VMs
```
vagrant status
```
To SSH to a VM
```
vagrant ssh vagrant_name_for_vm
```
Example VM definition
```
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
# General Vagrant VM configuration.
# config.vm.box = "ansible.box"
config.ssh.insert_key = false
config.vm.synced_folder ".", "/vagrant", disabled: true
config.vm.provider :virtualbox do |v|
v.memory = 512
v.linked_clone = true
end

# Ansible Controller
config.vm.define "controller" do |controller|
controller.vm.box = "centos/7"
controller.vm.hostname = "controller.test"
controller.vm.network :private_network, ip: "192.168.60.4"
end
end
```

# vi
## apply edit on multiple lines
1. Select the lines: `Ctl-v` to enter selection mode, then the usual keys eg `j` or `G` to select 
a block of lines.
2. Perform the edit eg `I` to insert at the beginning of the line. Note that, at this point, the UI may not show the 
effects of the edit for the whole block.
3. `Esc` when done
## auto-indent off when pasting
```
:set paste
```
and, when done:
```
:set nopaste
```
## copy characters
With the cursor on the first character, `v` then move to select text. `y` to yank.
## turn off highlighting from last search
```
:noh
```
Also, the following might help to stop vim from highlighting every occurrence
of a match after doing a substitution:
```
:set nohlsearch
```
## List colors
```
ls -l /usr/share/vim/vim*/colors
```
To apply:
```
:color elflord
```
## Tabs
```
set tabstop=4 shiftwidth=4 expandtab
```
## Vertical split
With `Ctl-w` pressed, hit `v`
## Whitespace
To see if some tab characters have sneaked in...
```
:set list
```
