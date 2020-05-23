# Confluent Kafka Lab for Training

Not all participants has the liberty to have good machines, thus using Cloud as an alternative for a traning, in this repository you can deploy the Confluent Kafka training stack that is used in the Kafka Developer trainig.

### Setup your GCP account and Project

First setup your GCP account and user a free tier while learning this is a [link](https://cloud.google.com/free/docs/gcp-free-tier)

Then you need to follow the requisites:

Install GCP modules:

`$ pip install requests google-auth`

Next create a Service account in GCP in a project and a json file:

To create a service account this is a good [link](https://developers.google.com/identity/protocols/oauth2/service-account#creatinganaccount) for it

Then creating a json file for your JSON account this is the [link](https://developers.google.com/identity/protocols/oauth2/service-account#creatinganaccount)

add also a Project wide ssh keys for easier and upload the public key.

for you to create ssh keys you can use this command:

`$ ssh-keygen -t ed25519 -f kafka.key -t ed25519 -q -N ""`

it would create a `kafka.key` (private key) and `kafka.key.pub` (public key)

upload the contents of the public key.

### Configure the ansible playbooks

clone the repository
`$ git clone https://github.com/ajohnsc/k8s4learning.git`

copy your keys inside the directory.
`$ cp gcp.key* k8s4learning/`

go to the directory and edit the following files:
1. ansible.cfg-sample
2. config.yaml-sample
3. credentials.json-sample

#### Ansible.cfg configuration sample file
```$ cat ansible.cfg-sample
[defaults]
remote_user = (your remote user)
host_key_checking = False
private_key_file = (your private key)


[inventory]
enable_plugins = gcp_compute

[privilege_escalation]
become = True
become_method = sudo
become_user = root
become_ask_pass = False
```

`( your remote user)` will be replaced with your user that you use in your Linux Desktop since you created the keys with that user, and `(your private key)` is the path of your private key.

#### config.yaml sample file
```$ cat config.yaml-sample 
---
gcp_project: ( your project ID )
gcp_cred_kind: serviceaccount
gcp_cred_file: credentials.json
region: ( your region )
zone: ( your regions zone )
instance_config:
  - vm_name: "kafka"
    vm_machine_type: "n1-standard-2"
    vm_address_name: "kafka-address"
    vm_group: "kafka"
    disk_name: "kafka-disk"
    disk_size: "20"
```

`(your project ID)` is the id that showsup in the Project tab beside your Project name, and `( your region )` is the Region and `( your regions zone )` options when you create a VM in GCP, check it in the GCP dashboard for your refference.


#### credentials.json file

If you got your credential file from the just rename it to `credentials.json` and copy it to your `kafkalab` directory.

#### Next is run the playbook

`$ ansible-playbook main_create.yaml`

#### Delete the cluster

`$ ansible-playbook main_delete.yaml`
