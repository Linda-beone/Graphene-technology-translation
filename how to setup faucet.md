 ===========================================

  **原文链接**：<http://dev.bitshares.works/en/master/development/testnets.html>

 **译者**：https://github.com/towel223 （小办龙的蜗牛）

 **校对者**：

 ===========================================

# 如何设置水龙头 #
## 目录 ##
1.安装相关包

2.获得源码

3.配置

更新钱包

为了让人们在没有资金支付费用的情况下进入系统，我们需要安装一个水龙头。这里，我们将用mina作为部署生产工具的安装。



1.安装相关包

    sudo apt-get install mysql-server libmysqlclient-dev
    # 给mysql设置一个主密码


同样安装合适版本的Ruby：

    cd
    git clone git://github.com/sstephenson/rbenv.git .rbenv
    echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
    echo 'eval "$(rbenv init -)"' >> ~/.bashrc
    exec $SHELL
    
    git clone git://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build
    echo 'export PATH="$HOME/.rbenv/plugins/ruby-build/bin:$PATH"' >> ~/.bashrc
    exec $SHELL
    
    git clone https://github.com/sstephenson/rbenv-gem-rehash.git ~/.rbenv/plugins/rbenv-gem-rehash
    
    sudo rbenv install 2.2.3
    sudo rbenv global 2.2.3
    sudo gem install bundle



2.获取源码

    git clone https://github.com/cryptonomex/faucet
    cd faucet
    sudo bundle   # ignore warnings



3.配置

1）水龙头配置

2）rails API的密码种子配置

3）数据库存储配置

4）部署工具与mina



1）水龙头配置

所有的请求设置都在config、faucet.yml，他有一个示例文件：

    cp config/faucet-example.yml config/faucet.yml
    vim config/faucet.yml



2）Rails API配置
Rails的内部架构需要一个密码种子，我们可以搜索一个新的密码种子。将他加到配置文件对应的行中。

    cp config/secrets-example.yml config/secrets.yml
    rake secret
    vim config/secrets.yml



3a）数据存储配置
默认的数据存储配置会认为MYSQL的密码是空根密码。因此如果你想设置不同的密码，你需要在编辑下面文件并提供不同的密码：


    vim config/database.yml



我们通过如下操作生成我们的数据：

    rake db:create; rake db:migrate; rake db:seed
    RAILS_ENV=production bundle exec rake db:create db:schema:load



3b）数据库设置
我们也需要增加一个数据库的入口以便于页面的加载和推荐工作正常：

    1. Go to `/www/current`
    2. execute: `rails db`, a mysql console will open
    3. Execute: `insert into widgets set allowed_domains='testnet.bitshares.eu'`; (replace the domain with your domain)



4）Mina部署
Mina适用于部署水龙头并将所有必要的文件放到专用的机器上（可能是同一台机器）。：

    cp config/deploy-example.yml config/deploy.yml
    vim config/deploy.yml



确认添加你的ssh-pub密钥到认证文件中以便mina可以部署相对应的机器。

创建直接服务的公共文件，并将配置文件复制/链接到部署的共享文件。

    sudo mkdir /www
    sudo chown -R gph:gph /www
    
    mina setup
    ln -s $HOME/faucet/config/faucet.yml /www/shared/config/
    ln -s $HOME/faucet/config/secrets.yml /www/shared/config/
    ln -s $HOME/faucet/config/database.yml /www/shared/config/


部署mina和wallet如下：

    ln -s $HOME/graphene-ui/web/dist $HOME/faucet/public/wallet
    mina deploy
    mina wallet



更新钱包

如果你的钱包有一些修改，你需要重新编译Javascript/HTML文件和重新部署钱包如下：

    _# re-compile_
    _#重新编译
    cd ~/graphene-ui/web
    git pull # fetch changes from origin #从源头获取改变
    npm run build
    
    _ # deploy_
    _#部署
    cd ~/faucet
    mina wallet

