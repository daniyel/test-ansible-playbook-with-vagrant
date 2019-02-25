This setup is used to test and run playbooks locally on a virtual machine set up with vagrant.
---

## Prerequisites
- VirtualBox
- Vagrant
- ansible

## Installation Guide
- [OSX](#installing-on-osx)
- Windows
- Linux

### Installing on OSX

1. Install VirtualBox

```
$ brew cask install virtualbox
```

Notice: if you get an error while installing you need to accept `Allow` in `System Preferences > Security & Privacy > General`. You can also install VirtualBox manually from https://www.virtualbox.org/wiki/Downloads. More infos: https://apple.stackexchange.com/a/301305.

2. Install vagrant
```
$ brew cask install vagrant
```

3. Run vagrant

```
$ vagrant up
```

4. After the vm is successfully up and running, you can create a snapshot. Snapshot comes in handy, if you wish to run mulitple tests, one after another, on a clean installed machine.

```
$ vagrant snapshot push
```

restore the snapshot

```
$ vagrant snapshot pop
```

## Run playbooks

To run the playbook from your machine, you can such execute a command as an example command below.

```
$ ansible-playbook -v --vault-password-file ~/.ssh/vault_password_file -i inventories/local/hosts --limit "myhosts" play-mystack.yaml
```

Explanation:
- `-v` - display verbose output
- `--vault-password-file` - file with password, used to decrypt encrypted vault sensitive data (you need to have `vault_password_file` on your local machine under `~/.ssh` folder)
- `-i` - path to the inventories, for testing purpose we use `inventories/local/hosts`
- `--limit` - used to limit to which hosts the playbook will be applied

Important: You need `ansible.pem` private key and `config` file under `~/.ssh` folder. `config` should contain
```
Host 127.0.0.1
    User ubuntu
    IdentityFile ~/.ssh/ansible.pem
```

## Updating Vagrantfile

If you updated Vagrantfile and want to apply changes execute

```
$ vagrant reload
```

## Checking changes on vm locally

If you want to see, if changes were applied to the local vm machine or you want to prove, that everything went well, you can ssh to the vm with

```
$ ssh -v -i ~/.ssh/ansible.pem ubuntu@127.0.0.1 -p 2222
```
