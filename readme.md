# Ansible Role: Vaultwarden in Docker deployment

This role deploys a Vaultwarden install in a Docker container.

## Variables

### Basics

| Variable Name           | Usage                 |
|-------------------------|-----------------------|
| vaultwarden_install     | True to install on host |
| vaultwarden_uninstall   | True to uninstall on host |
| vaultwarden_image_tag   | Specify tag to pull |
| vaultwarden_pull        | Should image be pulled on every run |
| vaultwarden_uid         | UID to use for vaultwarden linux user |
| vaultwarden_gid         | GID to use for vaultwarden linux group | 
| vaultwarden_nginx_network | Docker network of reverse proxy | 
| vaultwarden_signup      | Disable or enable signup | 
| vaultwarden_invitations_allowed | Disable or enable signup via invitation |
| vaultwarden_disable_admin_token | Setting this to true will expose the admin panel WITHOUT protection |
| vaultwarden_password_hint | Ability to display password hint created during registration |
| vaultwarden_install_path | Path to install vaultwarden to |
| vaultwarden_restart_policy | Container restart policy | 
| vaultwarden_admin_token | Password used for admin authentication

### Defaults
  Defaults for most Variables are in ```defaults/main.yml```. 

### A few important notes
To deploy vaultwarden set ```vaultwarden_install``` to ```true``` and ```vaultwarden_uninstall``` to false (default).

To uninstall vaultwarden set ```vaultwarden_install``` to ```false``` (default) and ```vaultwarden_uninstall``` to ```true```.

A default for ```vaultwarden_admin_token``` IS NOT SET. Therefore the admin interface is DISABLED.
Please set a password or secure the admin panel otherwise. 
Unless you know what you doing leave ```vaultwarden_disable_admin_token``` to default.

