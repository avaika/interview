# Interview hell

## What's this?
One day I thought it'd be nice to write down interview questions. I'm old and can't keep everything in my head already. So here we are.

## Any hints?
1. Do not ask all the questions from this list. In most cases it's enough to go through just some.
2. Feel free to end the interview as soon as you have a good idea about candidate level. There's no need to torture both of you.
3. Please please try to avoid questions like 'What is X?'. Ask 'How to do Y?' instead. This way is more fun :)
4. Questions marked with `#fun:` are tricky. I do not expect a correct answer in most cases, but it's fun to try to resolve the issue at least.
5. It's very helpful to share command line and show code snippets to candidate. Use this nice feature.

# The questions

## Linux
1. Memory:
    1. How `free`, `htop` and other guys know how much memory is consumed?
    2. What will happen if we disable swap? Short term and long term.
    3. How to save a process from OOM?
2. File descriptors:
    1. Root partition is out of space. How are you going to fix it? 
    2. apache log ate all the space. How to clean up the space without apache restart?
    3. How to find which files are used by process?
3. File permissions:
    1. passwd is able to write data to `-rw-r----- 1 root shadow /etc/shadow` without `sudo`. How does it work?
    2. What will be the permission after `chmod 1777 dir`?
    3. #fun: root just did this: `cd ~user; touch file; chmod 000 file`. Can I remove this file as a user and why / why not?
4. Network: 
    1. There is no ping between 2 servers. How to find out the root cause?
    2. How to check whether the port is being used?
    3. How to redirect traffic between servers? The more options the better.
    4. The app is listening to the port, but isn't accessible from remote server. How would you debug?
5. Processes:
    1. Is there a way to ignore -9 signal?
    2. How to allow other users to manage a specific process for your user without being root?
6. I see load average value for last 5 minutes is 14. Is it good or bad?
7. Why do we need ssh fingerprints?

## Scripting (bash/python)
1. bash
    1. `ls *tmp | while read file; do rm $file && echo "removed $file" >> log || echo "failed to remove $file" >> log; done` -- What might go wrong here?
    2. Difference `. ./file` vs `./file` 
    3. `echo "$SHELL"` vs `echo '$SHELL'`
    4. Why `str` is empty second time: `echo test | while read str; do a=$str; echo a=$a; done; echo a=$a`? (if while is blamed show `while read str; do a=$str; echo a=$a; done < <(echo test); echo a=$a` to prove `while` is innocent :) )
    5. What's to get script path from inside the script?
    6. #fun: `mkdir new; cd new; touch a; ls > b; cat b;` => what will happen and why?
2. python
    1. How to debug a python script?
    2. When and why do we need `__init__.py`?
    3. Why do we need a decorator?
    4. `f = lambda x: 2*x` => what will happen here? is it really worth to use lambdas?
    5. Any experience with frameworks?
    6. #fun: `True, True, True == (True, True, True)` => why deos it return `(True, True, False)`? :)

## Ansible
0. You have 100 machines. How would you install `nginx` on every one of them? (hope you won't hear about bash loop here :) )
1. roles vs playbooks. What's the difference?
2. You have a role to create database user and to set a password. How to store the creds in a safe way?
3. How to speed up ansible connection?
4. Please explain this template:
```
{% for server in servers -%}
  {% set serverloop = loop %}
  {%- for ip in server.ips|slice(3) %}
    allow {{ ip.ip }};  ## {{ serverloop.index }}-{{ loop.index }}
  {%- endfor %}
{% else %}
    allow 127.0.0.1;
{% endfor %}
```
5. #advanced: bind9 zone file has dns_serial entry which depends on generation date. How to ensure next deployment will update dns_serial in zone file only in case other vars were changed?
5. How do you test ansible changes?
6. #fun: ansible vs other tools. which is better and why?

## Docker
1. #fun: How to use a different kernel version inside docker container?
2. How does docker affect aplication performance?
3. There's a nice (oh my eyes) Dockerfile. Any ideas for optimizations?
```
FROM ubuntu

WORKDIR /app/
ADD . /app/

RUN apt-get update 
RUN apt-get install -y git
RUN apt-get install -y build-essential
RUN apt-get install -y python3-dev
RUN apt-get install -y python3-pip

RUN pip3 install -r /app/requirements.txt

RUN python3 /app/manage.py collectstatic

CMD ['/bin/bash /app/run.sh']
```
4. Docker container with multiple processes inside. What are pros & cons?
5. When do we need docker capabilities?
```
docker run -it bash
apk add e2fsprogs-extra
touch test
chattr +i test

docker run --cap-add LINUX_IMMUTABLE -it bash
```
6. docker alternatives. Why / why not?

## K8S
1. #fun: How to run containers from a single pod on different nodes?
2. Operators? Why and why not?
3. helm vs kapitan vs kustomize vs etc?
4. How to access external database in k8s?
5. What happens when a node goes down?
6. Application pod isn't able to access database pod. How to debug?
7. How to persist data in k8s?
8. I'm too lazy to put more questions.

## CI/CD
1. You have a java app source code. How would you build CI/CD pipeline?

## Databases
1. You have database replication. How would you ensure it's still working?

## Monitoring
1. How to monitor stuff?

## ELK
1. How to scale up and down elasticsearch cluster? Underwater stones?
2. Index rotation?
3. Alternatives for log collection?

# Fun things I've learned during interviews:
1. ping is a client-server application
2. bash has garbage collector
3. passwd has a daemon to update `/etc/shadow` password hash
