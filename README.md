# Ansible Role: GIT

An Ansible Role that managed a local Git installation and additional repositories on a host.

The purpose of this role is

* to manage the configuration of a local git installation.
* to manage the available GIT repositories that have been cloned.
* to manage the local GIT configuration of the cloned repositories.
* to manage the available bare GIT repositories on a host.

## Requirements

* None

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

```yaml
git_config_global: []

# Example
git_config_global:
  - name: init.defaultBranch
    value: 'master'
```

Global config settings for GIT, stored in `~/.gitconfig`. Takes a list of `name`, `value` lists with the settings that should be applied.

```yaml
git_remote_repositories: []

# Example
git_remote_repositories:
  - name: my_git_repository
    repo: git@github.com:johndoe/example.git
    config:
      user.name: DBV
      user.email: github@micronarrativ.org
```

Takes a list of dictionaries to configure remote GIT repositories that will be cloned to the local harddisk. Additionally the repository configuration can be speciefied.

```yaml
git_bare_repositories: []

# Example
git_bare_repositories:
  - dest: /data/bare_repo1
  - dest: /data/bare_repo2
  - dest: /data/bare_repo3
    recurse: true
```

Takes a list of `dest` dicts with the path on where to create a bare repository with GIT. Additionally the parameter `recurse` can be set to re-set also the permissions within existing bare-repositories.

```yaml
git_repository_directory: "{{ lookup('env', 'HOME') }}./git_repos"
```

String setting the location of the cloned GIT repositories shall be stored. The default is `~/git_repos`.

### Additional variables

```yaml
git_package_state: present
```

Setting to define the overall package installation status. Takes `present`, `absent`, `latest`, etc.

```yaml
git_repository_owner: "{{ lookup('env', 'USER') }}"
```

Let's you change the owner of the repositories and files inside the repositories. Defaults to the current user running the Ansible role.

```yaml
git_repository_group: "{{ lookup('env', 'USER') }}"
```

Let's you change the group of the repositories and files inside the repositories. Defaults to the group of the current user running the Ansible role.

```yaml
git_repository_mode: '0750'
```

File mode for accessing the GIT repositories cloned to the local system.

```yaml
git_repository_clone_timeout: 20
```

Timeout in seconds used when cloning remote repositories to the local harddisk. With very big repositories this number might needed to be increated.

## Dependencies

None.

## Example Playbooks

### Creating bare repositories on remote server

Playbook:

```yaml
---
- hosts: all
  vars:
    git_bare_repositories:
      - dest: /git/repo_01
      - dest: /git/repo_02
      - dest: /git/repo_03

  roles:
    - spreadcat.git
```

### Setup local GIT repositories

Playbook:

```yaml
---
- hosts: all
  vars:
    git_remote_repositories:
      - name: repo_01
        repo: git@github.com:johndoe/example.git
        config:
          user.name: John Doe
          user.email: john.doe@example.com

  roles:
    - spreadcat.git
```

## License

MIT / BSD

## Author Information

This role was created a while ago (before 2023) by [DBV](http://micronarrativ.org).
