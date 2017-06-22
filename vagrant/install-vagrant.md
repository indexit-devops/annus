### 1) Install vagrant using following command.
```
# yum install https://releases.hashicorp.com/vagrant/1.9.5/vagrant_1.9.5_x86_64.rpm -y
```

### 2) Check the Vagrant is installed properly or not.
```
# vagrant --version
```

### 3) Install required packages to install vagrant google plugin.
```
# yum install gcc git -y
```

### 4) Install vagrant plugin
```
vagrant plugin install vagrant-google
```

## To create a resources  in google cloud create a service account.
## Then follow the following process.

### 5) Copy the JSON file from windows to linux.
```
# vim /root/provision.pem
<Copy and Paste the JSON file for the service account you created>
```

### 6) Generate SSH-KEYS
```
# ssh-keygen -f /root/provision
# mv /root/provision /root/provision.pem
```

#### Copy provision.pub file to Google Compute Metadata
   #### Note: Change the username from `root` to `provision`
### 7) Create directory structure as follows
```
# cd /root
# mkdir vagrant
# cd vagrant
# mkdir demo1
# cd demo1
```

### 8) Create vagrant configuration .
```
# vim Vagrantfile
Vagrant.configure("2") do |config|

    config.vm.box = "google/gce"
    config.vm.provision "shell", path: "https://raw.githubusercontent.com/indexit-devops/annus/master/init.sh"
    config.vm.provider :google do |google, override|
    google.google_project_id = "mor8am"
    google.google_client_email = "provision@mor8am.iam.gserviceaccount.com" 
    google.google_json_key_location = "/root/provision.json"

    # Make sure to set this to trigger the zone_config
    google.zone = "us-central1-f"
     
     
    override.ssh.username = "provision"
    override.ssh.private_key_path = "/root/provision.pem"
    
    
    google.zone_config "us-central1-f" do |zone1f|
        zone1f.name = "demo1"
        zone1f.image = "centos-7-v20170620"
        zone1f.machine_type = "n1-standard-1"
        zone1f.zone = "us-central1-f"
    end
  end
end
```
