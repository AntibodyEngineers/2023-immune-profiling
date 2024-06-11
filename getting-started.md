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
