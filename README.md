# README

## Requirements
- ansible >= 2.3
- AWS Account w/ IAM access

## Setup

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

TODO
- [ ] decrease permissions of ansible user - https://awspolicygen.s3.amazonaws.com/policygen.html

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

## ansible
- add keys to `vars/localhost/aws.yml`.

### ansible vault
- [ ] add keys to ansible vault

## 1. AWS VPC
- [x] Setup localhost AWS profile
- [x] Scaffold VPC networking
- [x] Setup AWS private ssh key

### TODO
- [ ] Enable IPv6

Ref: https://awspolicygen.s3.amazonaws.com/policygen.html

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
- [-] docker Web Server
- [ ] create tables DB


### TODO
- [-] server setup w/ CIS & docker
- [ ] update access policy (ansible user)
- [ ] elastic-cloud script
- [ ] jenkins script