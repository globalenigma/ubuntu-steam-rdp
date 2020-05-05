This is just a cloned copy of:
danielguerra/ubuntu-xrdp:18.04

Please see:

https://github.com/danielguerra69/ubuntu-xrdp


​
51
You can change your password in the rdp session in a terminal
52
​
53
```bash
54
passwd
55
```
56
​
57
## Add new users
58
​
59
No configuration is needed for new users just do
60
​
61
```bash
62
docker exec -ti uxrdp adduser mynewuser
63
```
64
​
65
After this the new user can login
66
​
67
## Add new services
68
​
69
To make sure all processes are working supervisor is installed.
70
The location for services to start is /etc/supervisor/conf.d
71
​
72
Example: Add mysql as a service
73
​
74
```bash
75
apt-get -yy install mysql-server
76
echo "[program:mysqld] \
77
command= /usr/sbin/mysqld \
78
user=mysql \
79
autorestart=true \
80
priority=100" > /etc/supervisor/conf.d/mysql.conf
81
supervisorctl update
82
```
83
​
84
## Volumes
85
This image uses two volumes:
86
1. `/etc/ssh/` holds the sshd host keys and config
87
2. `/home/` holds the `ubuntu/` default user home directory
88
​
89
When bind-mounting `/home/`, make sure it contains a folder `ubuntu/` with proper permission, otherwise no login will be possible.
90
​
91
```
92
mkdir -p ubuntu
93
chown 999:999 ubuntu
94
```
95
​
96
## Installing additional packages during build
97
​
98
The Dockerfile has support for the build argument ADDITIONAL_PACKAGES to install additional packages during build. Either pass it with `--build-arg` during `docker build` or add it 
99
as `args` in your `docker-compose.override.yml` and run `docker-compose build`.
100
​
101
## To run with docker-compose
102
​
103
```bash
104
git clone https://github.com/danielguerra69/ubuntu-xrdp.git
105
cd ubuntu-xrdp/
106
vi docker-compose.override.yml # if you want to override any default value
107
docker-compose up -d
108
```
