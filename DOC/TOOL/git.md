# git

## 设置帐号和邮件

git config --global user.name "Your Name"
git config --global user.email "email@example.com"


## 创建repository

在需要创建的目录下执行初始化命令
git init


## 提交

git add .            添加当前所有文件，可以选择添加
git commit -m  ..    提交，-m加入提交理由


## 状态

git status           查看当前状态
git diff             查看当前库中修改的内容


## 记录

git log              查看历史提交记录   --oneline只显示简略信息
git reflog           回退后，可以查看后面的版本号


## 回退

git reset  --hard  ****

****可以是版本号，默认值是head~1(上一次提交的版本，数字往前提几个版本)



## 删除

git rm  a.txt


## 分支

```
git  branch                      查看分支  

git  branch   b1                 在当前分支状态创建分支  
git  checkout b1                 切换到b1分支  
git  checkout -b b1              创建并切换到分支b1  
git  branch -m oldname newname   重命名分支   

git  merge b1    --squash         当前分支合并b1分支  --squash参数只进行合并，先不进行提交  
git  cherry-pick   ***    -n      选择reflog中的提交号码进行合并，-n可以只进行合并，先不提交  


git  branch -d b1   删除b1分支(如果b1分支没有被合并，则使用-D)  


冲突
<<<<  HEAD  **** ======mergeTxt>>>>>branchName    

HEAD之后是开始冲突的内容

======mergeTxt>>>>>  mergeTxt 是合并过来的内容
branchName是合并分支的名称

```

## 局域网

+ 服务器  
  git init --bare   
  [//192.168.111.2/aaa (aaa为服务器init --bare的位置)]

+ 客户端
  git config --global user.name "Your Name"  
  git config --global user.email "email@example.com"

+ 创建git库    
  git init  

+ 获取  
  git pull   //192.168.111.2/aaa



+ 推送  
  git add *  
  git push  //192.168.111.2/aaa  
  git push --set-upstream //192.168.111.2/aaa master  



+ tortoiseGit设置
  Remote: 名称任意  
  URL: //192.168.111.2/aaa (aaa为服务器init --bare的位置)  



## github

git config --global user.name "user"  
git config --global user.email "123456@qq.com"  

//ubuntu安装ssh
sudo apt-get install ssh  
//测试  
ssh -T git@github.com  

//生成key  
cd ~/.ssh     
ssh-keygen -t rsa -C "760437565@qq.com"    
命令行需要输入验证码和确认    
.pub文件中包含sshkey，github中SSH Keys进行添加    

下载  
git clone https://github.com/JiYangLin/**  
