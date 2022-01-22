# ansible role の書き方

## 基本形

`tasks/main.yml`にインストールコマンドを記述する



```yml
# build-essentialパッケージをインストールする例

- name: install build-essential
  become: yes
  apt:
    name: build-essential
    update_cache: yes
```

```yml
# command lineを実行する例

- name: nvm
  shell: >
    curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.38.0/install.sh | bash
  args:
    creates: "{{ ansible_env.HOME }}/.nvm/nvm.sh"
```