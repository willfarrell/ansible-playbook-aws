# ansible-playbook-aws

## Requirements
- ansible >= 2.3 (`pip install git+git://github.com/ansible/ansible.git@stable-2.3`)
- AWS Account w/ IAM access

## Setup

### Set org_id
Keep it lowercase.
- `./run`
- `./playbook.yml`

### IAM Policies
These step will allow you to create the necessary policies for all required ansible commands.
Repeat each for all files in `docs/aws_policies`.

1. Click [Policies](https://console.aws.amazon.com/iam/home#/policies) 
1. Click `Create Policy` 
1. Find `Create Your Own Policy`
1. Click `Select`.
1. Field `Policy Name`: Enter something like `ansible_{{file_name}}`.
1. Field `Policy Document`: Paste contents of `{{file_name}}` into field.
1. Click `Create Policy`.

### IAM Group

1. Click [Groups](https://console.aws.amazon.com/iam/home#/groups) 
1. Click `Create New Group`.
1. Enter `ansible`.
1. Click `Next Step`.
1. Select all `ansible_*` policies (created above).
1. Click `Next Step`.
1. Click `Create Group`.

Also attach `AdministratorAccess`.

### IAM User

1. Click [Users](https://console.aws.amazon.com/iam/home#/users)
1. Field `User name`: Enter `ansible`.
1. Check `Programmatic access`.
1. Click `Next: Permissions`.
1. Select group `ansible` (created above).
1. Click `Next: Review`.
1. Click `Create user`.
1. Save `Access key ID` and `Secret access key` to localhost.
1. Click `Close`.

### Setup secrets
1. Create `~/.vault_password_{{ org_id }}` with the contents being a long random password.

2. Create `group_vars/all/secrets.yml`.

```yml
---

## AWS ##
# IAM Access key
aws_access_key: ''
aws_secret_key: ''

# RDS
db_password: ''
```

3. Encrypt secrets. `ansible-vault encrypt group_vars/all/secrets.yml --vault-password-file ~/.vault_password`

## Run
`./run`

## 1. AWS VPC
- [x] Setup localhost AWS profile
- [x] Scaffold VPC networking
- [x] Setup AWS private ssh key

### TODO
- [ ] Enable IPv6

## 2. Bastion Host
- [x] Deploy EC2 instance
- [x] Setup bastion host
- [x] Setup Security Groups (SSH)
- [x] role to add public keys to servers
- [-] Docs for google-authenticator
- [-] Docs for multi-plexing through bastion and setting up OTP

## 3. Servers
- [x] Security groups
- [x] deploy Web Server + LB
- [x] deploy DB
- [x] harden Web Server
- [x] docker Web Server
- [ ] create users & tables DB

## Security
### AWS
- [ ] update access policy (ansible user) https://awspolicygen.s3.amazonaws.com/policygen.html

### docker
- [ ] 2.6 & 3.{7-14} - TLS
- [ ] 2.8  - Enable user namespace support
- [ ] 2.11 - Use authorization plugin - https://github.com/twistlock/authz
- [ ] 2.12 - Configure centralized and remote logging

### Testing
```bash
#git clone -b configuration_file_args https://github.com/konstruktoid/docker-bench-security.git
git clone https://github.com/docker/docker-bench-security.git
cd docker-bench-security
sh docker-bench-security.sh
```

## TODO
- [ ] docker swarm
- [ ] elastic-cloud ansible
- [ ] jenkins ansible