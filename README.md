# How to make a  VirtualBox-based Vagrant Amazon 2 Linux Base Box

*Improvements or suggestions towards this document are welcome.*

* Download latest `.vdi` file. You should be able to find the most recent `.vdi`
file at https://cdn.amazonlinux.com/os-images/latest/virtualbox/

* Build virtual CD that contains cloud-init data (Will generate a file `seed.iso`. Ensure that this is ran from the same
directly which contains the files `user-data` and `meta-data` from this repository.
###### Linux:
```bash
# Navigate into the seedconfig folder and execute the following command:
genisoimage -output seed.iso -volid cidata -joliet -rock user-data meta-data
```
###### Mac:
```bash
# Navigate one level up from the seedconfig folder and execute the following command:
hdiutil makehybrid -o seed.iso -hfs -joliet -iso -default-volume-name cidata seedconfig/
```

### Creating VM in VirtualBox

* Create new VM with the following criteria
  - name: AMZN
  - type: linux
  - version: Other Linux 64bit
* Select the `.vdi` image downloaded earlier as the Hard Disk (option might state
something like "Use an existing virtual hard disk file")
* Add the `seed.iso` file generated above to the virtual CD drive
* Start the Amazon Linux 2 virtual machine

### Do not install VirtualBox Guest Additions
* We will use the vagrant-vbguest plugin to install GuestAdditions as needed

### Run system updates
```bash
sudo yum -y update
```

#### Post processing
```bash
#Delete Bash command history
export HISTSIZE=0

#Delete cache of yum
yum clean all
rm -rf /var/cache/yum

shutdown -h now
```

#### Create Base Box from virtual machine

These commands to be executed on the host machine
```bash
vagrant init
vagrant package --base AMZN --output amazon-linux-2_X.X.X.box
```
The output amazon-linux-2_X.X.X.box is the file of Base Box

#### Notes:
The ssh key in the user-data file is the usual Vagrant insecure keypair from [here](https://github.com/hashicorp/vagrant/tree/master/keys)

#### References:
http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/amazon-linux-2-virtual-machine.html

https://qiita.com/aibax/items/7fd9a874cb7e88f95488

https://superuser.com/questions/1048091/can-i-install-ec2-amazon-linux-os-locally-on-virtual-machine


#### Contributors

[Paul O'Flynn](https://github.com/poflynn) Original Author

[Sam Anthony](https://github.com/expertcoder)
