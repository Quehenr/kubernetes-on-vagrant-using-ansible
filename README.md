# 목표
Vagrant로 Virtual OS를 구성 후 Ansible을 통해서 kubernetes cluster 환경을 설정합니다.

# Prerequisites
## Vagrant
https://www.vagrantup.com/downloads 를 참고하여 설치합니다.<br>
설치 후 다음과 같은 command 를 통해서 확인하실 수 있습니다.
```bash
$ vagrant --version 
Vagrant 2.2.14
```

## VirtualBox
https://www.virtualbox.org/wiki/Downloads 를 참고하여 설치합니다.

## Ansible
https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html 를 참고하여 설치합니다.
#### ex. asdf를 통한 설치
1.  `asdf`를 통한 python 환경 설정
    ```bash
    # python local version 설정
    $ asdf local python 3.9.1
    
    # asdf local version 확인
    $ asdf current
    java            ______          No version set. Run "asdf <global|shell|local> java <version>"
    maven           ______          No version set. Run "asdf <global|shell|local> maven <version>"
    python          3.9.1           /Users/younggyu.lee/quehenr/private/infrastructure/kubernetes-on-vagrant-using-ansible/.tool-versions
    
    # python package manager 확인
    $ pip --version
    pip 21.0.1 from /Users/younggyu.lee/.asdf/installs/python/3.9.1/lib/python3.9/site-packages/pip (python 3.9)
    ```
    
2.  ansible 설치
    ```bash
    # ansible 설치
    $ pip install ansible
    
    # 설치된 모듈 path 적용
    $ asdf reshim python
    
    # 설치 확인
    $ ansible --version
    
    ansible 2.10.6
    config file = None
    configured module search path = ['/Users/younggyu.lee/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
    ansible python module location = /Users/younggyu.lee/.asdf/installs/python/3.9.1/lib/python3.9/site-packages/ansible
    executable location = /Users/younggyu.lee/.asdf/installs/python/3.9.1/bin/ansible
    python version = 3.9.1 (default, Feb 15 2021, 15:12:22) [Clang 12.0.0 (clang-1200.0.32.29)]
    ```

# 실행 명령어
#### VM 시작
```bash
vagrant up
```
#### VM 중지
```bash
vagrant halt
```
#### VM 상태 조회
```bash
vagrant status
```
#### VM 삭제
```bash
vagrant destroy
```
#### VM box list 조회
```bash
vagrant box list
```
#### VM 접속 
```bash
vagrant ssh <VM-name>
```
#### VM provision
```bash
vagrant provision
```
	
# Kubernetes Cluster 구조
|VM name| IP address | CPUs | Memory |
|---|---|---|---|
|k8s-master| 172.16.0.10 | 2 | 2 GB |
|k8s-node-1| 172.16.0.11 | 1 | 2 GB |
|k8s-node-2| 172.16.0.12 | 1 | 2 GB |
