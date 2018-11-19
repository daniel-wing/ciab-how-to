# ciab-how-to
Learning out load: how to install Guacamole on a Ubuntu server to use as a remote desktop

################################################################################
# Script to install CIBA on an Ubuntu 18.04 server tested on digitalocean.com 
# droplet successfully
# Repo: https://github.com/bmullan/ciab-remote-desktop
#
#                               *The droplet has been destroyed so I kept the IPs
#################################################################################

Open an SSH connection to the server:

  ssh root@104.248.76.233

Create a new non-root user

  adduser danielwing

  It will ask for a password, type one and confirm it
  It will ask for basic data, you can leave it blank and just hit enter or return

Add this user to the sudo group
  gpasswd -a danielwing sudo

Change the user to the new one
  su - danielwing

Update you libraries
  sudo apt-get update
    It wil ask for you password again to give permission

Install zip and unzip libraries:
  sudo apt-get install zip
  sudo apt-get install unzip

Remove all unnecesary packages
  sudo apt autoremove

Create the directory:
   sudo mkdir /opt/ciab

Move to (Open the) folder:
     cd /opt/ciab/

Own the directory:
   sudo chown danielwing:danielwing /opt/ciab/

Download the required files from GitHub (not clone, download)
  wget -P /opt/ciab/ https://github.com/bmullan/ciab-remote-desktop/archive/master.zip

Unzip the Master file
  unzip master.zip -d /opt/ciab/

Move into the extracted folder
  cd /opt/ciab/ciab-remote-desktop-master

Move all files from /opt/ciab/ciab-remote-desktop-master into /opt/ciab/
  mv *.pdf  *.md *.sh *.list /opt/ciab/

Move back to /opt/ciab/
  cd ..

Delete the now empty folder:
  rm -rf ciab-remote-desktop-master

List all files to verify
    ls

All files ending in "sh" are a program, to allow the system to execute those programs we run:
  chmod +x /opt/ciab/*.sh

Now we execute the first program:
  ./setup-ciab.sh

You will be shown and should decided as follows (if no text after the ":" then default ):
Would you like to use LXD clustering? (yes/no) [default=no]:
Do you want to configure a new storage pool? (yes/no) [default=yes]:
Name of the new storage pool [default=default]:
Name of the storage backend to use (btrfs, ceph, dir, lvm, zfs) [default=zfs]: dir
Would you like to connect to a MAAS server? (yes/no) [default=no]:
Would you like to create a new local network bridge? (yes/no) [default=yes]:
What should the new bridge be called? [default=lxdbr0]:
What IPv4 address should be used? (CIDR subnet notation, “auto” or “none”) [default=auto]:
What IPv6 address should be used? (CIDR subnet notation, “auto” or “none”) [default=auto]: none
Would you like LXD to be available over the network? (yes/no) [default=no]:
Would you like stale cached images to be updated automatically? (yes/no) [default=yes]
Would you like a YAML "lxd init" preseed to be printed? (yes/no) [default=no]:

Next step. You will see this:
      Next, please run the script...

               $/opt/ciab/setup-containers.sh



      Press any key to continue...

Press enter, no other key worked for me. Then run the next scrip
  ./setup-containers.sh

            When the listing of LXD/LXC containers appears write down the IP address of ciab-guac and cn1..

            You will need those IP addresses later when you first login to Guacamole and configure
            'connections' to CN1 so users can access them and the Desktop Environments.

           +-----------+---------+-----------------------+------+------------+-----------+
           |   NAME    |  STATE  |         IPV4          | IPV6 |    TYPE    | SNAPSHOTS |
           +-----------+---------+-----------------------+------+------------+-----------+
           | ciab-guac | RUNNING | 10.103.250.16 (eth0)  |      | PERSISTENT |           |
           +-----------+---------+-----------------------+------+------------+-----------+
           | cn1       | RUNNING | 10.103.250.215 (eth0) |      | PERSISTENT |           |
           +-----------+---------+-----------------------+------+------------+-----------+

It will ask for the SQL Master password and a guacamole database password, type one and confirm

Then

      Installation Complete
      https://IP_of_Server/guacamole/
       Default login guacadmin:guacadmin
      Be sure to change the password.

       Guacamole, MySQL and Tomcat Installation is Done!

Press any key to install NGINX...

==========================={ CIAB Remote Desktop Installation complete! }=============================

There you go!
