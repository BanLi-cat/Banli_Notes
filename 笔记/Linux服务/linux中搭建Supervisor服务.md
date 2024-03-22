## supervisor简介

```txt
Supervisor是用Python开发的一套通用的进程管理程序，能将一个普通的命令行进程变为后台daemon，并监控进程状态，异常退出时能自动重启。
只要在supervisor的配置文件中，把要管理的进程的可执行文件的路径写进去即可。
也实现当子进程挂掉的时候，父进程可以准确获取子进程挂掉的信息的，可以选择是否自己启动和报警。
```

## supervisor安装

```sh

# 1.安装
yum install -y supervisor

# 2.配置
echo_supervisord_conf > /data/localData/z_home/supervisor/conf/supervisord.conf   # 配置文件位置可自行定义

```

## supervisor使用

```sh

# 目录树
/data/localData/z_home/supervisor

├── bin
├── conf
│   ├── code_v_2_10_guangxin.conf               # 子进程配置文件
│   ├── collection_compensation.conf            # 子进程配置文件
│   ├── media_backup.conf                       # 子进程配置文件
│   ├── supervisord.conf                        # supervisor配置文件
│   └── videoUpload.conf                        # 子进程配置文件
└── log
    └── collection_compensation_stdout.log      # 日志文件

```

## 配置文件说明

### supervisord.conf

```conf

; Sample supervisor config file.
;
; For more information on the config file, please see:
; http://supervisord.org/configuration.html
;
; Notes:
;  - Shell expansion ("~" or "$HOME") is not supported.  Environment
;    variables can be expanded using this syntax: "%(ENV_HOME)s".
;  - Quotes around values are not supported, except in the case of
;    the environment= options as shown below.
;  - Comments must have a leading space: "a=b ;comment" not "a=b;comment".
;  - Command will be truncated if it looks like a config file comment, e.g.
;    "command=bash -c 'foo ; bar'" will truncate to "command=bash -c 'foo ".

[unix_http_server]
file=/data/localData/z_home/supervisor/bin/supervisor.sock   ; UNIX socket 文件，supervisorctl 会使用
;chmod=0700                 ; socket文件的mode，默认是0700
;chown=nobody:nogroup       ; ssocket文件的owner，格式：uid:gid
;username=user              ; default is no username (open server)
;password=123               ; default is no password (open server)

[inet_http_server]          ; HTTP服务器，提供web管理界面
port=0.0.0.0:9001           ; Web管理后台运行的IP和端口，如果开放到公网，需要注意安全性
;port=127.0.0.1:9001        ; ip_address:port specifier, *:port for all iface
username=user               ; 登录管理后台的用户名
password=123                ; 登录管理后台的密码

[supervisord]
logfile=/data/localData/z_home/supervisor/log/supervisord.log ; 日志文件，默认是 $CWD/supervisord.log
logfile_maxbytes=50MB        ; 日志文件大小，超出会rotate，默认 50MB，如果设成0，表示不限制大小
logfile_backups=10           ; 日志文件保留备份数量默认10，设为0表示不备份
loglevel=info                ; 日志级别，默认info，其它: debug,warn,trace
pidfile=/data/localData/z_home/supervisor/bin/supervisord.pid ; pid 文件
nodaemon=false               ; 是否在前台启动，默认是false，即以 daemon 的方式启动
minfds=1024                  ; 可以打开的文件描述符的最小值，默认 1024
minprocs=200                 ; 可以打开的进程数的最小值，默认 200
;umask=022                   ; process file creation umask; default 022
;user=supervisord            ; setuid to this UNIX account at startup; recommended if root
;identifier=supervisor       ; supervisord identifier, default is 'supervisor'
;directory=/tmp              ; default is not to cd during start
;nocleanup=true              ; don't clean up tempfiles at start; default false
;childlogdir=/tmp            ; 'AUTO' child log dir, default $TEMP
;environment=KEY="value"     ; key value pairs to add to environment
;strip_ansi=false            ; strip ansi escape codes in logs; def. false

; The rpcinterface:supervisor section must remain in the config file for
; RPC (supervisorctl/web interface) to work.  Additional interfaces may be
; added by defining them in separate [rpcinterface:x] sections.

[rpcinterface:supervisor]
supervisor.rpcinterface_factory = supervisor.rpcinterface:make_main_rpcinterface

; The supervisorctl section configures how supervisorctl will connect to
; supervisord.  configure it match the settings in either the unix_http_server
; or inet_http_server section.

[supervisorctl]
serverurl=unix:///data/localData/z_home/supervisor/bin/supervisor.sock ; 通过UNIX socket连接supervisord，路径与unix_http_server部分的file一致
;serverurl=http://127.0.0.1:9001 ; 通过HTTP的方式连接supervisord
;username=chris              ; should be same as in [*_http_server] if set
;password=123                ; should be same as in [*_http_server] if set
;prompt=mysupervisor         ; cmd line prompt (default "supervisor")
;history_file=~/.sc_history  ; use readline history if available

; The sample program section below shows all possible program subsection values.
; Create one or more 'real' program: sections to be able to control them under
; supervisor.

;[program:theprogramname]
;command=/bin/cat              ; the program (relative uses PATH, can take args)
;process_name=%(program_name)s ; process_name expr (default %(program_name)s)
;numprocs=1                    ; number of processes copies to start (def 1)
;directory=/tmp                ; directory to cwd to before exec (def no cwd)
;umask=022                     ; umask for process (default None)
;priority=999                  ; the relative start priority (default 999)
;autostart=true                ; start at supervisord start (default: true)
;startsecs=1                   ; # of secs prog must stay up to be running (def. 1)
;startretries=3                ; max # of serial start failures when starting (default 3)
;autorestart=unexpected        ; when to restart if exited after running (def: unexpected)
;exitcodes=0,2                 ; 'expected' exit codes used with autorestart (default 0,2)
;stopsignal=QUIT               ; signal used to kill process (default TERM)
;stopwaitsecs=10               ; max num secs to wait b4 SIGKILL (default 10)
;stopasgroup=false             ; send stop signal to the UNIX process group (default false)
;killasgroup=false             ; SIGKILL the UNIX process group (def false)
;user=chrism                   ; setuid to this UNIX account to run the program
;redirect_stderr=true          ; redirect proc stderr to stdout (default false)
;stdout_logfile=/a/path        ; stdout log path, NONE for none; default AUTO
;stdout_logfile_maxbytes=1MB   ; max # logfile bytes b4 rotation (default 50MB)
;stdout_logfile_backups=10     ; # of stdout logfile backups (0 means none, default 10)
;stdout_capture_maxbytes=1MB   ; number of bytes in 'capturemode' (default 0)
;stdout_events_enabled=false   ; emit events on stdout writes (default false)
;stderr_logfile=/a/path        ; stderr log path, NONE for none; default AUTO
;stderr_logfile_maxbytes=1MB   ; max # logfile bytes b4 rotation (default 50MB)
;stderr_logfile_backups=10     ; # of stderr logfile backups (0 means none, default 10)
;stderr_capture_maxbytes=1MB   ; number of bytes in 'capturemode' (default 0)
;stderr_events_enabled=false   ; emit events on stderr writes (default false)
;environment=A="1",B="2"       ; process environment additions (def no adds)
;serverurl=AUTO                ; override serverurl computation (childutils)

; The sample eventlistener section below shows all possible eventlistener
; subsection values.  Create one or more 'real' eventlistener: sections to be
; able to handle event notifications sent by supervisord.

;[eventlistener:theeventlistenername]
;command=/bin/eventlistener    ; the program (relative uses PATH, can take args)
;process_name=%(program_name)s ; process_name expr (default %(program_name)s)
;numprocs=1                    ; number of processes copies to start (def 1)
;events=EVENT                  ; event notif. types to subscribe to (req'd)
;buffer_size=10                ; event buffer queue size (default 10)
;directory=/tmp                ; directory to cwd to before exec (def no cwd)
;umask=022                     ; umask for process (default None)
;priority=-1                   ; the relative start priority (default -1)
;autostart=true                ; start at supervisord start (default: true)
;startsecs=1                   ; # of secs prog must stay up to be running (def. 1)
;startretries=3                ; max # of serial start failures when starting (default 3)
;autorestart=unexpected        ; autorestart if exited after running (def: unexpected)
;exitcodes=0,2                 ; 'expected' exit codes used with autorestart (default 0,2)
;stopsignal=QUIT               ; signal used to kill process (default TERM)
;stopwaitsecs=10               ; max num secs to wait b4 SIGKILL (default 10)
;stopasgroup=false             ; send stop signal to the UNIX process group (default false)
;killasgroup=false             ; SIGKILL the UNIX process group (def false)
;user=chrism                   ; setuid to this UNIX account to run the program
;redirect_stderr=false         ; redirect_stderr=true is not allowed for eventlisteners
;stdout_logfile=/a/path        ; stdout log path, NONE for none; default AUTO
;stdout_logfile_maxbytes=1MB   ; max # logfile bytes b4 rotation (default 50MB)
;stdout_logfile_backups=10     ; # of stdout logfile backups (0 means none, default 10)
;stdout_events_enabled=false   ; emit events on stdout writes (default false)
;stderr_logfile=/a/path        ; stderr log path, NONE for none; default AUTO
;stderr_logfile_maxbytes=1MB   ; max # logfile bytes b4 rotation (default 50MB)
;stderr_logfile_backups=10     ; # of stderr logfile backups (0 means none, default 10)
;stderr_events_enabled=false   ; emit events on stderr writes (default false)
;environment=A="1",B="2"       ; process environment additions
;serverurl=AUTO                ; override serverurl computation (childutils)

; The sample group section below shows all possible group values.  Create one
; or more 'real' group: sections to create "heterogeneous" process groups.

;[group:thegroupname]
;programs=progname1,progname2  ; each refers to 'x' in [program:x] definitions
;priority=999                  ; the relative start priority (default 999)

; The [include] section can just contain the "files" setting.  This
; setting can list multiple files (separated by whitespace or
; newlines).  It can also contain wildcards.  The filenames are
; interpreted as relative to this file.  Included files *cannot*
; include files themselves.

[include]       ; 包含其它配置文件
files = /data/localData/z_home/supervisor/conf/*.conf       ; 可以指定一个或多个以.conf结束的配置文件

```


### supervisord.conf

```conf

[program:videoUpload]       ; 项目名
command=python3 main.py     ; 脚本执行命令
directory=/data/localData/z_home/videoUpload    ; 脚本目录
# autostart=true    ; supervisor启动的时候是否随着同时启动，默认True
# autorestart=true
priority=904
user=root    ; 脚本运行的用户身份 
redirect_stderr = true              ; 把stderr重定向到stdout，默认 false
stdout_logfile_maxbytes = 20MB      ; stdout日志文件大小，默认 50MB
stdout_logfile_backups = 20         ; stdout日志文件备份数
stdout_logfile = /data/localData/z_home/supervisor/log/videoUpload_stdout.log       ; 日志输出 

```


## 启动 supervisord

```sh

# 启动supervisord服务
systemctl start supervisord
/usr/bin/supervisord -c /etc/supervisord.conf

# 查看supervisord服务状态
systemctl status supervisord
/usr/bin/supervisorctl -c /etc/supervisord.conf status

# 查看监听
netstat -tunlp | grep 9001

#  查看 supervisor 的web界面
http://localhost:9001/ 

```


## 问题

现象:
* 启动服务时出现 Error: Another program is already listening on a port that one of our HTTP servers is configured to use. Shut this program down first before starting supervisord

解决:
* 解除软连接
 
  ```sh
  # 找到这个文件的软连接
  find / -name supervisor.sock

  # 然后，解除软连接
  unlink /run/supervisor.sock

  # 再重新启动就可以了
  ```