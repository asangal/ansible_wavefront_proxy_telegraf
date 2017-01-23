# ansible_wavefront_proxy_telegraf
==================================

Steps:
1. Download/Clone this repository
2. To install Wavefront Proxy and Collector(Telegraf) agents, run the following or similar:

Examples:
---------

To install Wavefront Proxy agent
```
ansible-playbook wavefront-ansible.yml -i "`hostname`," --connection=local --extra-vars "wavefront_install_proxy=true wavefront_api_token=dummy012-f223-11cf-789c-T0kenOfWFUrl"
```

To install Wavefront preferred Collector (Telegraf) agent
```
ansible-playbook wavefront-ansible.yml -i "`hostname`," --connection=local --extra-vars "wavefront_install_collector=true proxy_address=my-valid-wavefront-proxy-server-host.com proxy_port=2878"
```

3. For detailed info about role/playbook and its variables, look inside role 'wavefront.wavefront-ansible' and it's README.md file
