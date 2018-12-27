## [个人实践] 献给热爱CI的同事 —— GitLab 携手 Jenkins 助力CI
> 撰稿的时间在2017-3-17   作者：苏玲（ git2019@163.com ）

### 外文参考资料：
* [gitlab-plugin](https://github.com/jenkinsci/gitlab-plugin)
* [continuous-integration-with-jenkins-and-gitlab](https://medium.com/@teeks99/continuous-integration-with-jenkins-and-gitlab-fa770c62e88a)
 
### 闪亮点：
* 每个 commit 有 CI 的结果
* 每个 merge request 有 CI 的结果

### 前提：
* 采用公司的 gitlab 服务。
* jenkins已安装 git-lugin , gitlab-plugin 。
* 创建了一对公私钥( ssh-keygen -trsa方式）

### jenkins和gitlab的配置：
#### jenkins：创建credentials（SSH Username with private key） ，建好后第5步骤会用到。如图：
![创建credentials（SSH Username with private key） ](https://raw.githubusercontent.com/DevOpsLakes/devopslakes.github.io/master/images/jenkins/jenkins_credentials.JPG)
  
#### gitlab：粘贴公钥。
上面 credentials 中输入的是私钥（private key），我们得把对应的公钥（public key）放到gitlab某账号的ssh keys中去（略）。1）和 2）结合，才能在jenkins处获取代码仓库。

#### jenkins：创建credentials（Gitlab API token） ，建好后过会儿再用。
jenkins跑好ci后，要把状态写回到 gitlab ，因此，在jenkins服务这边必须做相应的配置，配置jenkins job访问gitlab。注意，图中的“API token”来自gitlab上某账户的 Private token 。
如下图：
![创建credentials（Gitlab API token）](https://raw.githubusercontent.com/DevOpsLakes/devopslakes.github.io/master/images/jenkins/jenkins_credentials.JPG)

#### jenkins：配置gitlab-plugin，用上面创建的credentials（Gitlab API token）。
系统管理 -> 系统设置 -> Gitlab
![配置 gitlab-plugin](https://raw.githubusercontent.com/DevOpsLakes/devopslakes.github.io/master/images/jenkins/jenkins_config_gitlab-plugin.JPG)

#### jenkins：创建job，配置源码管理 -> git。
![配置 job-git](https://raw.githubusercontent.com/DevOpsLakes/devopslakes.github.io/master/images/jenkins/jenkins_config_job_git.JPG)

#### jenkins：配置构建触发器/Build Triggers。
![配置触发器](https://raw.githubusercontent.com/DevOpsLakes/devopslakes.github.io/master/images/jenkins/jenkins_job_trigger.JPG)

#### jenkins: 配置构建后操作/Post-build Actions。
![配置构建后操作](https://raw.githubusercontent.com/DevOpsLakes/devopslakes.github.io/master/images/jenkins/jenkins_job_post_build.JPG)

#### jenkins: 配置 GitLab connection 。
![配置GitLab connection](https://raw.githubusercontent.com/DevOpsLakes/devopslakes.github.io/master/images/jenkins/jenkins_job_gitlab_connection.JPG)

#### gitlab:  配置 webhook 。
![到gitlab上配置 webhook](https://raw.githubusercontent.com/DevOpsLakes/devopslakes.github.io/master/images/jenkins/gitlab_webhook.JPG)
