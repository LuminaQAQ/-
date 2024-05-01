## 一. 基本操作

1. 清除历史
   
   ```bash
   clear
   ```

2. 查看配置
   
   ```bash
   git config [--system/--global] [--list]
   ```

3. 查看本地仓库状态
   
   ```bash
   git status
   ```

## 二. 基本配置

1. 配置用户名
   
   ```bash
   git config --global user.name "name"
   ```

2. 配置邮箱
   
   ```bash
   git config --global user.email "email"
   ```

## 三. 搭建本地仓库

1. 初始化
   
   ```bash
   git init
   ```

2. 克隆
   
   ```bash
   git clone <link>
   ```

3. 添加到暂存区(.代表所有)
   
   ```bash
   git add 单个文件
   
   git add 文件夹1/ 文件夹2/ ……多个文件夹之间空格隔开git
   
   git add .
   ```

4. 提交暂存区到本地仓库(-m 意思是 该次提交时的信息)
   
   ```bash
   git commit -m
   ```

5. 提交到远程仓库
   
   ```bash
   git push -u origin master
   ```

6. 指定不提交的文件(.gitignore)
   
   ```gitignore
   *.txt        # 忽略所有 .txt 文件
   !lib.txt     # 除 lib.txt 文件
   /temp        # 忽略 根目录下 的temp文件
   build/       # 忽略 build目录下 的所有文件
   build/1.txt  # 忽略 build目录下 的 1.txt 文件
   ```

7. 绑定本机SSH公钥(实现免密码登陆)
   
   1. 需要找到 .ssh 文件夹, 位置: ` C:\Users\Administrator\.ssh`
   
   2. 若没有, 则` ssh-keygen -t rsa` 然后一路回车即可(-t rsa是指加密)
   
   3. 测试SSH连接`ssh -T git@github.com `

8. 列出本地分支
   
   ```bash
   git branch
   ```

9. 列出远程分支
   
   ```bash
   git branch -r
   ```

10. 新建分支
    
    ```bash
    git branch [branch_name]
    ```

11. 新建分支并切换到该分支
    
    ```bash
    git checkout -b [branch_name]
    ```

12. 合并分支
    
    ```bash
    git merge [branch_name]
    ```

## 四. 总结(一般会用到的)

1. 基本流程
   
   1. 克隆远程仓库
      
      ```bash
      git clone 'path'
      ```
   
   2. 生成SSH或测试远程连接
      
      ```bash
      ssh-keygen -t rsa [-C 'email']
      
      ssh -T git@github.com
      ```
   
   3. 初始化本地仓库
      
      ```bash
      git init
      ```
   
   4. 添加暂存文件
      
      ```bash
      git add .
      ```
   
   5. 提交暂存文件到本地仓库
      
      - 注意这一步是会跳到.gitignore, 需要提交的文件要把前面的#号去掉
      
      ```bash
      git commit
      ```
   
   6. 提交到远程仓库
      
      ```bash
      git push -u origin master
      ```

2. 分支
   
   1. 查看分支
      
      ```bash
      git branch
      ```
   
   2. 切换分支
      
      ```bash
      git checkout <branch-name>
      ```
   
   3. 合并分支
      
      ```bash
      git merge <branch-name>
      ```
   
   4. 删除分支
      
      ```bash
      git branch -d <branch-name>
      ```
   
   5. 更新分支
      
      ```bash
      git pull origin <branch-name>
      ```
   
   6. 
