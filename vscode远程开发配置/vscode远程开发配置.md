### VSCode 远程开发进行配置

#### 本地生成SSH的公钥私钥
+ 这里使用git的命令行工具进行生成,会得到两个文件id_rsa和id_rsa.pub，当然名字在生成的过程中可以自定义

  ```shell
  ssh-keygen -t rsa -C 'test@gmail.com'
  ```

#### 给需要远程的机器加上SSH登录功能（SSH公钥登录）

+ 拷贝本地生成的 `id_rsa.pub`文件到目标机器 tmp文件夹 /tmp，然后把文件拷贝到 目标机器的authorized_keys配置中

  ```shell
  cd /tmp
  cat id_rsa.pub >> ~/.ssh/authorized_keys
  ```

+ 对被远程的目标机器设置可以进行SSH公钥登录，然后重启

  ```shell
  vim /etc/ssh/sshd_config
  
  #修改配置
  PasswordAuthentication yes　　　　　　# 口令登录
  RSAAuthentication yes　　　　　　　　　# RSA认证
  PubkeyAuthentication yes　　　　　　　# 公钥登录 
  
  #重启sshd
  sudo /etc/init.d/ssh restart
  ```

#### 对本机的vscode的添加远程插件

![image-20231007143912827](G:\source\mygithub\ToolInstall\vscode远程开发配置\assets\image-20231007143912827-1696660766472-1.png)

#### 配置被远程机器

1. 添加机器![image-20231007144154269](G:\source\mygithub\ToolInstall\vscode远程开发配置\assets\image-20231007144154269.png)

2. 增加配置

![image-20231007144335878](G:\source\mygithub\ToolInstall\vscode远程开发配置\assets\image-20231007144335878.png)

3. 编辑配置

   ![image-20231007144503863](G:\source\mygithub\ToolInstall\vscode远程开发配置\assets\image-20231007144503863.png)

   

   + 编辑配置

     > 点击按钮后会在配置中自动增加一个对应192.168.11.7的配置，需要手动补充对于SSH的配置

     ![image-20231007144934222](G:\source\mygithub\ToolInstall\vscode远程开发配置\assets\image-20231007144934222.png)

	+ 这是一个完整的配置
```shell
# 配置文件参数
# Host : 配置对应的的主机名和 ssh 文件（可以直接填写 ip 地址）
# HostName : 要登录主机的主机名（建议与 Host 一致）
# PreferredAuthentications ：配置登录的验证方式(含密码可自行查询配置方式)
# IdentityFile : 指明上面 User 对应的配置路径
# User : 登录名（如 github 的 username）
# Port: 端口号（默认 22）

#gogs
Host 192.168.11.6
HostName 192.168.11.6
PreferredAuthentications publickey
IdentityFile C:\Users\kai.yin\.ssh\id_rsa_gogs_yk
User kai.yin@kingsunsoft.com
Port 10022

#192.168.3.101
Host 192.168.3.101
  HostName 192.168.3.101
  PreferredAuthentications publickey
  IdentityFile C:\Users\kai.yin\.ssh\id_rsa_gogs_yk
  User yinkai
  ForwardAgent yes

#192.168.11.8
Host 192.168.11.8
  HostName 192.168.11.8
  PreferredAuthentications publickey
  IdentityFile C:\Users\kai.yin\.ssh\id_rsa_gogs_yk
  User yinkai

#192.168.11.7
  Host 192.168.11.7
  HostName 192.168.11.7
  PreferredAuthentications publickey
  IdentityFile C:\Users\kai.yin\.ssh\id_rsa_gogs_yk
  User yinkai

Host *
HostkeyAlgorithms +ssh-rsa
PubkeyAcceptedKeyTypes +ssh-rsa
```

