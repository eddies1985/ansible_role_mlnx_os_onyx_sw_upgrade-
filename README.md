Role Name
=========



Requirements
------------
ansible modules:

onyx_config
onyx_command
python module scp - pip install scp


Role Variables
--------------

A description of the settable variables for this role should go here, including any variables that are in defaults/main.yml, vars/main.yml, and any variables that can/should be set via parameters to the role. Any variables that are read from other roles and/or the global scope (ie. hostvars, group vars, etc.) should be mentioned here as well.



force_upgrade_downgrade: If we want to allow a downgrade situation where we may end up with a factory configuration if false then the local ansible server will use the net_put ansible module.  default - False
fetch:                   The switch will copy the image from the server using the "image fetch" command.  default - True
transfer_protocol:       The protocol to use when copying the image to the switch. default - scp
server_ip:               The server ip address from which to fetch the image when using the image fetch command
server_user:             The server username. default - ""
server_password:         The server user password. default - ""
img_file:                The full path to the img file on the remote server. default - ""
fail_on_file:            Fail the role if the img file path doesn't exist  .default - True
destination_folder:      when not using fetch - the img file will be copied to this folder . default - /var/opt/tms/images/
reload:                  If to reload the switch to load the new software image. default - True
e_config_backup: true    if to backup the switch configuration before the reload to the new image. default - true
config_backup: "{{ e_config_backup  | bool }}"

Dependencies
------------

A list of other roles hosted on Galaxy should go here, plus any details in regards to parameters that may need to be set for other roles, or variables that are used from other roles.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

---
- name: EddieS_Playbook
  hosts: ONYX
  gather_facts: no
  strategy: free
  connection: network_cli
  become: yes
  become_method: enable
  vars:
    ansible_network_os: onyx
    force_upgrade_downgrade: False
    fetch: True
    transfer_protocol: scp
    server_ip: 10.10.10.10
    server_user: ""
    server_password: ""
    img_file: "/tmp/image-X86_64-3.9.2006.img"
    fail_on_file: True
    destination_folder: /var/opt/tms/images/
    reload: True
    e_config_backup: true
    config_backup: "{{ e_config_backup  | bool }}"
  remote_user: admin
  roles:
    - { role: mlnx_os_onyx_sw_upgrade , tags: ['onyx_upgrade']}



An example command lines:

ansible-playbook onyx_upgrade_test.yml -l l-csi-2410a1-tmp-17 -e '{"fetch":"false","force_upgrade_downgrade":"false","e_config_backup":"1"}'  -e "img_file=/tmp/image-X86_64-3.9.2006.img" -i hosts
ansible-playbook onyx_upgrade_test.yml -l l-csi-2410a1-tmp-17 -e '{"fetch":"true","force_upgrade_downgrade":"false","e_config_backup":"1"}'  -e "img_file=/tmp/image-X86_64-3.9.2006.img server_ip=192.168.10.10 server_user=root server_password=mypassword" -i hosts

License
-------

BSD

Author Information
------------------

Eddie Shklaer
eddies@nvidia.com
eddie.sshklaer@gmail.com
