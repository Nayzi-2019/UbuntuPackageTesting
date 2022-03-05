# UbuntuPackageTesting

## Get source for a package
```shell
sudo nano /etc/apt/sources.list
#uncomment the ones with deb-src and save the file
apt update
#update the changes for sources.list
apt source hello
#get source file for hello
```
You'll see new folder and packages in the dir, cd into it
```
cd hello-2.10
#cd into the the folder of the package
```

## Add files to the project to echo infos when install

add/edit the postinst file, this script will execute after installation
```bash
nano debian/postinst
```
add/edit content for the postinst file in debian folder
```shell
#!/bin/bash
echo "this is a test with postinst from Jian"
/usr/bin/testing.sh
```
save the file and quit nano

add testing script to project
```bash
nano testing.sh
```
add contents to testing.sh
```shell
#!/bin/bash
echo "this is a test from Jian"
```
save the file

add debian/install file, this will help to put testing.sh to the folder while install
```shell
echo "testing.sh usr/bin/" >> debian/install
#this will help to put testing.sh to the folder when installing package
```
## Build the package
```shell
chmod 755 testing.sh
chmod 755 debian/postinst
#give permission to files
dpkg-buildpackage -rfakeroot -b -uc -us
cd ..
#you should see the new deb and .change file
sudo dpkg -i hello_2.10-1build3_amd64.deb
#to install the package using dpkg
dpkg -S usr/bin/testing.sh; testing.sh
#you'll see the testing.sh located in usr/bin and excute it
```

## Upload to PPA
```shell
## the following operations will help you to upload the package to PPA
debsign -k B17F23AD40DBA0CB2B0C502D9D6D8E87343BA25D hello_2.10-1build3_amd64.changes
#sign the changes, where B17F23AD40DBA0CB2B0C502D9D6D8E87343BA25D  is my gpg key
dput ppa:<lp-id>/ppa hello_2.10-1build3_amd64.changes
#to push the package to your ppa
sudo add-apt-repository ppa:<lp-id>/ppa
#to put ppa to the apt list so you can get the package using apt
```
