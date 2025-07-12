# Configuration Management

- [Ansible](#ansible-playbooks-roles-inventory)
- [Chef](#chef-recipes-cookbooks)
- [Puppet](#puppet-manifests-modules)
- [Salt](#saltstack-states-grains)

---

## Ansible (playbooks, roles, inventory)

### 1. Ansible Basics

```bash title="Check version"
ansible --version
```
```bash title="Check inventory"
ansible-inventory --list -y
```
```bash title="Ping all hosts"
ansible all -m ping
```
```bash title="Run command on all hosts"
ansible all -a "uptime"
```

### 2. Inventory & Configuration

#### Default inventory:
- `/etc/ansible/hosts`

#### Custom inventory:

- `ansible -i inventory.ini all -m ping`

#### Define hosts in inventory.ini:

```bash title="inventory.ini"
# fmt: ini

[web]

web1 ansible_host=192.168.1.10 ansible_user=ubuntu

[db]

db1 ansible_host=192.168.1.20 ansible_user=root
```

### 3. Ad-Hoc Commands

```bash title="Run as a specific user"
ansible all -m ping -u ubuntu --become
```
```bash title="Copy file to remote host"
ansible all -m copy -a "src=/etc/hosts dest=/tmp/hosts"
```
```bash title="Install a package (example: nginx)"
ansible all -m apt -a "name=nginx state=present" --become
```

### 4. Playbook Structure

```yaml
- name: Install Nginx
  hosts: web
  become: yes

  tasks:
  - name: Install Nginx
    apt:
    name: nginx
    state: present
```

```bash title="Run the playbook:"
ansible-playbook install_nginx.yml
```

### 5. Variables & Facts

```bash title="Define variables in vars.yml:"
nginx_version: latest
```

#### Use variables in playbook:

```yaml
- name: Install Nginx
  apt:
  name: nginx={{ nginx_version }}
  state: present
```

```bash title="Display all facts:"
ansible all -m setup
```

### 6. Handlers & Notifications

```yaml
- name: Restart Nginx
  hosts: web
  become: yes

  tasks:
  - name: Install Nginx
    apt:
      name: nginx
      state: present
    notify: Restart Nginx

  handlers:
  - name: Restart Nginx
    service:
    name: nginx
    state: restarted
```

### 7. Loops & Conditionals

#### Loop over items:

```yaml
- name: Install multiple packages
  apt:
    name: "{{ item }}"
    state: present
  loop:
  - nginx
  - curl
  - git
```

#### Conditional execution:

```yaml
- name: Restart service only if Nginx is installed
  service:
    name: nginx
    state: restarted
  when: ansible_facts['pkg_mgr'] == 'apt'
```

### 8. Roles & Reusability

```bash title="Create a role:"
Ansible-galaxy init my_role
```

#### Run a role in a playbook:

```yaml
- hosts: web
  roles:
  - my_role
```

### 9. Debugging & Testing

#### Debug a variable:

```yaml
- debug:
  msg: "The value of nginx_version is {{ nginx_version }}"
```

```bash title="Check playbook syntax:"
ansible-playbook myplaybook.yml --syntax-check
```

```bash title="Run in dry mode:"
ansible-playbook myplaybook.yml --check
```

## Ansible Playbook

### 1. Playbook Structure

```yaml
- name: Example Playbook
  hosts: all
  become: yes

  tasks:
  - name: Print a message
    debug:
      msg: "Hello, Ansible!"
```

```bash title="Run the playbook:"
ansible-playbook playbook.yml
```

### 2. Defining Hosts & Privilege Escalation

```yaml
- name: Install Nginx
  hosts: web
  become: yes
```

#### Run as a specific user:

```yaml
- name: Install package
  apt:
    name: nginx
  state: present
  become_user: root
```

### 3. Tasks & Modules

```yaml
- name: Ensure Nginx is installed
  hosts: web
  become: yes

  tasks:
  - name: Install Nginx
    apt:
      name: nginx
      state: present
```

#### Common Modules

- `command`: Run shell commands
- `copy`: Copy files
- `service`: Manage services
- `user`: Manage users
- `file`: Set file permissions

### 4. Using Variables

#### Define variables inside the playbook:

```yaml
vars:
  package_name: nginx
```

#### Use them in tasks:

```yaml
- name: Install {{ package_name }}
  apt:
    name: "{{ package_name }}"
    state: present
```

#### Load external variables from vars.yml:

```yaml
- name: Load Variables
  include_vars: vars.yml
```

### 5. Conditionals

```yaml
- name: Restart Nginx only if installed
  service:
    name: nginx
    state: restarted
  when: ansible_facts['pkg_mgr'] == 'apt'
```

### 6. Loops

```yaml
- name: Install multiple packages
  apt:
    name: "{{ item }}"
    state: present
  loop:
  - nginx
  - git
  - curl
```

### 7. Handlers

```yaml
- name: Install Nginx
  apt:
    name: nginx
    state: present
  notify: Restart Nginx

handlers:
  - name: Restart Nginx
    service:
      name: nginx
      state: restarted
```

### 8. Debugging & Testing

```yaml
- name: Debug Variable
  debug:
    msg: "The server is running {{ ansible_distribution }}"
```

```bash title="Check syntax:"
ansible-playbook playbook.yml --syntax-check
```

```bash title="Dry run:"
ansible-playbook playbook.yml --check
```

### 9. Roles(Best Practice)

```bash title="Create a role:"
ansible-galaxy init my_role
```

- Use the role in a playbook:

```yaml
- hosts: web
  roles:
  - my_role
```

## Chef (recipes, cookbooks)

### Basic Concepts

- `Recipe` - Defines a set of resources to configure a system.
- `Cookbook` - A collection of recipes, templates, and attributes.
- `Resource` - Represents system objects (e.g., package, service, file).
- `Node` - A machine managed by Chef.
- `Run List` - Specifies the order in which recipes are applied.
- `Attributes` - Variables used to customize recipes.

### Commands

```bash title="Run Chef on a node"
chef-client
```
```bash title="Create a new cookbook"
knife cookbook create my_cookbook
```
```bash title="List all nodes"
knife node list
```
```bash title="List all roles"
knife role list
```
```bash title="Run Chef in solo mode"
chef-solo -c solo.rb -j run_list.json
```

### Example Recipe

```ruby
package 'nginx' do
  action :install

end

service 'nginx' do
  action [:enable, :start]

end

file '/var/www/html/index.html' do
  content '<h1>Welcome to Chef</h1>' 

end
```

## Puppet (manifests, modules)

### Basic Concepts

- `Manifest` - A file defining resources and configurations (.pp).
- `Module` - A collection of manifests, templates, and files.
- `Class` - A reusable block of Puppet code.
- `Node` - A system managed by Puppet.
- `Fact` - System information collected by Facter.
- `Resource` - The basic unit of configuration (e.g., package, service).

### Commands

```bash title="Apply a local manifest"
puppet apply my_manifest.pp
```
```bash title="Install a module"
puppet module install my_module
```
```bash title="Run Puppet on an agent node"
puppet agent --test
```
```bash title="Check a resource state"
puppet resource service nginx
```

### Example Manifest

```puppet
class nginx {
  package { 'nginx':
  ensure => installed,
}

service { 'nginx': 
  ensure => running, 
  enable => true,
}

file { '/var/www/html/index.html':
  content => '<h1>Welcome to Puppet</h1>', 
  mode => '0644',
  }
}

include nginx
```

---

## SaltStack (states, grains)

### Basic Concepts

- `State` - Defines configurations and how they should be enforced.
- `Grain` - System metadata like OS, CPU, and memory.
- `Pillar` - Secure data storage for variables.
- `Minion` - A node managed by the Salt master.
- `Master` - The central server controlling minions.

### Commands

```bash title="Check connectivity with minions"
salt '*' test.ping
```
```bash title="Install a package on all minions"
salt '*' pkg.install nginx
```
```bash title="Start a service"
salt '*' service.start nginx
```
```bash title="Show all grains for a minion"
salt '*' grains.items
```
```bash title="Apply a state to minions"
salt '*' state.apply webserver
```

### Example State (nginx.sls)

```yaml
nginx:
  pkg.installed: []
  service.running:
  - enable: true

/var/www/html/index.html:
  file.managed:
  - source: salt://webserver/index.html
  - mode: 644
```
