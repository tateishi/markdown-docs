# dockerのrole

## tasks

tasks/main.yml
```yml
- name: docker_host
  become: yes
  apt:
    name:
      - apt-transport-https
      - ca-certificates
      - software-properties-common
    update_cache: yes

- name: add apt-key
  become: yes
  apt_key:
    url: "{{ docker.key.url }}"
    id: "{{ docker.key.id }}"
    state: present

- name: add repository
  become: yes
  apt_repository:
    repo: "{{ docker.repo }}"

- name: install docker ce
  become: yes
  apt:
    name: docker-ce
    update_cache: yes

- name: add default user to docker group
  become: yes
  user:
    name: "{{ docker_user }}"
    groups: "{{ docker_group }}"
    append: yes

- name: install docker-compose
  become: yes
  get_url:
    url: "{{ docker_compose.url }}"
    dest: "{{ docker_compose.dest }}"
    mode: "{{ docker_compose.mode }}"

- name: install python3-docker
  become: yes
  apt:
    name: python3-docker
    update_cache: yes
```

## default

defaults/main.yml

```yml
docker_user: ubuntu
docker_group: docker
docker:
  key:
    url: https://download.docker.com/linux/ubuntu/gpg
    id: 0EBFCD88
  repo: "deb [arch=amd64] https://download.docker.com/linux/ubuntu {{ ansible_distribution_release|lower }} stable"

docker_compose_ver: 1.29.1
docker_compose:
  url: "https://github.com/docker/compose/releases/download/{{ docker_compose_ver }}/docker-compose-{{ ansible_system}}-{{ ansible_architecture }}"
  dest: /usr/local/bin/docker-compose
  mode: "0755"

docker_universal: "deb http://us.archive.ubuntu.com/ubuntu {{ ansible_distribution_release|lower }} universe"
```