# jetbrains
YouTrack - UpSource - Hub - Team City Docker on one single machine.

```
track.example.com   source.example.com   hub.example.com
       +                   +                    +
       |                   |                    |
       +----------------------------------------+
                           |
                           v
                         Nginx
                           +
       +-----------------------------------------+
       |                   |                     |
       v                   v                     v
  You Track              UpSource              Hub
 [host:youtrack:8080]   [host:upsource:8090]  [host:hub:7990]
       +                   +                     |
       |                   |                     |
       +-----------------------------------------+
                           |
                           v
                        Postgres
                   [host:database:5432]
                           +
                           |
       +------------------------------------------+
       |                   |                      |
       v                   v                      v
   [db:jira]           [db:wiki]          [db:bitbucket]
```


With:
- Postgres `9.4`
- Nginx `latest`

Requirements:

- Docker version 1.13.1+
- docker-compose version 1.10.0+

Docker image source files:

- [nginx](https://hub.docker.com/_/nginx/)
- [postgres](https://hub.docker.com/_/postgres/)

How to use:

1. Clone the atlassian:


    ```
    $ git clone https://github.com/arizonacoders/jetbrains
    ```

2. Set environment variables:

    ```
    $ export DOMAIN=example.com
     ```
 
3. Run docker compose:


    ```
    $ docker-compose -p -d jetbrains up
    ```
    
4. Set `DNS` according to the above `DOMAIN` value, on somewhere that you want to connect to host of `docker-compose`:


    ``` 
    $ vim /etc/hosts
        127.0.0.1 track.example.com www.track.example.com
        127.0.0.1 source.example.com www.source.example.com
        127.0.0.1 hub.example.com www.hub.example.com
    ```
Replace `127.0.0.1` with IP of host that `docker-compose` command run on it.

5. Create Databases:


    ```
    $ docker exec -it atlassian_database_1  psql -U postgres
       postgres=# CREATE DATABASE track;
       postgres=# CREATE DATABASE source;
       postgres=# CREATE DATABASE hub;
       postgres=# \l
       postgres-# \q
    ```
Notes: 

Data persisted on the  named volumes, to see them:


       $ docker volume ls
       local               hub-data
