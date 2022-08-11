# HOMEWORK_ANSIBLE_ROLES_8_3_(VITKIN_K_N)



### 1. Подготовка к выполнению.
#### Создаём три пустых публичных репозитория в своём проекте: elastic-role и kibana-role.

- *[kibana-role репозиторий](https://github.com/VitkinKN/kibana-role)*
- *[elastic - role репозиторий](https://github.com/VitkinKN/elastic-role)*
- *[playbook репозиторий](https://github.com/VitkinKN/playbook)*

#### Установливаем molecule:.
```
konstantin@konstantin-forever:~$ pip3 install molecule
Defaulting to user installation because normal site-packages is not writeable
Collecting molecule
  Downloading molecule-4.0.1-py3-none-any.whl (248 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 248.1/248.1 KB 806.9 kB/s eta 0:00:00
Requirement already satisfied: packaging in /usr/lib/python3/dist-packages (from molecule) (21.3)
Collecting cookiecutter>=1.7.3
...
```
#### *Скачаем дистрибутив kibana и elasic и положим их в директорию playbook/files*
#### *elasticsearch-7.10.1-linux-x86_64.tar.gz и kibana-7.10.1-linux-x86_64.tar.gz*
- *Публичную часть своего ключа к своему профилю в github добавленна*
___
### 2. Выполнение.
#### *Создадим в старой версии playbook файл requirements.yml и заполним его следующим содержимым:*
```yaml
---
  - src: github.com/VitkinKN/elastic-role.git
    scm: git
    version: "master"
    name: elastic-role
```
#### *При помощи ansible-galaxy скачаem себе эту роль.*
```
konstantin@konstantin-forever:~/DEVOPS_COURSE/ANSIBLE_ROLE/playbook$ ansible-galaxy install -r requirements.yml --roles-path ./roles
Starting galaxy role install process
- extracting elastic-role to /home/konstantin/DEVOPS_COURSE/ANSIBLE_ROLE/playbook/roles/elastic-role
- elastic-role (master) was installed successfully

```
___
#### *Запустим molecule test, посмотрим на вывод команды*
```
konstantin@konstantin-forever:~/DEVOPS_COURSE/ANSIBLE_ROLE/playbook$ molecule test
INFO     default scenario test matrix: dependency, lint, cleanup, destroy, syntax, create, prepare, converge, idempotence, side_effect, verify, cleanup, destroy
INFO     Performing prerun with role_name_check=0...
INFO     Running ansible-galaxy role install -vr requirements.yml --roles-path /home/konstantin/.cache/ansible-compat/2f2cc2/roles
INFO     Set ANSIBLE_LIBRARY=/home/konstantin/.cache/ansible-compat/2f2cc2/modules:/home/konstantin/.ansible/plugins/modules:/usr/share/ansible/plugins/modules
INFO     Set ANSIBLE_COLLECTIONS_PATH=/home/konstantin/.cache/ansible-compat/2f2cc2/collections:/home/konstantin/.ansible/collections:/usr/share/ansible/collections
INFO     Set ANSIBLE_ROLES_PATH=/home/konstantin/.cache/ansible-compat/2f2cc2/roles:roles:/home/konstantin/.ansible/roles:/usr/share/ansible/roles:/etc/ansible/roles
INFO     Running default > dependency
INFO     Running ansible-galaxy collection install -v --force --pre community.docker:>=3.0.0-a2
WARNING  Skipping, missing the requirements file.
WARNING  Skipping, missing the requirements file.
INFO     Running default > lint
INFO     Lint is disabled.
INFO     Running default > cleanup
WARNING  Skipping, cleanup playbook not configured.
INFO     Running default > destroy
INFO     Sanity checks: 'docker'
[WARNING]: Collection community.docker does not support Ansible version 2.10.8
PLAY [Destroy] *****************************************************************
TASK [Destroy molecule instance(s)] ********************************************
changed: [localhost] => (item=instance)
TASK [Wait for instance(s) deletion to complete] *******************************
FAILED - RETRYING: Wait for instance(s) deletion to complete (300 retries left).
ok: [localhost] => (item=instance)
TASK [Delete docker networks(s)] ***********************************************
PLAY RECAP *********************************************************************
localhost                  : ok=2    changed=1    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0
INFO     Running default > syntax
playbook: /home/konstantin/DEVOPS_COURSE/ANSIBLE_ROLE/playbook/molecule/default/converge.yml
INFO     Running default > create
[WARNING]: Collection community.docker does not support Ansible version 2.10.8
PLAY [Create] ******************************************************************
TASK [Log into a Docker registry] **********************************************
skipping: [localhost] => (item=None) 
skipping: [localhost]
TASK [Check presence of custom Dockerfiles] ************************************
ok: [localhost] => (item={'image': 'quay.io/centos/centos:stream8', 'name': 'instance', 'pre_build_image': True})
TASK [Create Dockerfiles from image names] *************************************
skipping: [localhost] => (item={'image': 'quay.io/centos/centos:stream8', 'name': 'instance', 'pre_build_image': True})
TASK [Discover local Docker images] ********************************************
ok: [localhost] => (item={'changed': False, 'skipped': True, 'skip_reason': 'Conditional result was False', 'item': {'image': 'quay.io/centos/centos:stream8', 'name': 'instance', 'pre_build_image': True}, 'ansible_loop_var': 'item', 'i': 0, 'ansible_index_var': 'i'})
TASK [Build an Ansible compatible image (new)] *********************************
skipping: [localhost] => (item=molecule_local/quay.io/centos/centos:stream8) 
TASK [Create docker network(s)] ************************************************
TASK [Determine the CMD directives] ********************************************
ok: [localhost] => (item={'image': 'quay.io/centos/centos:stream8', 'name': 'instance', 'pre_build_image': True})
TASK [Create molecule instance(s)] *********************************************
changed: [localhost] => (item=instance)
TASK [Wait for instance(s) creation to complete] *******************************
FAILED - RETRYING: Wait for instance(s) creation to complete (300 retries left).
FAILED - RETRYING: Wait for instance(s) creation to complete (299 retries left).
FAILED - RETRYING: Wait for instance(s) creation to complete (298 retries left).
FAILED - RETRYING: Wait for instance(s) creation to complete (297 retries left).
FAILED - RETRYING: Wait for instance(s) creation to complete (296 retries left).
FAILED - RETRYING: Wait for instance(s) creation to complete (295 retries left).
FAILED - RETRYING: Wait for instance(s) creation to complete (294 retries left).
changed: [localhost] => (item={'started': 1, 'finished': 0, 'ansible_job_id': '779049012551.10427', 'results_file': '/home/konstantin/.ansible_async/779049012551.10427', 'changed': True, 'failed': False, 'item': {'image': 'quay.io/centos/centos:stream8', 'name': 'instance', 'pre_build_image': True}, 'ansible_loop_var': 'item'})
PLAY RECAP *********************************************************************
localhost                  : ok=5    changed=2    unreachable=0    failed=0    skipped=4    rescued=0    ignored=0
INFO     Running default > prepare
WARNING  Skipping, prepare playbook not configured.
INFO     Running default > converge
PLAY [Converge] ****************************************************************
TASK [Gathering Facts] *********************************************************
[WARNING]: Collection community.docker does not support Ansible version 2.10.8
ok: [instance]
TASK [Include playbook] ********************************************************
PLAY RECAP *********************************************************************
instance                   : ok=1    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
INFO     Running default > idempotence
PLAY [Converge] ****************************************************************
TASK [Gathering Facts] *********************************************************
[WARNING]: Collection community.docker does not support Ansible version 2.10.8
ok: [instance]
TASK [Include playbook] ********************************************************
PLAY RECAP *********************************************************************
instance                   : ok=1    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
INFO     Idempotence completed successfully.
INFO     Running default > side_effect
WARNING  Skipping, side effect playbook not configured.
INFO     Running default > verify
INFO     Running Ansible Verifier
PLAY [Verify] ******************************************************************
TASK [Example assertion] *******************************************************
[WARNING]: Collection community.docker does not support Ansible version 2.10.8
ok: [instance] => {
    "changed": false,
    "msg": "All assertions passed"
}
PLAY RECAP *********************************************************************
instance                   : ok=1    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
INFO     Verifier completed successfully.
INFO     Running default > cleanup
WARNING  Skipping, cleanup playbook not configured.
INFO     Running default > destroy
[WARNING]: Collection community.docker does not support Ansible version 2.10.8
PLAY [Destroy] *****************************************************************
TASK [Destroy molecule instance(s)] ********************************************
changed: [localhost] => (item=instance)
TASK [Wait for instance(s) deletion to complete] *******************************
FAILED - RETRYING: Wait for instance(s) deletion to complete (300 retries left).
changed: [localhost] => (item=instance)
TASK [Delete docker networks(s)] ***********************************************
PLAY RECAP *********************************************************************
localhost                  : ok=2    changed=2    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0
INFO     Pruning extra files from scenario ephemeral directory

```

#### *Перейдём в каталог с ролью elastic-role и создадим сценарий тестирования по умолчаню при помощи molecule init scenario --driver-name docker*
```
konstantin@konstantin-forever:~/DEVOPS_COURSE/ANSIBLE_ROLE/playbook$ molecule init scenario --driver-name docker
INFO     Initializing new scenario default...
INFO     Initialized scenario in /home/konstantin/DEVOPS_COURSE/ANSIBLE_ROLE/playbook/molecule/default successfully.
```
#### *Добавим несколько разных дистрибутивов (centos:8, ubuntu:latest) для инстансов и протестируем роль, исправим найденные ошибки, если они есть.*
```
konstantin@konstantin-forever:~/DEVOPS_COURSE/ANSIBLE_ROLE/playbook$ molecule test
INFO     default scenario test matrix: dependency, lint, cleanup, destroy, syntax, create, prepare, converge, idempotence, side_effect, verify, cleanup, destroy
INFO     Performing prerun with role_name_check=0...
INFO     Running ansible-galaxy role install -vr requirements.yml --roles-path /home/konstantin/.cache/ansible-compat/2f2cc2/roles
INFO     Set ANSIBLE_LIBRARY=/home/konstantin/.cache/ansible-compat/2f2cc2/modules:/home/konstantin/.ansible/plugins/modules:/usr/share/ansible/plugins/modules
INFO     Set ANSIBLE_COLLECTIONS_PATH=/home/konstantin/.cache/ansible-compat/2f2cc2/collections:/home/konstantin/.ansible/collections:/usr/share/ansible/collections
INFO     Set ANSIBLE_ROLES_PATH=/home/konstantin/.cache/ansible-compat/2f2cc2/roles:roles:/home/konstantin/.ansible/roles:/usr/share/ansible/roles:/etc/ansible/roles
INFO     Running default > dependency
WARNING  Skipping, missing the requirements file.
WARNING  Skipping, missing the requirements file.
INFO     Running default > lint
INFO     Lint is disabled.
INFO     Running default > cleanup
WARNING  Skipping, cleanup playbook not configured.
INFO     Running default > destroy
INFO     Sanity checks: 'docker'
[WARNING]: Collection community.docker does not support Ansible version 2.10.8
PLAY [Destroy] *****************************************************************
TASK [Destroy molecule instance(s)] ********************************************
changed: [localhost] => (item=centos8)
changed: [localhost] => (item=centos7)
changed: [localhost] => (item=ubuntu)
TASK [Wait for instance(s) deletion to complete] *******************************
FAILED - RETRYING: Wait for instance(s) deletion to complete (300 retries left).
changed: [localhost] => (item=centos8)
changed: [localhost] => (item=centos7)
changed: [localhost] => (item=ubuntu)
TASK [Delete docker networks(s)] ***********************************************
PLAY RECAP *********************************************************************
localhost                  : ok=2    changed=2    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0
INFO     Running default > syntax
playbook: /home/konstantin/DEVOPS_COURSE/ANSIBLE_ROLE/playbook/molecule/default/converge.yml
INFO     Running default > create
[WARNING]: Collection community.docker does not support Ansible version 2.10.8
PLAY [Create] ******************************************************************
TASK [Log into a Docker registry] **********************************************
skipping: [localhost] => (item=None) 
skipping: [localhost] => (item=None) 
skipping: [localhost] => (item=None) 
skipping: [localhost]
TASK [Check presence of custom Dockerfiles] ************************************
ok: [localhost] => (item={'image': 'docker.io/pycontribs/centos:8', 'name': 'centos8', 'pre_build_image': True})
ok: [localhost] => (item={'image': 'docker.io/pycontribs/centos:7', 'name': 'centos7', 'pre_build_image': True})
ok: [localhost] => (item={'image': 'docker.io/pycontribs/ubuntu:latest', 'name': 'ubuntu', 'pre_build_image': True})
TASK [Create Dockerfiles from image names] *************************************
skipping: [localhost] => (item={'image': 'docker.io/pycontribs/centos:8', 'name': 'centos8', 'pre_build_image': True})
skipping: [localhost] => (item={'image': 'docker.io/pycontribs/centos:7', 'name': 'centos7', 'pre_build_image': True})
skipping: [localhost] => (item={'image': 'docker.io/pycontribs/ubuntu:latest', 'name': 'ubuntu', 'pre_build_image': True})
TASK [Discover local Docker images] ********************************************
ok: [localhost] => (item={'changed': False, 'skipped': True, 'skip_reason': 'Conditional result was False', 'item': {'image': 'docker.io/pycontribs/centos:8', 'name': 'centos8', 'pre_build_image': True}, 'ansible_loop_var': 'item', 'i': 0, 'ansible_index_var': 'i'})
ok: [localhost] => (item={'changed': False, 'skipped': True, 'skip_reason': 'Conditional result was False', 'item': {'image': 'docker.io/pycontribs/centos:7', 'name': 'centos7', 'pre_build_image': True}, 'ansible_loop_var': 'item', 'i': 1, 'ansible_index_var': 'i'})
ok: [localhost] => (item={'changed': False, 'skipped': True, 'skip_reason': 'Conditional result was False', 'item': {'image': 'docker.io/pycontribs/ubuntu:latest', 'name': 'ubuntu', 'pre_build_image': True}, 'ansible_loop_var': 'item', 'i': 2, 'ansible_index_var': 'i'})
TASK [Build an Ansible compatible image (new)] *********************************
skipping: [localhost] => (item=molecule_local/docker.io/pycontribs/centos:8) 
skipping: [localhost] => (item=molecule_local/docker.io/pycontribs/centos:7) 
skipping: [localhost] => (item=molecule_local/docker.io/pycontribs/ubuntu:latest)
TASK [Create docker network(s)] ************************************************
TASK [Determine the CMD directives] ********************************************
ok: [localhost] => (item={'image': 'docker.io/pycontribs/centos:8', 'name': 'centos8', 'pre_build_image': True})
ok: [localhost] => (item={'image': 'docker.io/pycontribs/centos:7', 'name': 'centos7', 'pre_build_image': True})
ok: [localhost] => (item={'image': 'docker.io/pycontribs/ubuntu:latest', 'name': 'ubuntu', 'pre_build_image': True})
TASK [Create molecule instance(s)] *********************************************
changed: [localhost] => (item=centos8)
changed: [localhost] => (item=centos7)
changed: [localhost] => (item=ubuntu)
TASK [Wait for instance(s) creation to complete] *******************************
FAILED - RETRYING: Wait for instance(s) creation to complete (300 retries left).
changed: [localhost] => (item={'started': 1, 'finished': 0, 'ansible_job_id': '6755034777.15123', 'results_file': '/home/konstantin/.ansible_async/6755034777.15123', 'changed': True, 'failed': False, 'item': {'image': 'docker.io/pycontribs/centos:8', 'name': 'centos8', 'pre_build_image': True}, 'ansible_loop_var': 'item'})
FAILED - RETRYING: Wait for instance(s) creation to complete (300 retries left).
FAILED - RETRYING: Wait for instance(s) creation to complete (299 retries left).
FAILED - RETRYING: Wait for instance(s) creation to complete (298 retries left).
FAILED - RETRYING: Wait for instance(s) creation to complete (297 retries left).
FAILED - RETRYING: Wait for instance(s) creation to complete (296 retries left).
changed: [localhost] => (item={'started': 1, 'finished': 0, 'ansible_job_id': '762772668752.15149', 'results_file': '/home/konstantin/.ansible_async/762772668752.15149', 'changed': True, 'failed': False, 'item': {'image': 'docker.io/pycontribs/centos:7', 'name': 'centos7', 'pre_build_image': True}, 'ansible_loop_var': 'item'})
changed: [localhost] => (item={'started': 1, 'finished': 0, 'ansible_job_id': '189057004072.15181', 'results_file': '/home/konstantin/.ansible_async/189057004072.15181', 'changed': True, 'failed': False, 'item': {'image': 'docker.io/pycontribs/ubuntu:latest', 'name': 'ubuntu', 'pre_build_image': True}, 'ansible_loop_var': 'item'})
PLAY RECAP *********************************************************************
localhost                  : ok=5    changed=2    unreachable=0    failed=0    skipped=4    rescued=0    ignored=0
INFO     Running default > prepare
WARNING  Skipping, prepare playbook not configured.
INFO     Running default > converge
PLAY [Converge] ****************************************************************
TASK [Gathering Facts] *********************************************************
[WARNING]: Collection community.docker does not support Ansible version 2.10.8
[WARNING]: Collection community.docker does not support Ansible version 2.10.8
[WARNING]: Collection community.docker does not support Ansible version 2.10.8
ok: [centos8]
ok: [ubuntu]
ok: [centos7]
TASK [Include playbook] ********************************************************
PLAY RECAP *********************************************************************
centos7                    : ok=1    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
centos8                    : ok=1    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
ubuntu                     : ok=1    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
INFO     Running default > idempotence
PLAY [Converge] ****************************************************************
TASK [Gathering Facts] *********************************************************
[WARNING]: Collection community.docker does not support Ansible version 2.10.8
[WARNING]: Collection community.docker does not support Ansible version 2.10.8
[WARNING]: Collection community.docker does not support Ansible version 2.10.8
ok: [centos8]
ok: [ubuntu]
ok: [centos7]
TASK [Include playbook] ********************************************************
PLAY RECAP *********************************************************************
centos7                    : ok=1    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
centos8                    : ok=1    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
ubuntu                     : ok=1    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
INFO     Idempotence completed successfully.
INFO     Running default > side_effect
WARNING  Skipping, side effect playbook not configured.
INFO     Running default > verify
INFO     Running Ansible Verifier
PLAY [Verify] ******************************************************************
TASK [Example assertion] *******************************************************
[WARNING]: Collection community.docker does not support Ansible version 2.10.8
[WARNING]: Collection community.docker does not support Ansible version 2.10.8
ok: [centos7] => {
    "changed": false,
    "msg": "All assertions passed"
}
[WARNING]: Collection community.docker does not support Ansible version 2.10.8
ok: [centos8] => {
    "changed": false,
    "msg": "All assertions passed"
}
ok: [ubuntu] => {
    "changed": false,
    "msg": "All assertions passed"
}

PLAY RECAP *********************************************************************
centos7                    : ok=1    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
centos8                    : ok=1    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
ubuntu                     : ok=1    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

INFO     Verifier completed successfully.
INFO     Running default > cleanup
WARNING  Skipping, cleanup playbook not configured.
INFO     Running default > destroy
[WARNING]: Collection community.docker does not support Ansible version 2.10.8
PLAY [Destroy] *****************************************************************
TASK [Destroy molecule instance(s)] ********************************************
changed: [localhost] => (item=centos8)
changed: [localhost] => (item=centos7)
changed: [localhost] => (item=ubuntu)
TASK [Wait for instance(s) deletion to complete] *******************************
FAILED - RETRYING: Wait for instance(s) deletion to complete (300 retries left).
changed: [localhost] => (item=centos8)
changed: [localhost] => (item=centos7)
changed: [localhost] => (item=ubuntu)
TASK [Delete docker networks(s)] ***********************************************
PLAY RECAP *********************************************************************
localhost                  : ok=2    changed=2    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0
INFO     Pruning extra files from scenario ephemeral directory
```
#### *Создадим новый каталог с ролью при помощи molecule init role --driver-name docker kibana-role. Используем другой драйвер, который более удобен.*
```
konstantin@konstantin-forever:~/DEVOPS_COURSE/ANSIBLE_ROLE/playbook/roles$ ansible-galaxy init -v --offline kibana-role
No config file found; using defaults
- Role kibana-role was created successfully
```
#### *На основе tasks из старого playbook заполним новую role. Разнесиём переменные между vars и default. Проведиём тестирование на разных дистрибитивах (centos:7, centos:8, ubuntu).*
```

...
PLAY [Destroy] *****************************************************************

TASK [Destroy molecule instance(s)] ********************************************
changed: [localhost] => (item=centos8)
changed: [localhost] => (item=centos7)
changed: [localhost] => (item=ubuntu)

TASK [Wait for instance(s) deletion to complete] *******************************
FAILED - RETRYING: Wait for instance(s) deletion to complete (300 retries left).
changed: [localhost] => (item=centos8)
changed: [localhost] => (item=centos7)
changed: [localhost] => (item=ubuntu)

TASK [Delete docker networks(s)] ***********************************************

PLAY RECAP *********************************************************************
localhost                  : ok=2    changed=2    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0

INFO     Pruning extra files from scenario ephemeral directory
```
#### *Выложим все roles в репозитории. Проставим тэги, используя семантическую нумерацию.*

[kibana-role репозиторий](https://github.com/VitkinKN/kibana-role)
#### *Добавим roles в requirements.yml в playbook.*
```yml
---
  - src: git@github.com:VitkinKN/elastic-role.git
    scm: git
    version: master
    name: elastic-role
  - src: git@github.com:VitkinKN/kibana-role.git
    scm: git
    version: master
    name: kibana-role
```
#### *Переработайте playbook на использование roles.*
```yml
---
- hosts: elastichost
  roles:
  - elastic-role
- hosts: kibanahost
  roles:
  - kibana-role
```
#### *Прописываем playbook, устанавливаем переменные для каждой роли*

```
konstantin@konstantin-forever:~/DEVOPS_COURSE/ANSIBLE_ROLE/playbook$ ansible-playbook -i inventory/prod.yml site.yml

PLAY [elastichost] *************************************************************

TASK [Gathering Facts] *********************************************************
ok: [elastichost]

TASK [elastic-role : Upload tar.gz Elasticsearch from local storage] ***********
ok: [elastichost]

TASK [elastic-role : Create directrory for Elasticsearch] **********************
ok: [elastichost]

TASK [elastic-role : Extract Elasticsearch in the installation directory] ******
skipping: [elastichost]

TASK [elastic-role : Set environment Elastic] **********************************
ok: [elastichost]

PLAY [kibanahost] **************************************************************

TASK [Gathering Facts] *********************************************************
ok: [kibanahost]

TASK [kibana-role : Upload tar.gz kibana from local storage] *******************
ok: [kibanahost]

TASK [kibana-role : Create directrory for Kibana (/opt/kibana/7.10.1)] *********
ok: [kibanahost]

TASK [kibana-role : Extract Kibana in the installation directory] **************
skipping: [kibanahost]

TASK [kibana-role : Set environment Kibana] ************************************
ok: [kibanahost]

PLAY RECAP *********************************************************************
elastichost                : ok=4    changed=0    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0   
kibanahost                 : ok=4    changed=0    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0   
```
___
