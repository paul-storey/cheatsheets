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
Use `--volumes` to remove associated anonymous volumes
