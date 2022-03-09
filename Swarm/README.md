# Docker Swarm

Let's try the Docker Swarm.

Document: https://ithelp.ithome.com.tw/articles/10247217


```
~ > docker swarm init 

Swarm initialized: current node (xdl1mi52jmb6p63mywxnvyhc) is now a manager.

To add a worker to this swarm, run the following command:

    docker swarm join --token SWMTKN-1-5y0cdwryyxo120dai3wxt54nmch2yu7ide9i6l4njinx-bnt9kfly8rbit0c104co5vnr6 10.1.2.123:2377

To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.
```

We need to create **worker** and **manager**, and we can use **join** to create them.

#### Start by docker-compose file

```
$ docker stack deploy --compose-file docker-compose.yml            
 mystack
```

#### Check node

```
$ docker node ls
```

#### Remove

Check the services first.
```
~ > docker service ls                                                                                            user@user-System-Product-Name 03:24:34 暮
ID             NAME            MODE         REPLICAS   IMAGE            PORTS
75p08p3uonwa   mystack_app     replicated   0/1        node:12-alpine   *:3000->3000/tcp
r4lt4gbgo7fq   mystack_mysql   replicated   1/1        mysql:5.7     
```

Remove the services.
```
~ > docker service rm 75p08p3uonwa                                                                               user@user-System-Product-Name 03:24:37 暮
75p08p3uonwa

~ > docker service rm r4lt4gbgo7fq                                                                               user@user-System-Product-Name 03:24:48 暮
r4lt4gbgo7fq
```

#### For example

```
$ ~/te/node-bulletin-board/bulletin-board-app master ?1 > docker swarm init          user@user-System-Product-Name 03:35:40 暮
Error response from daemon: This node is already part of a swarm. Use "docker swarm leave" to leave this swarm and join another one.

$ ~/te/node-bulletin-board/bulletin-board-app master ?1 > docker stack deploy -c bb-stack.yaml mystack
Creating service mystack_bb-app

$ ~/te/node-bulletin-board/bulletin-board-app master ?1 > docker service ls       3s user@user-System-Product-Name 03:36:15 暮
ID             NAME             MODE         REPLICAS   IMAGE               PORTS
vi0tevz9zlw5   mystack_bb-app   replicated   1/1        bulletinboard:1.0   *:8000->8080/tcp

$ ~/te/node-bulletin-board/bulletin-board-app master ?1 > docker service scale vi0tevz9zlw5=10vi0tevz9zlw5
vi0tevz9zlw5: invalid replicas value 10vi0tevz9zlw5: strconv.ParseUint: parsing "10vi0tevz9zlw5": invalid syntax

$ ~/te/node-bulletin-board/bulletin-board-app master ?1 > docker service scale vi0tevz9zlw5=10            
vi0tevz9zlw5 scaled to 10
overall progress: 10 out of 10 tasks 
1/10: running   [==================================================>] 
2/10: running   [==================================================>] 
3/10: running   [==================================================>] 
4/10: running   [==================================================>] 
5/10: running   [==================================================>] 
6/10: running   [==================================================>] 
7/10: running   [==================================================>] 
8/10: running   [==================================================>] 
9/10: running   [==================================================>] 
10/10: running   [==================================================>] 
verify: Service converged 

$ ~/te/node-bulletin-board/bulletin-board-app master ?1 > docker service ls      13s user@user-System-Product-Name 03:37:55 暮
ID             NAME             MODE         REPLICAS   IMAGE               PORTS
vi0tevz9zlw5   mystack_bb-app   replicated   10/10      bulletinboard:1.0   *:8000->8080/tcp

$ ~/te/node-bulletin-board/bulletin-board-app master ?1 > docker service ps vi0tevz9zlw5
ID             NAME                IMAGE               NODE                       DESIRED STATE   CURRENT STATE            ERROR     PORTS
jgjs9papr6dc   mystack_bb-app.1    bulletinboard:1.0   user-System-Product-Name   Running         Running 2 minutes ago              
8fny92gznadj   mystack_bb-app.2    bulletinboard:1.0   user-System-Product-Name   Running         Running 31 seconds ago             
pgp46g9v8eus   mystack_bb-app.3    bulletinboard:1.0   user-System-Product-Name   Running         Running 31 seconds ago             
n4n8ozcfmtcf   mystack_bb-app.4    bulletinboard:1.0   user-System-Product-Name   Running         Running 33 seconds ago             
3foqsptt0php   mystack_bb-app.5    bulletinboard:1.0   user-System-Product-Name   Running         Running 32 seconds ago             
08tc7382pmq4   mystack_bb-app.6    bulletinboard:1.0   user-System-Product-Name   Running         Running 33 seconds ago             
837s5xxh7yd4   mystack_bb-app.7    bulletinboard:1.0   user-System-Product-Name   Running         Running 31 seconds ago             
ngp2ripjviq3   mystack_bb-app.8    bulletinboard:1.0   user-System-Product-Name   Running         Running 33 seconds ago             
yd9mn1hcn3om   mystack_bb-app.9    bulletinboard:1.0   user-System-Product-Name   Running         Running 31 seconds ago             
zbicdvfj0ht1   mystack_bb-app.10   bulletinboard:1.0   user-System-Product-Name   Running         Running 31 seconds ago             
```

## Note:

1. The network mode needs to set by **overlay**
2. Not sure whether it can accept 2 web servers in one container and then applying several replicas.
    > It can work.
