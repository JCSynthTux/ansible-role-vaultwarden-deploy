# Ansible Role: Vaultwarden in Docker deployment

This role deploys a Vaultwarden install in a Docker container.

## Variables

### Basics

| Variable Name           | Usage                 |
|-------------------------|-----------------------|
| vaultwarden_uninstall   | True to uninstall on host |
| vaultwarden_image_tag   | Specify tag to pull |
| vaultwarden_pull        | Should image be pulled on every run |
| vaultwarden_uid         | UID to use for vaultwarden linux user |
| vaultwarden_gid         | GID to use for vaultwarden linux group | 
| vaultwarden_signup      | Disable or enable signup | 
| vaultwarden_invitations_allowed | Disable or enable signup via invitation |
| vaultwarden_disable_admin_token | Setting this to true will expose the admin panel WITHOUT protection |
| vaultwarden_password_hint | Ability to display password hint created during registration |
| vaultwarden_install_path | Path to install vaultwarden to |
| vaultwarden_restart_policy | Container restart policy | 
| vaultwarden_admin_token | Password used for admin authentication
| vaultwarden_data | Path of your old vaultwarden data you want to migrate over |
| vaultwarden_domain | Domain which should be used for vaultwarden. See the [Proxies](https://github.com/JCSynthTux/ansible-role-vaultwarden-deploy#proxies) section |
| vaultwarden_container_name | Name of vaultwarden container |
| vaultwarden_networks | List of networks to attach to container |

### Defaults
Defaults for most Variables are in ```defaults/main.yml```. 

For ```vaultwarden_networks``` a list like this is expected:
```
vaultwarden_networks:
  - name: proxy_network
  - name: nginx_network
  - name: internal_network
```

### A few important notes
A default for ```vaultwarden_admin_token``` IS NOT SET. Therefore the admin interface is DISABLED.
Please set a password or secure the admin panel otherwise. 
Unless you know what you doing leave ```vaultwarden_disable_admin_token``` to default.

## Migrating
Setting ```vaultwarden_data``` to the path of your old vaultwarden data (on the machine from where you are running ansible) will trigger the migrate process.

This will basically push your old data over to the target machine.

Please set ```vaultwarden_data``` via ```--extra-vars```. DO NOT HARDCODE IT INTO YOUR HOST VARS.

## Use with Ansible
I would recommend including the role like this:
```
- hosts: all
  roles:
    - role: jchroback.vaultwarden
      when: vaultwarden_install
```
This should prevent unwanted deployments on systems.

Based on this [Issue](https://github.com/geerlingguy/ansible-role-docker/pull/17#discussion_r134054221).
## Uninstall Vaultwarden
Setting ```vaultwarden_uninstall``` to ```true``` will start the uninstall process.

This process will:
- Remove the container
- Backup your data 
- Remove the volume of your data (only backup will remain)
- Remove vaultwarden linux user
- Remove vaultwarden linux group

## Proxies
This container is meant to be attached to a docker network of your proxy. 

The container listens on port ```80``` , but does not export anyport to wider network. Port ```80``` is basically only exposed on the localhost of the container.

I would recommend using this [role](https://github.com/JCSynthTux/ansible-role-docker-nginx) of mine. But I guess you could use any reverse proxy as long as the vaultwarden and the proxy can communicate. 
