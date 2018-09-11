### 前言

	本次使用的安装方式是通过原始的安装方式，需要什么安装什么，也可以使用Anaconda来安装方便快捷，
	后面有时间的话，在写一下使用Anaconda的方式来搭建环境。
	环境及版本信息：
	阿里云新的Ubuntu16.04LTS服务器（查看服务器版本信息：lsb_release -a），
	默认Python2.7.12和Python3.5.2
	Scrapy 1.5.1 
	Django 2.0.7
	暂未考虑在虚拟环境中安装。
	生成requirements.txt: pip freeze >requirements.txt
	安装requirements.txt：pip install requirements.txt
	涉及第三方工具或者库：MongoDB、scrapyd、scrapykeeper、supervisor

### 一、安装pip3

 1. 先更新apt

    ```
    apt update
    ```

2. 安装pip3

    ```
    sudo apt install python3-pip 
    ```

3. 更新pip3

    ```
    python3 -m pip  install --upgrade pip 
    或者 pip install -U pip
    ```

    查看pip版本：使用pip3 -V会提示：

    ```
    root@iZwz9gfvwaa0qsyrd0cmupZ:~# pip3 -V
    -bash: /usr/bin/pip3: No such file or directory
    ```

    但是直接使用pip -V 

    ```
    root@iZwz9gfvwaa0qsyrd0cmupZ:~# pip -V
    pip 18.0 from /usr/local/lib/python3.5/dist-packages/pip (python 3.5)
    ```

    注意：这里自动就连接到python3.5的，我也不知道原因，希望知道的同学可以指教一下。

### 二、 安装Scrapy

直接使用pip install scrapy 会安装失败，故这里通过使用whl文件的方式来安装。

1. 下载需要的文件

   下载地址：https://www.lfd.uci.edu/~gohlke/pythonlibs/，在页面内直接搜索scrapy即可找到scrapy的whl包，然后根据需要下载对应版本的包，我们这里下载Scrapy‑1.5.1‑py2.py3‑none‑any.whl

   由于scrapy依赖twisted包，所以要需要下载twisted的whl包，我们这里下载：Twisted‑18.7.0‑cp35‑cp35m‑win_amd64.whl

   可以直接使用wget下载，也可以在Windows上下载后，上传到Ubuntu

2.  安装scrapy

   注意Scrapy‑1.5.1‑py2.py3‑none‑any.whl文件的路径

   ```
   pip install  Scrapy‑1.5.1‑py2.py3‑none‑any.whl
   ```

   很奇怪，本来以为还想需要安装Twisted包的，结果执行上面代码之后，居然没有报错，Scrapy就直接安装成功了，看来阿里云自己的镜像库确实做的不错。验证安装结果如下：

   ```
   root@iZwz9gfvwaa0qsyrd0cmupZ:~# scrapy -h
   Scrapy 1.5.1 - no active project
   
   Usage:
     scrapy <command> [options] [args]
   
   Available commands:
     bench         Run quick benchmark test
     fetch         Fetch a URL using the Scrapy downloader
     genspider     Generate new spider using pre-defined templates
     runspider     Run a self-contained spider (without creating a project)
     settings      Get settings values
     shell         Interactive scraping console
     startproject  Create new project
     version       Print Scrapy version
     view          Open URL in browser, as seen by Scrapy
   
     [ more ]      More commands available when run from project directory
   
   Use "scrapy <command> -h" to see more info about a command
   root@iZwz9gfvwaa0qsyrd0cmupZ:~# scrapy version
   Scrapy 1.5.1
   ```

### 三、安装Scrapyd

   [官方文档](https://scrapyd.readthedocs.io/en/stable/install.html)有如下一段话：

   > 如果您计划在Ubuntu中部署Scrapyd，Scrapyd会附带官方Ubuntu软件包（见下文），以便将其安装为系统服务，从而简化管理工作。 
   >
   > Scrapyd附带了正式的Ubuntu软件包，可以在您的Ubuntu服务器中使用。它们在Scrapy的相同APT存储库中提供，可以按照Scrapy Ubuntu软件包中的描述添加。

   在有桌面的Ubuntu系统中有个应用商店，上面的意思应该是直接可以在应用上面里面搜索安装。但是我们这里服务器一般是没有界面的，所以也试一下看看能不能直接使用：`apt-get install scrapyd`安装成功.

   然而并不行（摊手）：

   ```
   root@iZwz9gfvwaa0qsyrd0cmupZ:~# apt install scrapyd
   Reading package lists... Done
   Building dependency tree       
   Reading state information... Done
   E: Unable to locate package scrapyd
   ```

   看来还是得使用`pip install scrapyd`来进行安装，果然这种方式安装就成功了。

   ```
   ...
   Installing collected packages: scrapyd
   Successfully installed scrapyd-1.2.0
   ```

   验证一下：

   ```
   root@iZwz9gfvwaa0qsyrd0cmupZ:~# scrapyd
   2018-08-31T15:30:20+0800 [-] Loading /usr/local/lib/python3.5/dist-packages/scrapyd/txapp.py...
   2018-08-31T15:30:20+0800 [-] Scrapyd web console available at http://127.0.0.1:6800/
   2018-08-31T15:30:20+0800 [-] Loaded.
   2018-08-31T15:30:20+0800 [twisted.scripts._twistd_unix.UnixAppLogger#info] twistd 18.7.0 (/usr/bin/python3 3.
   5.2) starting up.2018-08-31T15:30:20+0800 [twisted.scripts._twistd_unix.UnixAppLogger#info] reactor class: twisted.internet.ep
   ollreactor.EPollReactor.2018-08-31T15:30:20+0800 [-] Site starting on 6800
   2018-08-31T15:30:20+0800 [twisted.web.server.Site#info] Starting factory <twisted.web.server.Site object at 0
   x7ff075e2ea20>2018-08-31T15:30:20+0800 [Launcher] Scrapyd 1.2.0 started: max_proc=4, runner='scrapyd.runner'
   ```

   通过pip安装的scrapyd，默认配置文件在`/usr/local/lib/python3.5/dist-packages/scrapyd/default_scrapyd.conf`

   为了服务器外部能访问，需要将配置文件default_scrapyd.conf中的bind_address修改为

    `bind_address = 0.0.0.0`,当然云服务器的6800端口也要打开才行。

### 四、部署Scrapy项目

   重要到了部署项目这一步了。

   根据官方文档有两种方式：

   1. 通过addversion.json的请求部署，执行下面的请求指令就可以了，其中的参数根据实际工程修改

      ```
      curl http://localhost:6800/addversion.json -F project=myproject -F version=r23 -F egg=@myproject.egg
      ```

2. 通过scrapyd-client部署，实际上就是借用工具执行第一种方式而已。

3. 安装scrapyd-client:

   ```
   pip install scrapyd-client
   ```

4. 修改Scrapy项目的配置文件scrapy.cfg

   ```
   [deploy]
   url = http://localhost:6800/
   project = demoproject
   ```

5. 上传Scrapy项目到服务器，也可以直接通过git下载比较方便，这样后续迭代更新也会比较方便一些。

6. 启动scrapyd,这里必须要先启动scrpyd,否则部署会失败。

7. scrapyd-deploy部署：切换路径到scrapy.cfg文件所在目录然后执行：`scrapyd-deploy` 

   如果部署成功的会看到一个json文件，内容如下：

   ```
   root@iZwz9gfvwaa0qsyrd0cmupZ:~/GeekParity/geekparity# scrapyd-deploy
   Packing version 1535706730
   Deploying to project "geekparity" in http://parity.geekview.cn:6800/addversion.json
   Server response (200):
   {"node_name": "iZwz9gfvwaa0qsyrd0cmupZ", "message": "Traceback (most recent call last):\n  File \"/usr/lib/py
   thon3.5/runpy.py\", line 184, in _run_module_as_main\n    \"__main__\", mod_spec)\n  File \"/usr/lib/python3.5/runpy.py\", line 85, in _run_code\n    exec(code, run_globals)\n  File \"/usr/local/lib/python3.5/dist-packages/scrapyd/runner.py\", line 40, in <module>\n    main()\n  File \"/usr/local/lib/python3.5/dist-packages/scrapyd/runner.py\", line 37, in main\n    execute()\n  File \"/usr/local/lib/python3.5/dist-packages/scrapy/cmdline.py\", line 129, in execute\n    cmds = _get_commands_dict(settings, inproject)\n  File \"/usr/local/lib/python3.5/dist-packages/scrapy/cmdline.py\", line 51, in _get_commands_dict\n    cmds.update(_get_commands_from_module(cmds_module, inproject))\n  File \"/usr/local/lib/python3.5/dist-packages/scrapy/cmdline.py\", line 30, in _get_commands_from_module\n    for cmd in _iter_command_classes(module):\n  File \"/usr/local/lib/python3.5/dist-packages/scrapy/cmdline.py\", line 20, in _iter_command_classes\n    for module in walk_modules(module_name):\n  File \"/usr/local/lib/python3.5/dist-packages/scrapy/utils/misc.py\", line 63, in walk_modules\n    mod = import_module(path)\n  File \"/usr/lib/python3.5/importlib/__init__.py\", line 126, in import_module\n    return _bootstrap._gcd_import(name[level:], package, level)\n  File \"<frozen importlib._bootstrap>\", line 986, in _gcd_import\n  File \"<frozen importlib._bootstrap>\", line 969, in _find_and_load\n  File \"<frozen importlib._bootstrap>\", line 956, in _find_and_load_unlocked\nImportError: No module named 'geekparity.commands'\n", "status": "error"}
   ```

   版本号默认为当前时间戳，可自定义`scrapyd-deploy --version 201808311719`,版本号必须为纯数字，否则会报错。

   回到根目录，能够看到/root/eggs/geekparity目录下包含一个egg文件。

   五、运行Spider:

   curl http://localhost:6800/schedule.json -d project=demoproject -d spider=newsspider



   相关博文：https://www.colabug.com/2987685.html

### 五、安装MongoDB

	[官方文档](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/)
	
	默认配置文件路径：/etc/mongod.conf

​	**这样安装，重启服务器，MongoDB不会自动启动**

### 六、定时抓取设置、保留进程

如果成功走完上面所有的流程，那么已经可以直接跑，并且使用了，但是还有两个问题需要处理：

- 不能让爬虫定时抓取
- 命令窗口也不能关闭，关闭之后服务也关闭了（没有进程守护）

1. ##### 安装spiderkeeper（[GitHub地址](https://github.com/DormyMo/SpiderKeeper)）：

   这个库，可以让你跟pyspider一样在网页上设置scrapy项目的配置，定时抓取启动等等，这样就解决了我们的声明的第一个问题。

   ```
    pip install spiderkeeper
   ```

   spiderkeeper还是依赖scrapyd的，所以要先启动scrapyd，否则spiderkeeper也不能控制scrapy.

   具体使用根据GitHub上面的文档，完全可以搞定。

2. 安装supervisor

   由于supervisor不支持python3.x的任何版本，所以需要使用python2.x来安装，

   ```
   pip2 install supervisor
   ```

   如果在安装目录和etc下面找不到supervisord_conf文件，那就自己生成一个：

   ```
   echo_supervisord_conf > /etc/supervisord.conf
   ```

   启动supervisor

   ```
   supervisord -c /etc/supervisord.conf
   ```

   编辑supervisord.conf 用如下代码覆盖全部默认代码（我已经将注释都删除掉）：

   ```
   [unix_http_server]
   file=/tmp/supervisor.sock   ; the path to the socket file
   
   [inet_http_server]         ; inet (TCP) server disabled by default
   port=127.0.0.1:9001        ; ip_address:port specifier, *:port for all iface
   username=user              ; default is no username (open server)
   password=123               ; default is no password (open server)
   
   [supervisord]
   logfile=/tmp/supervisord.log ; main log file; default $CWD/supervisord.log
   logfile_maxbytes=50MB        ; max main logfile bytes b4 rotation; default 50MB
   logfile_backups=10           ; # of main logfile backups; 0 means none, default 10
   loglevel=info                ; log level; default info; others: debug,warn,trace
   pidfile=/tmp/supervisord.pid ; supervisord pidfile; default supervisord.pid
   nodaemon=false               ; start in foreground if true; default false
   minfds=1024                  ; min. avail startup file descriptors; default 1024
   minprocs=200                 ; min. avail process descriptors;default 200
   
   [rpcinterface:supervisor]
   supervisor.rpcinterface_factory = supervisor.rpcinterface:make_main_rpcinterface
   
   [supervisorctl]
   serverurl=unix:///tmp/supervisor.sock ; use a unix:// URL  for a unix socket
   
   [program:scrapyd]
   command=scrapyd              ; the program (relative uses PATH, can take args)
   process_name=%(program_name)s ; process_name expr (default %(program_name)s)
   numprocs=1                    ; number of processes copies to start (def 1)
   directory=/                ; directory to cwd to before exec (def no cwd)
   priority=1                  ; the relative start priority (default 999)
   autostart=true                ; start at supervisord start (default: true)
   startretries=10	; max # of serial start failures when starting (default 3)
   autorestart=true        ; when to restart if exited after running (def: unexpected)
   exitcodes=0                 ; 'expected' exit codes used with autorestart (default 0,2)
   stopsignal=KILL               ; signal used to kill process (default TERM)
   stopwaitsecs=10               ; max num secs to wait b4 SIGKILL (default 10)
   
   [program:scrapykeeper]
   command=spiderkeeper --server=http://localhost:6800 ; the program (relative uses PATH, can take args)
   process_name=%(program_name)s ; process_name expr (default %(program_name)s)
   numprocs=1                    ; number of processes copies to start (def 1)
   directory=/                ; directory to cwd to before exec (def no cwd)
   priority=1                  ; the relative start priority (default 999)
   autostart=true                ; start at supervisord start (default: true)
   startretries=10	; max # of serial start failures when starting (default 3)
   autorestart=true        ; when to restart if exited after running (def: unexpected)
   exitcodes=0                 ; 'expected' exit codes used with autorestart (default 0,2)
   stopsignal=KILL               ; signal used to kill process (default TERM)
   stopwaitsecs=10               ; max num secs to wait b4 SIGKILL (default 10)
   ```

   重启supervisor：

   ```
   root@iZwz9gfvwaa0qsyrd0cmupZ:~# supervisorctl reload
   Restarted supervisord
   ```

   查看启动情况：

   ```
   root@iZwz9gfvwaa0qsyrd0cmupZ:~# supervisorctl status
   scrapyd                          RUNNING   pid 20638, uptime 0:00:04
   scrapykeeper                     RUNNING   pid 20637, uptime 0:00:04
   ```

3. 现在就可以在登录scrapykeeper（http://pa:5000）上面对scrapy进行配置了。



OK，终于啰里啰嗦的部署完了。整个流程走下来，相对于Java的部署来说，确实有些麻烦。