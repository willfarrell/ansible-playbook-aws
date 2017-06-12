# ansible-playbook-aws

## Requirements
- ansible >= 2.3
- AWS Account w/ IAM access

## Setup
```bash
# 2017-04
brew install python
sudo -H pip install --upgrade ansible
sudo -H pip install --ignore-installed six	# fix bug with boto
sudo -H pip install --ignore-installed python-dateutil	# fix bug with botocore
sudo -H pip install --upgrade botocore boto boto3 passlib
sudo -H pip install --upgrade --user awscli

# bashrc
export PYTHONPATH=$(python -c "from distutils.sysconfig import get_python_lib; print(get_python_lib())")
export PATH=~/Library/Python/2.7/bin:$PATH

# Other deps
# mysql_*
sudo -H pip install --upgrade MySQL-python
```

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
1. Click `Add user`.
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
- [ ] BUG NAT deploys failed
- [ ] Double check route table has working nat and matches
- [ ] BUG DNS 8.8.8.8 not reachable from private subnet
- [ ] Add `delete on termination` to ec2 volumes
- [ ] Encrypted RDS not supported in ansible + boto - https://github.com/boto/boto/pull/3027

## Security
### AWS
- [ ] update access policy (ansible user) https://awspolicygen.s3.amazonaws.com/policygen.html

## TODO
- [ ] docker swarm
- [ ] elastic-cloud ansible
- [ ] jenkins ansible