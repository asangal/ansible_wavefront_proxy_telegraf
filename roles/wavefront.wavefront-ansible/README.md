Wavefront Ansible Role
=========

Ansible Role to deploy the Wavefront Proxy agent or Collector (Telegraf) agent.

Platforms
---------

* Amazon Linux
* CentOS
* RedHat
* Ubuntu

Role Variables
--------------
The following variables are available for override.
```
instance: try (default: mon).                                    # Optional. Instance name
wavefront_api_url: https://mon.wavefront.com (default)           # Optional. WaveFront URL
wavefront_api_token: xx1122ZZ3344111_GIVE_A_VALID_API_T0KEN      # Optional. Your API Key
wavefront_install_proxy=true (default: false)                    # Optional.
wavefront_install_collector=true (default: false)                # Optional.
proxy_address=valid.proxy.hostname (default: a WaveFront Proxy)  # Required, if collector(true)
proxy_port=2878_is_a_valid_port (default: 2878)                  # Required, if collector(true)
wavefront_create_cred_file=true (default: false)                 # Optional.
telegraf_tags                                                    # Optional. Check <roles>/defaults/main.yml for more info.
telegraf_inputs                                                  # Optional. Check <roles>/defaults/main.yml for more info.
```

Role Tags
---------
The following tags are supported, can be given comma separated (for ex: --tags "tag1,tag2").

```
--tags "check" or --tags "failfast"  # Will perform few checks/fail-fast only
--tags "prereqs"                     # Will install only prerequisite packages
--tags "install"                     # Will install on any RedHat/Debian(Ubuntu)
--tags "redhat"                      # Will install on any RedHat/CentOS only.
--tags "debian"                      # Will install on any Debian(Ubuntu) only.
--tags "proxy"                       # Will install only Wavefront Proxy agent.
--tags "collector"                   # Will install only Collector(Telegraf) agent.
--tags "configure"                   # Will install/ensure configuration of agents.
```

How To Install
----------------
Download from: https://github.com/asangal/ansible_wavefront_proxy_telegraf/archive/master.zip

Coming soon:
Using ansible galaxy, best for ad-hoc command situations:

    $ ansible-galaxy install wavefront.wavefront-ansible

To install into your playbook roles, use `-p ROLES_PATH` or `--path=ROLES_PATH`

    $ ansible-galaxy install wavefront.wavefront-ansible -p /your/project/root/roles

Check out: [Advanced Control over Role Requirements Files](http://docs.ansible.com/galaxy.html#advanced-control-over-role-requirements-files)

Examples
----------------
1) To install Wavefront Proxy agent (local machine): Go to the folder where 'wavefront-ansible.yml' file is present, then run:
```
$ ansible-playbook -i inventory -l localhost wavefront-ansible.yml --extra-vars "wavefront_install_proxy=true wavefront_api_token=dummy012-f223-11cf-789c-T0kenOfWFUrl"
```

OR if you know the OS is RedHat/CentOS and you want to install proxy agent on https://try.wavefront.com, then run:
```
$ ansible-playbook -i inventory -l localhost wavefront-ansible.yml --extra-vars "instance=try wavefront_install_proxy=true wavefront_api_token=dummy012-f223-11cf-789c-T0kenOfWFUrl" --tags "redhat"
```

OR if you also want to create the '~/.wavefront/credentials' file and if the OS is Debian/Ubuntu: 
```
$ ansible-playbook -i inventory -l localhost wavefront-ansible.yml --extra-vars "wavefront_create_cred_file=true wavefront_install_proxy=true wavefront_api_token=dummy012-f223-11cf-789c-T0kenOfWFUrl" --tags "debian"
```

2) To install Wavefront Collector agent (Telegraf) on a (local machine). 
```
$ ansible-playbook -i inventory -l localhost wavefront-ansible.yml --extra-vars "wavefront_install_collector=true proxy_address=my-valid-wavefront-proxy-server-host.com proxy_port=2878"
```

OR if you know the OS is Debian/Ubuntu based, then run:
```
$ansible-playbook -i inventory -l localhost wavefront-ansible.yml --extra-vars "wavefront_install_collector=true proxy_address=my-valid-wavefront-proxy-server-host.com proxy_port=2878" --tags "debian"
```

Dependencies
------------
For ex:
If target machines are in AWS, then user should have ~/.aws/credentials file setup with required info.
```
[Credentials]
aws_access_key_id = RBCKHC7KKJD99SSDUMMY
aws_secret_access_key = DummYGG62CCc62+iihhgga6+45Km+tG9+T9WL5Wc

[Boto]
ec2_region_name = us-west-2
ec2_region_endpoint = ec2.us-west-2.amazonaws.com

[default]
region = us-west-2
aws_access_key_id = RBCKHC7KKJD99SSDUMMY
aws_secret_access_key = DummyGG62CCc62+iihhgga6+45Km+tG9+T9WL5Wc
```

MISC Info
---------

Once Wavefront Proxy agent (wavefront-proxy) is installed/configured successfully.
--------------------------------------------

1) You can see its configuration files at: /etc/wavefront/wavefront-proxy/*

2) You can find the log files at: /var/log/wavefront/wavefront-proxy/*


Once Collector(telegraf) agent is installed/configured successfully.
------------------------------

1) You can see its configuration files at: /etc/telegraf/* or /etc/telegraf/telegraf.d/*

2) You can fine the log files at: /var/log/telegraf/*

3) telegraf_tags variable can be passed at cmd line (or set in <role>/defaults/main.yml) 
   This variable if enabled, will add tags under `[global_tags]` section in telegraf.conf file.

4) telegraf_inputs variable can be passed at cmd line (or set in <role>/defaults/main.yml) 
   This variable if enabled, will create respective inputs.<key>.conf files in telegraf.d folder
   for any user defined inputs plugins.


Optional
--------
1) You can create a new Dashboard (for Telegraf) on your WaveFront instance URL using:
   Telegarf Dashboard template JSON file available here: 
   https://raw.githubusercontent.com/wavefrontHQ/integrations/master/telegraf/dashboards/telegraf-host.json

License
-------

Apache 2.0

Author Information
------------------
Wavefront Ops <ops@wavefront.com>
Use github issues for bugs in this repo.
