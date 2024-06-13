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

**Software:** 
1. [Python](Python)
2. Jupyterlab
3. igBLAST
4. Datascience packages

### Python
Python is 3.10.12, which is OK, latest greatest (06/12/2024) is 3.12.4
Update Python and pip via (https://www.howtogeek.com/install-latest-python-version-on-ubuntu/)
```
sudo apt update
sudo add-apt-repository ppa:deadsnakes/ppa
sudo apt install python3.12
sudo update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.12 1 # sets the defualt to 3.12
```
### Jupyterlab
Want JupyterHub for multiuser system (https://jupyterhub.readthedocs.io/en/stable/tutorial/quickstart.html)

#### Better version
(https://jupyterhub.readthedocs.io/en/1.2.0/installation-guide-hard.html) - gives the appropriate steps and configurations, and is what I did before. 
```
sudo python3 -m venv /opt/jupyterhub/ # returned an error: Command '['/opt/jupyterhub/bin/python3', '-m', 'ensurepip', '--upgrade', '--default-pip']' returned non-zero exit status 1.
sudo apt install python3.12-venv # fixes (https://stackoverflow.com/questions/69594088/error-when-creating-venv-error-command-im-ensurepip-upgrade-def)

sudo python3 -m venv /opt/jupyterhub/ # rerun
sudo /opt/jupyterhub/bin/python3 -m pip install wheel
sudo /opt/jupyterhub/bin/python3 -m pip install jupyterhub jupyterlab
sudo /opt/jupyterhub/bin/python3 -m pip install ipywidgets
sudo apt install nodejs npm
sudo npm install -g configurable-http-proxy
sudo mkdir -p /opt/jupyterhub/etc/jupyterhub/
cd /opt/jupyterhub/etc/jupyterhub/
sudo /opt/jupyterhub/bin/jupyterhub --generate-config
sudo vim jupyterhub_config.py
   # uncomment and add /lab to c.Spawner.default_url = '/lab'
   # uncoment and add True to c.Authenticator.allow_all = True (see note)

sudo mkdir -p /opt/jupyterhub/etc/systemd
sudo vim /opt/jupyterhub/etc/systemd/jupyterhub.service
   # add the lines (jupyterhub.service, below)

sudo ln -s /opt/jupyterhub/etc/systemd/jupyterhub.service /etc/systemd/system/jupyterhub.service
sudo systemctl daemon-reload
sudo systemctl enable jupyterhub.service
sudo systemctl start jupyterhub.service 
```
jupyterhub.servie file:
```
[Unit]
Description=JupyterHub
After=syslog.target network.target

[Service]
User=root
Environment="PATH=/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/opt/jupyterhub/bin"
ExecStart=/opt/jupyterhub/bin/jupyterhub -f /opt/jupyterhub/etc/jupyterhub/jupyterhub_config.py

[Install]
WantedBy=multi-user.target
```
Note: At first could not log in needed to uncomment and set: c.Authenticator.allow_all = True

#### first attempt, replaced with Better version (above)
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
Installs JupyterHub, but this, and the node stuff, are in my home dir, not desired, see above. When the desired install was complete, I deleted the jupyter and .node-dirs (.nvm, .npm)

### igBLAST
For immune profiling projects starting with raw sequence data, igBLAST is used for the aligning and annotating ig sequences to reference data. Installations steps
1. Get the latest prebuilt binaries @ https://ftp.ncbi.nih.gov/blast/executables/igblast/release/LATEST/ 
2. Add to the jetstream base instance. Use upload in jupyter notebook to load into home dir, unpack and prepare for others to run:
```
tar -xf ncbi-igblast-VERSION-x64-linux.tar.gz # do this in homedir so owner is the admin
sudo mv ncbi-igblast-VERSION /usr/local/ # move the dir to a central place
sudo cp ncbi-igblast-VERSION/bin/* /usr/local/bin
sudo mv ncbi-igblast-VERSION igblast # simplifies things (see below)
cd; igblastn -h # test, should get usage 
```
3. Prepare the reference database (TBD, see https://digitalworldbiology.com/blog/how-bioinformatician-odysseus)
4. Set enviorment variables - igBLAST gotchas
add c.Spawner.environment = {'IGDATA': '/usr/local/igblast'} to /opt/jupyterhub/etc/jupyterhub/jupyter_config.py:
```
sudo systemctl restart jupyterhub.service # load the updated file
```
Notes:
- A common error is "Germline annotation database human/human_V could not be found in [internal_data] directory." This results from IGDATA not being set correctly. In the past I had IGDATA='/usr/local/igblast/bin' and moved internal_data and optional_file into the bin dir, which seems odd. This time, after encountering the error, again, I set IGDATA='/usr/local/igblast' and kept internal_data and optional_file in place. Works fine.
- igblast dbs are in /usr/local/igblast/database, but they can be anywhere as long as their path is specified.  

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
Test install pip install pandas - failed do to permission errors. Fix with:
```
cd /opt/jupyterhub/lib/python3.12
sudo chmod -R a+w site-packages/
```
Fixes, -R needed to make site packages fully writable. While the above has potential security issues, it should be OK for hackathon work in a virtual instance. In this way all team members can install packages as they work. 



