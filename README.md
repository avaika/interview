# Interview hell

## What's this?
One day I thought it'd be nice to write down some interview questions. I'm old and can't keep everything in my head anymore. So here we are.

## Any hints?
1. Do not ask all the questions from this list. In most cases it's enough to go through just some of them.
2. Feel free to end the interview as soon as you have a good idea about candidate level. There's no need to torture both of you.
3. Please please try to avoid questions like 'What is X?'. Ask 'How to do Y?' instead. This is way more fun. And actually gives you a better impression of how candidate understands the technology.
4. Questions marked with `#fun:` are mostly for fun in the first place, cause might be misleading or confusing for the candidate. Don't judge too hard based on these answers (unless it's a complete nonsense)
5. It might be helpful to share a screen with command line and show code snippets to candidate.
6. The scenario questions below are intentionally open-ended. There's no single right answer. Steer the conversation with follow-ups based on what the candidate says ("what if the DB is 2TB?", "what if there's no staging?"). This makes it nearly impossible to prepare a canned answer.

---

# Scenario questions

These are conversation starters, not trivia. The goal is to understand how the candidate thinks, prioritizes, and communicates trade-offs. Follow up on every answer — the real signal is in the discussion, not the first response.

## Architecture & Migration

1. **You're migrating a monolithic web app (app + DB + file storage) from on-prem to AWS. Walk me through your approach.**
    - Follow-ups: How do you handle the switch? What about data migration with minimal downtime? DNS? How do you validate nothing broke? What's your rollback plan?

2. **Your team runs a monolith. Product and engineering leadership want to move to microservices. CTO asks for your opinion. What do you say?**
    - Tests whether candidate can push back with nuance. Follow-ups: What problems are we actually trying to solve? What's the cost of the migration? When would you say "no, keep the monolith"? If you go ahead, what do you split out first and why?

3. **Your service currently runs in a single region. Business wants to expand to Europe and needs data to stay in EU. How do you approach this?**
    - Tests understanding of multi-region complexity: data residency, latency, consistency, deployment strategy, DNS routing. Follow-up: "The app uses a single PostgreSQL database with foreign keys everywhere. Now what?"

4. **You inherit a platform running on a single beefy VM — app, database, cron jobs, and nginx all on one box. It works. Management says "make it production-ready." What does that mean to you and where do you start?**
    - Tests ability to define "production-ready" without gold-plating. Follow-ups: What's the minimum viable improvement? How do you decide what to split out first?

## Incident Response & Debugging

1. **It's 3 AM, PagerDuty fires. Users report the site is slow. You SSH in. CPU is fine, memory is fine, disk is fine. What do you do next?**
    - Tests systematic debugging when the obvious metrics are green. Follow-ups: "Turns out it's the database, but you don't have DB access. Now what?" "You find the issue but the fix requires a deploy and nobody else is online. What do you do?"

2. **A deploy went out at 5 PM. At 5:15 PM, error rates spike to 10%. At 5:20 PM they drop back to 2% (pre-deploy was 1%). Do you rollback? Why or why not?**
    - Tests judgment under uncertainty. Follow-ups: "It's Friday evening. Does that change your decision?" "The next deploy is scheduled for Monday. Does that matter?"

3. **One of your services starts throwing 500 errors intermittently. Roughly 2% of requests, seemingly random. No recent deploys. Infra metrics look normal. How do you investigate?**
    - Tests methodical debugging of non-obvious issues. Follow-ups: "You add more logging and the problem temporarily goes away. Now what?" "You suspect it's a dependency, but that team says everything is fine on their end."

4. **Your service is fine, but a downstream dependency starts responding slowly (p99 latency went from 50ms to 2s). Your service's error rate is climbing. What do you do, both right now and longer term?**
    - Tests understanding of cascading failures, circuit breakers, timeouts, graceful degradation. Follow-up: "The downstream team says it'll take a week to fix. Can you survive a week?"

## Design & Trade-offs

1. **You need to design a system that sends email notifications when a user's subscription is about to expire (3 days before). How would you build it?**
    - Sounds simple, but reveals understanding of: cron vs. queues, idempotency, time zones, retries, observability. Follow-ups: "What breaks if the job server goes down for a day?" "How do you make sure nobody gets the email twice?" "How about users in different time zones?"

2. **Your team's deploy process takes 45 minutes and involves 8 manual steps documented in a wiki. The team wants to automate it. How do you approach this?**
    - Tests pragmatism: do you automate everything at once? What do you automate first? How do you handle the manual approvals that exist for a reason? Follow-up: "Two of the steps require logging into a production console and running SQL queries."

3. **You're tasked with building a CI/CD pipeline from scratch for a team that currently deploys by copying files to a server via SCP. Where do you start?**
    - Tests ability to meet people where they are and improve incrementally. Follow-ups: "The team has no tests." "The 'build' is just zipping a directory." "They deploy to 5 servers."

4. **You need to move a configuration value that's currently hardcoded in the application to something that can be changed without a deploy. How would you do it?**
    - Tests understanding of config management spectrum: env vars, config files, feature flags, config services, database. Follow-ups: "What if the value needs to change per-region?" "What if it needs to change within seconds, not minutes?" "Who should be able to change it?"

## Reliability & Scaling

1. **You run a service that handles 100 req/s. Product tells you a marketing campaign will 10x traffic next month. What do you do?**
    - Tests capacity planning thinking, load testing approach, identifying bottlenecks before they happen. Follow-ups: "Where do you expect it to break first?" "How do you load test without affecting production?" "What's your plan if it still falls over on launch day?"

2. **You have a critical service with no tests, no docs, written by someone who left. It works, but you need to make changes to it. How do you proceed?**
    - Tests risk management and pragmatism with legacy systems. Follow-ups: "How do you know if your change broke something?" "Would you rewrite it?" "What if you only have 2 days?"

3. **Your team's API has an informal SLA of 'it should be fast and not go down.' Leadership wants a real SLO. How do you define one? What do you measure?**
    - Tests understanding of SLO/SLI/error budgets. Follow-ups: "Your SLO is 99.9% availability. Last quarter you hit 99.7%. What do you do?" "The product team wants to ship a risky feature. Your error budget is almost spent. What happens?"

4. **Your primary database is 2TB and growing 10% monthly. Queries are getting slower. What do you do?**
    - Tests understanding of database scaling strategies: indexing, read replicas, archival, partitioning, sharding. Follow-ups: "The app does lots of joins." "Can you archive old data? How do you decide what's old?" "Product says they need all data queryable."

5. **You manage a fleet of 50 servers. Patching is done manually and is always behind. How do you improve this?**
    - Tests automation thinking and risk management. Follow-ups: "How do you handle patches that require reboot?" "How do you test patches before rolling them out everywhere?" "What about zero-day vulnerabilities?"

## Security & Access

1. **A developer asks for SSH access to production to debug an issue. What's your response?**
    - Tests security thinking vs. pragmatism. No single right answer — reveals philosophy. Follow-ups: "What if it's a critical outage?" "What alternatives do you offer?" "How do you balance security with developer productivity?"

2. **You discover that an API key for a third-party service was accidentally committed to a public repo 3 days ago. What do you do?**
    - Tests incident response for security events. Follow-ups: "The key has admin privileges." "Rotating the key requires coordination with 3 other teams and will cause 30 minutes of downtime." "How do you prevent this from happening again?"

3. **Your company needs to become SOC2 compliant. The CTO says you have 6 months. Where do you start from an infrastructure perspective?**
    - Tests understanding of compliance as a practical problem, not just a checkbox. Follow-ups: "You currently have no centralized logging." "Developers share a single AWS account." "There's no documented access review process."

4. **An engineer leaves the company. What do you do from an infrastructure/access perspective?**
    - Tests understanding of access management, secrets, and offboarding. Follow-ups: "They had personal AWS IAM keys." "They were the only person who knew the root account password for a legacy system." "They had SSH keys on 20 servers."

---

# Technical deep-dives

These are more specific technical questions. Good for gauging hands-on experience and depth of understanding in core areas.

## Linux
1. OS Processes:
    1. Can process survive `kill -9` in Linux system?
    2. `uptime` returns load average value as 14. Is it high or low? Should I worry about it?
    3. How to launch process via SSH and make the process survive SSH session disconnect?
    4. How to allow other users to manage a specific process for your user without being root?
2. Memory:
    1. How `free`, `htop` and other guys know how much memory is consumed?
    2. What will happen if we disable swap? Short term and long term.
    3. How to save a process from OOM?
3. File descriptors:
    1. Root partition is out of space. How are you going to fix it?
    2. nginx log ate all the space. How to clean up the space without nginx restart?
    3. Ops accidentally removed apache log file, but the daemon is still running. Is there a way to restore the file?
    4. `df -h` shows a lot of unused disk space, however any file creation operation fails with out of space error. What happened, how to fix?
    5. How to delete 1kk files fast w/o removing root directory?
    6. What is the difference between symlinks & hard links?
3. File permissions:
    1. passwd is able to write data to `-rw-r----- 1 root shadow /etc/shadow` from a user without `sudo`. How does it work?
    2. What will be the permission after `chmod 1777 dir`?
    3. If I will change permission for a file with multiple hardlinks, does it affect all of them or just one?
    4. #fun: root just did this: `cd ~user; touch file; chmod 000 file`. Can I remove the `file` as a user and why / why not?
4. Network:
    1. There is no ping between 2 servers. How to find out the root cause?
    2. How to check whether the port is being used?
    3. How to redirect traffic between servers? The more options the better.
    4. The app is listening to the port, but isn't accessible from remote server. How would you debug?

## Scripting (bash/python)
1. bash
    1. How to debug a shell script?
    2. What's a difference between `. ./file` vs `./file`?
    3. `ls *tmp | while read file; do rm $file && echo 'removed $file' >> log.tmp || echo "failed to remove $file" > log.tmp; done` -- what might go wrong here?
    4. `#!`, `$?`, `$1`, `##`, `$$`, `$!` -- when those are needed?
    5. Why is `str` empty with the second `echo`: `echo test | while read str; do a=$str; echo a=$a; done; echo a=$a`? (if `while` is blamed show `while read str; do a=$str; echo a=$a; done < <(echo test); echo a=$a` to prove `while` is innocent :) )
    6. #fun: `mkdir new; cd new; touch a; ls > b; cat b;` => what will be the output and why?
    7. `failure | cat` returns exit code 0. Is there a way to catch the exception from `failure`?
2. python
    1. How to debug a python script?
    2. When and why do we need `__init__.py`?
    3. Why do we need a decorator?
    4. `f = lambda x: 2*x` => what will happen here? is it really worth to use lambdas?
    5. Any experience with frameworks?
    6. #fun: `True, True, True == (True, True, True)` => why does it return `(True, True, False)`? :)

## Docker
1. How does docker affect application performance?
2. There's a nice (oh my eyes) Dockerfile. Any ideas for optimizations?
```
FROM ubuntu

WORKDIR /app/
ADD . /app/

RUN apt-get update
RUN apt-get install -y git
RUN apt-get install -y build-essential
RUN apt-get install -y python3-dev python3-pip

RUN pip3 install -r /app/requirements.txt

RUN apt-get purge -y python3-dev build-essential

RUN python3 /app/manage.py collectstatic

CMD ['/bin/bash', '/app/run.sh']
```
3. Docker container with multiple processes inside. What are pros & cons?

## K8S
1. What happens when a node goes down?
2. Application pod isn't able to access database pod. How to debug?
3. What's the difference between liveness / readiness / startup probes in k8s?

## Networks
1. Main differences between tcp and udp.
2. Main differences between nat and masquerade in iptables notation.
3. Describe process of `ping ddg.co` in details.

## GIT
1. How we can revert the latest merge request?
2. What's difference between `rebase` and `cherry-pick`?

---

# Fun things I've "learned" during interviews:
1. ping is a client-server application
2. bash has jvm-like garbage collector
3. If you want to check whether the file is used by process or not, you need to remove the file. If it is used, the system won't let you remove it :)
4. docker improves application performance, as there's only one process inside container and no other processes compete for the resources.
