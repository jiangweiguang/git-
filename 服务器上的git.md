# 连接远程服务器

1. 生成SSH公钥：

   1. 查看SSH密钥存储位置：一般情况下在`cd ~/.ssh`

      ```console
      $ cd ~/.ssh
      $ ls
      authorized_keys2  id_dsa       known_hosts
      config            id_dsa.pub
      ```

      1. 若没有id_dsa密钥和id_dsa.pub公钥，则需创建。
         1. `ssh-keygen`
      2. 查看公钥：`cat ~/.ssh/id_rsa.pub`