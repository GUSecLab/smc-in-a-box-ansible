# Ansible Recipes for smc-in-a-box
If anything in here is crap, blame [Micah](mailto:micah.sherr@georgetown.edu).

### Table of Contents
- [Ansible Recipes for smc-in-a-box](#ansible-recipes-for-smc-in-a-box)
    - [Table of Contents](#table-of-contents)
  - [Getting started](#getting-started)
    - [Obtaining a list of GCP nodes](#obtaining-a-list-of-gcp-nodes)
    - [Annotating that list](#annotating-that-list)
    - [GCP Preliminaries](#gcp-preliminaries)



## Getting started

### Obtaining a list of GCP nodes

A critical file the list of servers, clients, and output party.  Ansible will need this.

Using the Google Cloud Console (hereafter, "the Console"), you can generate this via:

```bash
gcloud compute instances list --format='csv[no-heading](networkInterfaces[0].accessConfigs[0].natIP,name)'
```

You can get to the console via the GCP web interface.

### Annotating that list

You'll next need to annotate that list a little bit, dividing it into sections.  For the ansible scripts here, the sections **must** bbe called `output`, `server`, and `client`.  An example file is included below:

```
[output]
output0 ansible_host=34.125.18.133

[server]
server0 ansible_host=34.23.63.163 server_id=0
server1 ansible_host=35.197.49.235 server_id=1
server2 ansible_host=34.22.167.188 server_id=2
server3 ansible_host=34.86.188.98 server_id=3
server4 ansible_host=34.42.86.58 server_id=4
server5 ansible_host=35.198.133.63 server_id=5
server6 ansible_host=34.168.114.41 server_id=6

[client]
client0 ansible_host=34.159.45.14 client_id=0
client1 ansible_host=35.199.25.158 client_id=1
client2 ansible_host=34.17.42.120 client_id=2
client3 ansible_host=34.133.134.193 client_id=3
client4 ansible_host=35.235.72.113 client_id=4
client5 ansible_host=34.138.201.163 client_id=5
client6 ansible_host=34.16.165.206 client_id=6
client7 ansible_host=34.155.189.54 client_id=7
client8 ansible_host=35.235.86.12 client_id=8
client9 ansible_host=34.106.174.33 client_id=9
```

Note that I manually added meaningful names on the left and added server and client IDs too.  You should probably make sure that the numbering scheme here matches the instance names from GCP.

The above is also available as [gcp-nodes.txt](/gcp-nodes.txt).

### GCP Preliminaries

Next, you'll need to make sure that you have the [ansible.cfg](/ansible.cfg) file handy.  In a nutshell, this allows you to do ssh key forwarding.  You'll need this because the ansible scripts do a `git clone` of a private repos.

Speaking of, make sure that if you have a key/passphrase for your ssh public key, you do:
```sh
ssh-add /path/to/key/file
```
You can omit `/path/to/key/file` if you use your default key with GCP.

Finally, you need to type:
```bash
export ANSIBLE_CONFIG=/path/to/ansible.cfg
```
using the full path to the [ansible.cfg](/ansible.cfg) file.

