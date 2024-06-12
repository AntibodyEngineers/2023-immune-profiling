Basic commands and websites for building a Jetstream instance from sctratch
# Create an instance
1. In the allocation overview, select Create > Instance from the Create Menu
2. Select an OS Ubuntu / RedHat
3. Select a Flavor (cpus, ram, root disK). Override root disk if desired
4. The instance can be used to create an image that can be a base for addtional instances
5. On the Instance Page, Allocation Name > Instances > Instance Name select Image from the Actions menu
   
Ubuntu 22 (defualt) has python 3.10.22 and pip 22.0.2, apt-get is also installed. 
# Configure the instance
## Add Users
1. Login via the exouser account - ssh id and password are on the instance page
2. Use the sudo adduser commands (see: https://linuxize.com/post/how-to-create-users-in-linux-using-the-useradd-command/) to add the admin user
3. Add that user to the sudoers list (see: https://linuxize.com/post/how-to-add-user-to-sudoers-in-ubuntu)
## Core software
Goal is to have system wide packages that can be used from python command lines, the python enviroment, and jupyter notebooks. Administators (tech leads and sudoers) can install packages for other in hackathons. This way the disk does not fill with packages in individual user accounts. 

Software: 
1. Python
2. Jupyterlab
3. Datascience packages

Python is 3.10.12, which is OK, latest greatest (06/12/2024) is 3.12.4
Update Python and pip via (https://www.howtogeek.com/install-latest-python-version-on-ubuntu/)
1. sudo apt update
2. sudo add-apt-repository ppa:deadsnakes/ppa
3. sudo apt install python3.12
4. sudo update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.12 1 # sets the defualt to 3.12

### Jupyterlab
Want JupyterHub for multiuser system (https://jupyterhub.readthedocs.io/en/stable/tutorial/quickstart.html)

#### Better version
(https://jupyterhub.readthedocs.io/en/1.2.0/installation-guide-hard.html) - gives the appropriate steps and configurations, and is what I did before. 
1. sudo python3 -m venv /opt/jupyterhub/ # returned an error Command '['/opt/jupyterhub/bin/python3', '-m', 'ensurepip', '--upgrade', '--default-pip']' returned non-zero exit status 1.
2. sudo apt install python3.12-venv # fixes (https://stackoverflow.com/questions/69594088/error-when-creating-venv-error-command-im-ensurepip-upgrade-def)
3. sudo /opt/jupyterhub/bin/python3 -m pip install wheel
4. sudo /opt/jupyterhub/bin/python3 -m pip install jupyterhub jupyterlab
5. sudo /opt/jupyterhub/bin/python3 -m pip install ipywidgets
6. sudo apt install nodejs npm
7. sudo npm install -g configurable-http-proxy
8. sudo mkdir -p /opt/jupyterhub/etc/jupyterhub/
9. cd /opt/jupyterhub/etc/jupyterhub/
10. sudo /opt/jupyterhub/bin/jupyterhub --generate-config
11. sudo vim jupyterhub_config.py
12. uncomment and add /lab to c.Spawner.default_url = '/lab'
13. sudo mkdir -p /opt/jupyterhub/etc/systemd
14. sudo vim /opt/jupyterhub/etc/systemd/jupyterhub.service
15. sudo ln -s /opt/jupyterhub/etc/systemd/jupyterhub.service /etc/systemd/system/jupyterhub.service
16. sudo systemctl daemon-reload
17. sudo systemctl enable jupyterhub.service
18. sudo systemctl start jupyterhub.service 
At first could not log in needed to uncomment and set:
c.Authenticator.allow_all = True
#### first attempt, replaced with above
First need Node.js and npm 
Need to start with a sudo apt update?
```
sudo apt-get install nodejs npm # Gives a buch of 404 errors, this was fixed with sudo apt update
sudo apt install nodejs --fix-missing # works, and see above
sudo apt install npm --fix-missing # does not work, lots of 404 errors, fixed with above
sudo apt remove nodejs : removes the apt installed package
```
Instead installed NVM (https://github.com/nvm-sh/nvm?tab=readme-ov-file#installing-and-updating)
```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm
```
NVM installed, next 
```
nvm install node
```
Installs node and npm 
```
python3 -m pip install jupyterhub
npm install -g configurable-http-proxy
python3 -m pip install jupyterlab notebook  # needed if running the notebook servers in the same environment
``` 
Installs JupyterHub, but this is in my home dir, not desired, see above. 
### Datascience packages
Log into JupterLab - via URL:8000, start a notebook. 

Test via import sys and pip. Pip returns
```
Usage:   
  /opt/jupyterhub/bin/python3 -m pip <command> [options]

Commands:
  install                     Install packages.
  download                    Download packages.
  uninstall                   Uninstall packages.
...
```
Test install pip install pandas - failed do to permission errors
```
cd /opt/jupyterhub/lib/python3.12
sudo chmod -R a+w site-packages/
```
Fixes, -R needed to make site packages fully writable. 



