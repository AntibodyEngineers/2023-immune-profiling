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

Python is current 3.10.12
### Jupyterlab
Want JupyterHub for multiuser system (https://jupyterhub.readthedocs.io/en/stable/tutorial/quickstart.html)

First need Node.js and npm. 
```
sudo apt-get install nodejs npm : Gives a buch of 404 errors
sudo apt install nodejs --fix-missing : works
sudo apt install npm --fix-missing : does not work, lots of 404 errors
sudo apt remove nodejs : removes the apt installed package
```
Instead installed NVM (https://github.com/nvm-sh/nvm?tab=readme-ov-file#installing-and-updating)
'''
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm
'''
NVM installed, next 
'''
nvm install node
'''
Installs node and npm 
'''
python3 -m pip install jupyterhub
npm install -g configurable-http-proxy
python3 -m pip install jupyterlab notebook  # needed if running the notebook servers in the same environment
''' 
Installs JupyterHub


