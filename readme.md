# Ansible Role: Vaultwarden in Docker deployment

This role deploys a Vaultwarden install in a Docker container.

## Variables

The Vaultwarden deployment requires some variables, while other are optional.
#### Required
```
vaultwarden_domain: your.domain.tld #Domain via vaultwarden should be reachable
vaultwarden_admin_token: "admintoken1234!" #Authentication for admin panel
```
#### Optional
```
vaultwarden_signup: "true" #Defaults to false
vaultwarden_password_hint: "true" #Defaults to false
vaultwarden_disable_admin_token: "false" #Defaults to false
vaultwarden_nginx_network: "my_nginx_network" #Defaults to nginx_network
```
```vaultwarden_nginx_network``` is needed since this role expects an nginx to be running. For this you could use my [nginx role](https://github.com/JCSynthTux/ansible-role-docker-nginx). 

There is also ```vaultwarden_data:/path/to/vw/data```. This is only used for migrating to this playbook. Dont put this hardcoded into your ```vars.yml``` file, instead use this with ```--extra-vars='vaultwarden_data=/path/to/vw/data/'``` when running the playbook. 
The path should be on the same system from where you are running the playbook. This will basically copy every file and folder of your existing vaultwarden volume the new one.

