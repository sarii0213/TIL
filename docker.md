# ð³ Docker

## ããä½¿ãã³ãã³ã
- `docker compose up` ï¼ãã³ã³ããèµ·åï¼Ctrl+cã§çµäºï¼
- `docker compose up -d`ã: ããã¯ã°ã©ã¦ã³ãã§èµ·å
- `docker ps`ã: èµ·åãã¦ããã³ã³ããä¸è¦§
- `docker attach <CONTAINER_NAME>`ã: Ctrl+ p -> qãã§dettachãexitã§åæ­¢
- `docker compose exec <SERVICE> <COMMAND>`ã: æå®ããservice ï¼docker-compose.ymlã«è¨è¼ãwebãªã©) ã®ä¸­ã§ãæå®ããcommand ï¼bashãªã©ï¼ ãå®è¡
- `docker stop <CONTAINER_NAME>`ï¼ã³ã³ããåæ­¢
- `docker compose down`: ã³ã³ããåæ­¢ & `docker-compose up`ã§ä½æããã³ã³ããããããã¯ã¼ã¯ãããªã¥ã¼ã ãã¤ã¡ã¼ã¸ãåé¤
<br>


## Dockerã«ã¤ãã¦çè§£
### åè
[å®è·µ Docker - ã½ããã¦ã§ã¢ã¨ã³ã¸ãã¢ã®ãDocker ããããããªãããçµããã«ããæ¬](https://zenn.dev/suzuki_hoge/books/2022-03-docker-practice-8ae36c33424b59/viewer/1-1-readme)
### ãµã¼ãä»®æ³åã¨ã¯
- 1å°ã®ãã·ã³ã«ããããã¯ã¼ã¯ãã¹ãã¬ã¼ã¸ãä»®æ³çã«ç¨æãã¦ãè¤æ°ã®ç°ãªããµã¼ããåãã¦ããããã«è¦ããããæè¡
- ç©ççãªãµã¼ãã¨ã½ããã¦ã§ã¢ã®éã«ä»®æ³çãªã½ããã¦ã§ã¢ãæããã¨ã§å®ç¾
- ä»®æ³åï¼ç¨®é¡
  - ãã¹ãåä»®æ³åï¼ãã¹ãOSï¼Mac OSï¼ã«ã¤ã³ã¹ãã¼ã«ãããã²ã¹ãOS(Ubuntu)ãç®¡çãã
  - ãã¤ãã¼ãã¤ã¶ã¼åä»®æ³åï¼ãã¼ãã¦ã§ã¢(MacBook)ã«ã¤ã³ã¹ãã¼ã«ãããã²ã¹ãOS(CentOS)ãç®¡çãã
  - ã³ã³ããåä»®æ³åï¼ ãã¹ãOSã«ã¤ã³ã¹ãã¼ã«ãããã¢ããªã±ã¼ã·ã§ã³ï¼php, mail system, mysqlï¼ãç®¡çããï¼ï¼ã³ã³ããï¼ã¢ããªã±ã¼ã·ã§ã³ï¼
- ã³ã³ããåä»®æ³åã®ç¹å¾´
  - èµ·åãæ©ãï¼ã²ã¹ãOSããªãããï¼
  - ããã­ã¤ãããããï¼ããã­ã¤åãã³ã³ããåãªãæéããããããã§ãªãã¦ããï¼ã³ã³ããï¼ã¢ããªã±ã¼ã·ã§ã³ãªã®ã§ç§»è¡ã®æéå°ãªãï¼
  - ãã¹ãOSã®éããã³ã³ããã«å½±é¿ä¸ãããã¨ãï¼ã³ã³ããã®ã«ã¼ãã«ï¼ãã¹ããã·ã³ã®ã«ã¼ãã«ãMacBook Intelã¨M1ã§è¡çªããããUbuntuã³ã³ããã¨Windowsã³ã³ãããå±å­ä¸å¯ã ã£ããï¼

### Dockerã¨ã¯
- ã³ã³ããåä»®æ³åãç¨ãã¦ã¢ããªã±ã¼ã·ã§ã³ã®éçºãéç½®ãè¡ãããã®ãã©ãããã©ã¼ã 
- Docker Engine: ã³ã³ããåä»®æ³åã½ããã¦ã§ã¢ï¼ã³ã³ãããè¼ããé¨åï¼ãLinuxã§åãã
- Docker CLI: ã³ãã³ããDocker Engineã«æä¾ããã`docker run`ã®ãããª`docker`ã§ã¯ãã¾ãã³ãã³ãã§ Docker ã«å½ä»¤ãããã
- Docker Desktop: Windows/Macã§Dockerãä½¿ãã¨ãã«ã¤ã³ã¹ãã¼ã«ããDockerä¸å¼ãå¥ã£ãGUIã¢ããªã±ã¼ã·ã§ã³ãDocker EngineãLinuxã®ã«ã¼ãã«ãå«ã¾ãã¦ãããããLinuxä»¥å¤ã®OSã§ãDocker Engineãåãããã¨ãã§ãããï¼Docker ComposeãKubernetesãå«ãï¼
- Docker Compose: Docker CLIï¼`docker`ã³ãã³ãç¾¤ï¼ãã¾ã¨ãã¦å®è¡ãã¦ãããä¾¿å©ãªãã¼ã«ãè¤éãªã³ãã³ãããYamlãã¡ã¤ã«ãæ¸ããã¨ã§å®ç¾ã§ããã
- Docker Hub: Dockerã®ã¤ã¡ã¼ã¸ã¬ã¸ã¹ããªã§ããSaasãµã¼ãã¹ãã¤ã¡ã¼ã¸ã®GitHubã®ãããªãã®ãã¤ã³ã¹ãã¼ã«ããããã¢ããã­ã¼ããããã
- ããã­ã¤æ
  - ECS/GKE:ECS(Amazon Elastic Container Service)ã¨GKE(Google Kubernetes Engine)ã¯ãã³ã³ããç®¡çãµã¼ãã¹ãDocker Engineã®å¥ã£ãLinuxã®ãã¨ã§ãã­ã¼ã«ã«ç°å¢ã§ä½¿ã£ãã³ã³ããããã®ã¾ã¾ããã­ã¤ã§ããå ´æã
  - ECR/GCR: ECR(Amazon Elastic Container Registory)ã¨GCR(Google Container Registory)ã¯ãéå¬éã®ã¤ã¡ã¼ã¸ã®ã¬ã¸ã¹ããªããã©ã¤ãã¼ããªDocker Hubãåç¨ãµã¼ãã¹ãå¬éããããªããã©ãã¬ã¸ã¹ããªã«ç»é²ããªãã¨ããã­ã¤ã§ããªãå ´åãªã©ã«å©ç¨ã
- ã³ã³ããéç¨ï¼ã¤ã³ãã©æ§ç¯ï¼
  - Kubernetes: å¤æ°ã®ã³ã³ãããç®¡çãããªã¼ã±ã¹ãã¬ã¼ã·ã§ã³ã½ããã¦ã§ã¢ã  

### çå
- ã³ã³ããã¯ãã¹ãOSã®ã«ã¼ãã«ãå©ç¨ããï¼ã§ãMac OSã®ã«ã¼ãã«ã¯XNUã§Linuxã§ã¯ãªããã­ï¼Docker Desktopã«å¥ã£ã¦ããLinuxã«ã¼ãã«ã¨ããã¹ãOSã®ã«ã¼ãã«ã¯ã©ãä½ç¨ãããã®ï¼

### Dockerã®åºæ¬è¦ç´ ï¼ã¤
- ã³ã³ãã
- ã¤ã¡ã¼ã¸
- Dockerfile

#### ã³ã³ãã
- ç¹å®ã®ã³ãã³ããå®è¡ããããã«ä½ããã ãã¹ããã·ã³ä¸ã®éé¢ãããé åï¼Linuxã®Namespaceã«ãããPIDã¯ãã¹ããã·ã³ã®ãã®ã¨è¡çªããªãï¼
- ãã¹ããã·ã³ã®ï¼ãã­ã»ã¹ã«ãããªããä»®æ³ãµã¼ãã§ã¯ãªãã
- ã¤ã¡ã¼ã¸ããã¨ã«ä½ããã
- Dockerã®CLIãAPIãä½¿ã£ã¦ãçæã»èµ·åã»åæ­¢ãè¡ãã
- è¤æ°ã®ã³ã³ããã¯äºãã«ç¬ç«ããå½±é¿ãä¸ããªã
- Docker Engineã®ä¸ãªãã­ã¼ã«ã«ãã·ã³ã§ãä»®æ³ãã·ã³ã§ãã¯ã©ã¦ãç°å¢ã§ãåããã

### ã¤ã¡ã¼ã¸
- ã³ã³ããã®å®è¡ã«å¿è¦ãªããã±ã¼ã¸ã§ããã¡ã¤ã«ãã¡ã¿æå ±ãéãããã®
- è¤æ°ã®ã¬ã¤ã¤ã¼ãããªã
- ã¤ã¡ã¼ã¸ã«å«ã¾ããæå ±
  -ããã¼ã¹ã¯ãªã«ããï¼ex. Ubuntuï¼
  -ããªã«ãã¤ã³ã¹ãã¼ã«ãã¦ããã (ex. PHP)
  -ãç°å¢å¤æ°ã¯ã©ããªã£ã¦ããã
  -ãã©ãããè¨­å®ãã¡ã¤ã«ãè¨­ç½®ãã¦ããã (ex. php.ini)
  -ãããã©ã«ãå½ä»¤ã¯ãªã«ã  
-ãDocker Hubã§å¬éããã¦ãããã®ãããæ¤ç´¢ãã¦é¸ã¶
  -ãï¼ï¼`image pull`ã¯ããã`container run`ã§`<image>`ã«`REPOSITORY:TAG`æå®ã`TAG`çç¥ï¼`latest`ï¼
  -ãTagãã¼ã¸ã®ãã¼ã¸ã§ã³ãé¸ãã ç»é¢ã§è¦ããã®ã¯Dockerfileã®ä¸­èº«ã§ã¯ãªããDockerfileã«ãã£ã¦ç©ã¿ä¸ããããã¬ã¤ã¤ã¼ã®æå ±

### Dockerfile
- æ¢å­ã®ã¤ã¡ã¼ã¸ã«ã¬ã¤ã¤ã¼ãããã«ç©ã¿éã­ãããã®ãã­ã¹ããã¡ã¤ã«
- å¬éããã¦ããã¤ã¡ã¼ã¸(ex. Ubuntu&PHP&.ini)ã«Dockerfileã§ã¬ã¤ã¤ã¼(ex. Git)ãä¹ããæã
- GitHubã§å±æãããªã©ãã¦ããã¼ã ã¡ã³ãã¼ã§åä¸ã®ã³ã³ãããæ§ç¯ã§ãã
- Dockerfileã®æç¨æ§
  - ã³ã³ããåã§è¡ã£ãæä½ã¯ãã³ã³ããçµäºã¨ã¨ãã«å¨ã¦ãªãã£ããã¨ã«ãªã
  - Docker Hubã«ããã¤ã¡ã¼ã¸ã¯ãã¬ã¤ã¤ã¼ãæä½éããç©ã¿ä¸ãã£ã¦ããªã
  - å¬å¼ã¤ã¡ã¼ã¸ã§ã¯ååãªã»ããã¢ãããå¾ãããªãå ´åã«ããããããå¿è¦ãªã»ããã¢ãããæ¸ã¾ããã¤ã¡ã¼ã¸ãèªåã§ä½æããã®ãâ 

#### Dockerfile ã®å½ä»¤
- `FROM`: ãã¼ã¹ã¤ã¡ã¼ã¸ãæå®
- `RUN`: ä»»æã®ã³ãã³ããå®è¡ãã¬ã¤ã¤ã¼ãç¢ºå®
  - ä¾ï¼ `apt-get install mariadb-client tzdata libvips`:MariaDB, ã¿ã¤ã ã¾ã¼ã³DB, libvipsãã¤ã³ã¹ãã¼ã«
- `COPY`: ãã¹ããã·ã³ã®ãã¡ã¤ã«ãã¤ã¡ã¼ã¸ã«è¿½å 
- `CMD`: ããã©ã«ãå½ä»¤ãæå®


### Docker åºæ¬ã³ãã³ããï¼æåã®`docker`çç¥ï¼
#### container æä½
- `container run [option] <image> [command]`: ã¤ã¡ã¼ã¸ããã³ã³ããèµ·åãï¼=`image pull`&`container create`&`container start`ï¼
  - `--rm`ãªãã·ã§ã³ï¼ ã³ã³ãããåæ­¢ããã¨ãèªååé¤
  - `--detach`:ããã¯ã°ã©ã¦ã³ãã§å®è¡
  - `--name`: ã³ã³ããåæå®
  - `--interactive`: ã³ã³ããã®æ¨æºå¥åã«æ¥ç¶ï¼å¯¾è©±æä½ï¼
  - `--tty`: çä¼¼ã¿ã¼ããã«ãå²ãå½ã¦ãï¼å¯¾è©±æä½ï¼ãï¼tty: æ¥ç¶ç«¯æ«ã®åæ­ã®ãããªãã®ï¼
  - `--platform`: Docker Hubã§ã®ãµãã¼ããã©ãããã©ã¼ã ã¨ç°ãªããã©ãããã©ã¼ã ãæå®ãããæï¼M1Macãªãå¿é ï¼
  - `--env`: ã³ã³ããã«ç°å¢å¤æ°ãè¨­å®
  - `--mount type=<bind/volume>, src=<dir_of_host_machine/volume_name>, dst=<dir_of_container>`:ãã¦ã³ãããï¼ä¸å¯§ã«æå®ï¼Docker Composeã«è»¢ç¨ããããï¼
  - `--volume`:ãã¦ã³ãããï¼ç­ãæå®ï¼
  - `--publishã<host machine>:<container>`: ã³ã³ããã®ãã¼ãããã¹ããã·ã³ã«å¬éãã
  - `--network`: ã³ã³ããããããã¯ã¼ã¯ã«æ¥ç¶
  - `--network-alias`: ã³ã³ããã«ãããã¯ã¼ã¯åã§ã®ã¨ã¤ãªã¢ã¹ãè¨­å®
  - `[command]`: ããã©ã«ãã®å½ä»¤ã§ã¯ãªããä»»æã®å½ä»¤ãå®è¡ããã

- `container execã[option] <container> <command>`: èµ·åä¸­ã®ã³ã³ããã«å½ä»¤ãéãï¼ã³ã³ããåã§å®è¡ããLinuxã³ãã³ãï¼ãã
  - `container run`ã§ã³ãã³ãå®è¡ããã®ã¨ã¯éãåãã«ãªãã
    - `container run ubuntu:20.04 cat hoge.txt`ã¨ï¼åº¦å®è¡ããã¨ãåãã¤ã¡ã¼ã¸ããç°ãªãã³ã³ãããèµ·åãã
    - å¯¾è©±æä½(`bash`ã³ãã³ãå®è¡)ã«ã¯ã`container run`æã¨åæ§ã®ãªãã·ã§ã³ãå¿è¦(`-it`)
  -ãã³ã³ããã®ä¸­ã«ããã­ã°ãèª¿ã¹ãããDockerfileãæ¸ãåã«bashã§ã¤ã³ã¹ãã¼ã«ã³ãã³ããè©¦ãæã¡ããããmysqlãç´æ¥æä½ãããã§ãã

- `container ls`: ã³ã³ããä¸è¦§
- `container stop <container>`: ã³ã³ããåæ­¢
- `container rm <container>`: ã³ã³ããåé¤ãï¼`-f`ã§åæ­¢ï¼åé¤ï¼

#### image æä½
- `image build [option] <path>`: Dockerfileããã¤ã¡ã¼ã¸ä½æï¼ãã®å¾ã`container run`ï¼
  - `--file`ãªãã·ã§ã³ï¼è¤æ°ã®Dockerfileãä½¿ãåããã¨ãï¼`./Dockerfile`ä»¥å¤ã®Dockerfileæå®æï¼ã«ãDockerfileãæå®
  - `--tag`: äººéãææ¡ããããããã«ããã«ãçµæã«ã¿ã°ãã¤ãã ãï¼`REPOSITORY:TAG`ã®å½¢ï¼
  - `<path>`: `COPY`ã§ä½¿ããã¡ã¤ã«æå®æã®ç¸å¯¾ãã¹
- `image ls`: ã¤ã¡ã¼ã¸ä¸è¦§(`image build` or `container run`ã®éç¨ã§åå¾ãããã¤ã¡ã¼ã¸ãã¡)
- `image history [option] <image>`: ã¤ã¡ã¼ã¸ã®ã¬ã¤ã¤ã¼ãç¢ºèª

#### compose æä½
- `compose up`: ã³ãã³ãã®æé æ¸(Yamlãã¡ã¤ã«)ã«æ¸ãããã³ãã³ã(`container run <image>`..)ãã¾ã¨ãã¦å®è¡ãã¦èµ·å

### ã³ã³ããã«ã¤ãã¦
- ã³ã³ããã¯ãã¡ã¤ã³ãã­ã»ã¹ï¼`PID`=1ï¼ãå®è¡ããããã«èµ·åãããããï¼ã¡ã¤ã³ãã­ã»ã¹ï¼ããã©ã«ãå½ä»¤ï¼ï¼
- ã³ã³ãããåæ­¢ããã®ã¯ã
  - ã³ã³ãããåæ­¢ãããã¨ã(`container stop`) or 
  - ã¡ã¤ã³ãã­ã»ã¹ãçµäºããæï¼`container run nginx:1.21 ls /etc/nginx`ã ã¨å³æçµäºï¼

### Dockerfileãã¼ã­ããæ¸ãå ´å
- ããã¼ã¹ã¤ã¡ã¼ã¸ããã èµ·åãã¦`bash`ã§è©¦ããï¼Dockerfileã¯æ¸ããã`container run --rm ruby:3.1.1 bash`ã¨æã¤ï¼
- ãããã§åããã³ãã³ãã Dockerfile ã«ãã¼ã¹ããããã¨ãããµã¤ã¯ã«ã«

### ããªã¥ã¼ã 
- ã³ã³ããã®ãã¼ã¿ãã³ã³ããåé¤ã¨ã¨ãã«æ¶å¤±ãããªãããã®ãã®
- ã³ã³ããåã®ãã¡ã¤ã«ããã¹ããã·ã³ä¸ã§Dockerãç®¡çãã¦ãããä»çµã¿
- ãã¹ããã·ã³å´ã®ã©ãã«ä¿å­ããã¦ãããã¯é¢å¿ããªããã¨ã«ãããã¼ã¿ãæ°¸ç¶åãããæã«æç¨ï¼DBã³ã³ãããªã©ï¼
- ã³ã³ããã§ã®å¤æ´ããã¹ããã·ã³ã«å½±é¿ããªã
- `docker volume create [option]`
  - `--name`:ããªã¥ã¼ã åãæå®
- `docker volume inspect <volume name>`: ããªã¥ã¼ã ã®è©³ç´°ç¢ºèª
  - `Mountpoint`:ãã¼ã¿ã®ããå ´æã®å®ä½ãDocker Desktopã®Linuxä¸ã®ãã¹ã®ãããç´æ¥èª­ã¿æ¸ãã¯ã§ããªãã 

### ãã¤ã³ããã¦ã³ã
- ãã¹ããã·ã³ã®ä»»æã®ãã£ã¬ã¯ããªãã³ã³ããã«ãã¦ã³ãããä»çµã¿
- ãã¹ããã·ã³ã¨ã³ã³ããåæ¹ããã¡ã¤ã«ã®å¤æ´ã«é¢å¿ãããå ´åã«æå¹ï¼ã½ã¼ã¹ã³ã¼ãã®å±æãªã©ã«æ´»ç¨ï¼
- ã³ã³ããã§ã®æä½ï¼åé¤ãªã©ï¼ããã¹ããã·ã³ã«æ³¢åããããããªãã¹ãããªã¥ã¼ã ãä½¿ã£ã¦ãã©ããã¦ãå¿è¦ãªããã¤ã³ããã¦ã³ããä½¿ãã®ãâ
- å¦çãéããªãããã
  - å¦çæéç­ç¸®ã®ããã«ããã¦ã³ããªãã·ã§ã³ãå©ç¨ããã¨ãã
  - ãããªãã®æ§è½ãæ±ãã¤ã¤ããã¹ãå´ã®å¤æ´ãã³ã³ããã«åæ ãããå ´åã¯`cached`ãªãã·ã§ã³ãä½¿ãâ 
  - [detail](https://gist.github.com/yallop/d4af9dd1bb33ae61d48adf86692cdf9e)

#### COPYã¨ãã¤ã³ããã¦ã³ãã®ä½¿ãåã
- COPYã¨ãã¤ã³ããã¦ã³ãã¯ã©ã¡ãããã¹ããã·ã³ã®ãã¡ã¤ã«ãã³ã³ããã§æ±ããããã«ããæ©è½
- COPYï¼to ã¤ã¡ã¼ã¸ï¼
  - `image build`ãããã¿ã¤ãã³ã°ã§ãã¡ã¤ã«ãå«ãããããã³ã³ãããèµ·åããã°ãã¡ã¤ã«ãå­å¨ãã
  - åãã¡ã¤ã«ã®å¤æ´ãè¡ãªã£ã¦ãã³ã³ããã«ã¯åæ ãããªãããã`image build`ã®åå®è¡ãå¿è¦ã«
  - ç¨éï¼è¨­å®ãã¡ã¤ã«ãæ¬çªããã­ã¤æã®ã½ã¼ã¹ã³ã¼ããªã©
- ãã¤ã³ããã¦ã³ã(to ã³ã³ãã)
  - ãã¹ããã·ã³ã§ãã¡ã¤ã«ãå¤æ´ããã¨ã³ã³ããã«å³æå½±é¿
  - ç¨éï¼éçºæã®ã½ã¼ã¹ã³ã¼ãï¼ãã¹ããã·ã³ã§å¤æ´ããããã³ã³ããã«å³æåæ ããããï¼ãåæåã¯ã¨ãªï¼ã¤ã¡ã¼ã¸éå¸æç¹ã§ã¯ç¨æã§ããªãï¼

  - ç¨éï¼éçºæã®ã½ã¼ã¹ã³ã¼ãï¼ãã¹ããã·ã³ã§å¤æ´ããããã³ã³ããã«å³æåæ ããããï¼ãåæåã¯ã¨ãªï¼ã¤ã¡ã¼ã¸éå¸æç¹ã§ã¯ç¨æã§ããªãï¼

### çå
- ã¤ã³ã¹ã¿ã¯ã­ã¼ã³ã¯ãã¤ã³ããã¦ã³ããã¦ããï¼ã½ã¼ã¹ã³ã¼ãã¯ãã¤ã³ããã¦ã³ããã¦ãããã ãã­ï¼ã©ãã§ç¢ºèªã§ããï¼
- ããªã¥ã¼ã ãå©ç¨ãã¦ããï¼docker-compose.ymlã®è¨è¿°ã§ã¯ãmysql_dataï¼DBã®ãã¼ã¿ï¼ã¨bundleï¼gemã®ãã¼ã¿ï¼ã¨ããããªã¥ã¼ã åã§ããªã¥ã¼ã ãä½æãã¦ããã

### ãã¼ãã®å¬é
- ã³ã³ããã«ãã¹ããã·ã³ããã¢ã¯ã»ã¹ãããããï¼ãã¹ããã·ã³ã¼ã³ã³ããã®éä¿¡ï¼
- ã³ã³ããã¯ãã¹ããã·ã³ããéé¢ããã¦ãããã³ã³ããåã§Webãµã¼ããDBãµã¼ããèµ·åãã¦ããã®ã¾ã¾ã§ã¯ãã¹ããã·ã³ããã¢ã¯ã»ã¹ãããã¨ã¯ã§ããªã
- `--publish host-machine:container`ã®ã`host-machine`å´ã®ãã¼ãã¯èµ·åããäººãèªç±ã«æ±ºããããï¼1024æªæºã¯ç¹æ¨©ãã¼ããªã®ã§NGï¼


### ãããã¯ã¼ã¯
- ã³ã³ããåå£«ã®éä¿¡ãå¯è½ã«ãã
- ãããã¯ã¼ã¯ãã©ã¤ãï¼
  - ããªãã¸ãããã¯ã¼ã¯ï¼åä¸ã®Docker Engineä¸ã®ã³ã³ãããäºãã«éä¿¡ãããå ´åã«å©ç¨
  - ãªã¼ãã¼ã¬ã¤ãããã¯ã¼ã¯ï¼ç°ãªãDocker Engineä¸ã®ã³ã³ãããäºãã«éä¿¡ãããå ´åã«å©ç¨ï¼ã­ã¼ã«ã«éçºã§ã¯èµ·ããå¾ãªãï¼
- ããã©ã«ãããªãã¸ãããã¯ã¼ã¯ï¼
  - ãã¹ã¦ã®ã³ã³ããéããªã³ã¯ããæä½ãå¿è¦
  - ã³ã³ããéã®éä¿¡ã¯IPã¢ãã¬ã¹ã§è¡ã
  - Docker Engineä¸ã®ãã¹ã¦ã®ã³ã³ããã«æ¥ç¶ã§ãã¦ãã¾ã
- ã¦ã¼ã¶å®ç¾©ããªãã¸ãããã¯ã¼ã¯
  - ç¸äºéä¿¡ãã§ããããã«ããã«ã¯åããããã¯ã¼ã¯ãå²ãå½ã¦ãã ãã§ãã
  - ã³ã³ããéã§èªåçã«DNSè§£æ±ºãè¡ãã
  - éä¿¡ã§ããã³ã³ãããåä¸ãããã¯ã¼ã¯ä¸ã®ã³ã³ããã«éãããéé¢åº¦ãä¸ãã
- `docker network create [option] <name>`ï¼ã¦ã¼ã¶å®ç¾©ãããã¯ã¼ã¯ãä½æ

### Docker Compose
- `image build`ã®æé ã¨`container run`ã®æé ãç½®ãæãã
- ã³ã³ããã®èµ·åãæ¥½ã«ãããã¼ã«ã§ãããã¤ã¡ã¼ã¸ãä½ããã¼ã«ã§ã¯ãªãã®ã§ãDockerfileã¯å¿é 
- `docker compose up [option]`
  - `--detach`: ããã¯ã°ã©ã¦ã³ãã§å®è¡ï¼ã¿ã¼ããã«ãåºã¾ãã®ãé¿ããï¼
  - `--build`: ã³ã³ãããèµ·åããåã«ã¤ã¡ã¼ã¸ããã«ãï¼Dockerfileã®å¤æ´ãåæ ï¼
- `docker compose down [option]`ï¼ãµã¼ãã¹ãåæ­¢

#### docker-compose.yml
- `services:`ã®ç´ä¸ã«ãç®¡çãããµã¼ãã¹ï¼ï¼ã³ã³ããï¼ãä»»æã®ååã§è¨è¿°
- `container_name:`ã«ã¯`container run --name`ã§ããã³ã³ããåãç½®ãæãã
- `build:`ã¯ã`image build --file dockerfile <path>`ã§æå®ãã¦ãã`dockerfile`ã`build.dockerfile:`ã«ã`<path>`ã`build.context:`ã§ç½®ãæãã
- `image:`ã¯ã`container run <image>`ã§æå®ãã¦ããã¤ã¡ã¼ã¸ãç½®ãæã
- ã¤ã¡ã¼ã¸ã®ãã«ãã¯ã`compose up`ããããã¤ã¡ã¼ã¸ãå­å¨ããªãã£ãå ´å or `compose up`ã«`--build`ãªãã·ã§ã³ãã¤ãã¦å®è¡ããå ´åã«è¡ããã
- ã¤ã¡ã¼ã¸ã®åå¾ã¯ã`compose up`ããããã¤ã¡ã¼ã¸ãå­å¨ããªãã£ãå ´åã«è¡ããã
- `environment:`ã¯ã`container run --env`ã§æå®ãã¦ããç°å¢å¤æ°ãç½®ãæãã
- `volumes`ï¼ï¼éå±¤ç®ï¼: `volume create`ã§è¡ã£ã¦ããããªã¥ã¼ã ã®ä½æãç½®ãæãã
- `volumes`ï¼ï¼éå±¤ç®ï¼ï¼`container run --mount`ã§æå®ãã¦ããããªã¥ã¼ã ã®ãã¦ã³ããç½®ãæãã
  - ï¼ã`--mount`ãªãã·ã§ã³ã®æå®æ¹æ³ã¨ä¼¼ã¦ãããã¨ãããã`--volume`ãªãã·ã§ã³ã§ã®æ¸ãæ¹ãã§ããã£ã½ãï¼ï¼ 
  - `<volume name>:<mount point>`ï¼ããªã¥ã¼ã åã¨ãã¦ã³ããã¤ã³ãï¼ã³ã³ããä¸ã®ãã¹ï¼ãæå®ããã®ããããªã¥ã¼ã ãã¦ã³ã
  - `<path>:<mount point>`ï¼ãã¹ããã·ã³ã®ãã¹ã¨ãã¦ã³ããã¤ã³ãï¼ã³ã³ããä¸ã®ãã¹ï¼ãæå®ããã®ãããã¤ã³ããã¦ã³ã
- `ports:`ã«ã`container run --publish`ã§æå®ãã¦ãããã¼ãã®å¬éãç½®ãæãã
- èªåã§ããªãã¸ãããã¯ã¼ã¯ãä½æããããµã¼ãã¹åãèªåã§ã³ã³ããã®ãããã¯ã¼ã¯ã«ãããã¨ã¤ãªã¢ã¹ã¨ãã¦è¨­å®ããã
- `command:`ã«ã¯ã`container run <command>`ã§æå®ãã¦ããããã©ã«ãå½ä»¤ã®æå®ãç½®ãæãã
- `depends_on:`ã«ã¯ãä¾å­é¢ä¿ãå®ç¾©ã`docker-compose up`ãããæã«ã`depends_on:`ã«è¨ããããµã¼ãã¹ãã¡ããåã«èµ·åããã`docker-compose stop`ãããæã«ã¯ã`depends_on:`ã«è¨ããããµã¼ãã¹ãã¡ãå¾ã«ãªããããªé ã§åæ­¢ããã
