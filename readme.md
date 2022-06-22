# Ansible Role: Vaultwarden in Docker deployment

This role deploys a Vaultwarden install in a Docker container.

## Variables

### Defaults
Default values for variables are in ```defaults/main.yml```.

### Examples
```
vaultwarden: "true"
vaultwarden_interface_addr: "0.0.0.0"
```

This is the most basic setup. 
The WebUI will be exposed on the hosts IP on port ```80``` and the websocket ```3012```.
This is not a recommended setup. Its highly recommended to have vaultwarden running behind a reverse proxy.

### A few important notes
A default for ```vaultwarden_admin_token``` IS NOT SET. Therefore the admin interface is DISABLED.
Please set a password or secure the admin panel otherwise.
Unless you know what you are doing leave ```vaultwarden_disable_admin_token``` to default.

## Migrating
Setting ```vaultwarden_data``` to the path of your old vaultwarden data (on the machine from where you are running ansible) will trigger the migrate process.

This will basically push your old data over to the target machine.

Please set ```vaultwarden_data``` via ```--extra-vars```. DO NOT HARDCODE IT INTO YOUR HOST VARS.

## Use with Ansible
Include the role like this:
```
- hosts: all
  roles:
    - role: jchroback.vaultwarden
```

## Uninstall Vaultwarden
Setting ```vaultwarden_uninstall``` to ```true``` and ```vaultwarden``` to ```false``` will start the uninstall process.

This process will:
- Remove the container
- Backup your data 
- Remove the volume of your data (only backup will remain)
- Remove vaultwarden linux user
- Remove vaultwarden linux group

## Proxies
This role should work with most reverse proxies in any configuration.

I would recommend using this [role](https://github.com/JCSynthTux/ansible-role-docker-nginx) of mine.
The variables in combination with the NGINX role would look like this:
```
vaultwarden: "true"
vaultwarden_networks:
  - name: nginx_network
vaultwarden_ports: []
vaultwarden_env_extra: {
  "VIRTUAL_PORT": "80"
  "VIRTUAL_HOST": "vault.domain.tld"
}
```
