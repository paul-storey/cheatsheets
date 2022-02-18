# tmux
- `Ctrl-B %` for a vertical split (one shell on the left, one shell on the right)
- `Ctrl-B"` for a horizontal split (one shell at the top, one shell at the bottom)
- `Ctrl-B o` to make the **o**ther shell active
- `Ctrl-B ?` for help
- `Ctrl-B d` detach from Tmux, leaving it running in the background (use tmux attach to reenter)

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
# vi
## copy characters
With the cursor on the first character, `v` then move to select text. `y` to yank.
## turn off highlighting from last search
```
:noh
```
## Tabs
```
set tabstop=4 shiftwidth=4 expandtab
```
# git
## Delete a local branch
```
git branch -d name-of-branch
```
## Delete remote branch
```
git push origin --delete new-branch
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
## Push a (new) local branch to the remote
The following command creates a remote branch and creates an association between the local and the remote branch. This means that one can subsequently simply run 'git push' or 'git pull' without having to care about specifying branch)
```
git push -u origin new-branch
```
# docker
## Bring services down and removal all trace of them
```
docker-compose down --volumes --remove-orphans
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
docker --volumes rm $(docker ps --filter status=exited -q)
```
Use `--volumes` to remove associated anonymous volumes.

Alternatively
```
docker system prune
```
