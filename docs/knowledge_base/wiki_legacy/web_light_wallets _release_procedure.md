  **原文链接**：[https://dev.bitshares.works/en/master/knowledge_base/wiki_legacy/web_light_wallets%20_release_procedure.html](https://dev.bitshares.works/en/master/knowledge_base/wiki_legacy/web_light_wallets%20_release_procedure.html)
 
 **译者**：[https://github.com/b-t-s-1](b-t-s-1)
 
 **校对者**： 
  
***

Web和轻钱包发布流程
==========================================

发布说明
----------------------

- 结帐并测试[graphene-ui master](https://github.com/cryptonomex/graphene-ui)
- 将发布说明添加到 `graphene-ui/release-notes.txt`
- 提交你的更改

打开Ledger钱包发布：

- 克隆 [openledger graphene-ui](https://github.com/openledger/graphene-ui)
- 添加上游 repo `git remote add cnx https://github.com/cryptonomex/graphene-ui`
- 获取上游 `git fetch cnx`
- 合并上游/主 `git merge cnx/master`
- 运行 `npm install` in `dl/` and in `web/`
- 提交对openledger存储库的更改
- 以这种格式标记: `2.X.YYMMDD`
- 推送包括标签 `git push --tags`
- 构建 (`cd web/ && npm run build`)
- 打开 `dist/index.html` 并确保一切正常，状态栏中的版本与标签匹配
- 克隆 `https://github.com/cryptonomex/faucet` 分支并使用bundle命令安装gem
- 在水龙头目录中运行 `mina wallet` - 这将部署到 'ol' 服务器 (在`.ssh/config`指定)
- 或者，将dist文件夹直接复制到服务器: scp dist/* bitshares.openledger.info:/www/current/public/wallet/
- 打开 [https://bitshares.openledger.info](https://bitshares.openledger.info/) 并确保没有错误和版本匹配发布标签

轻钱包
------------------

- （一次）安装 [NSIS](http://nsis.sourceforge.net/Main_Page)
- `git clone https://github.com/bitshares/bitshares-2-ui` branch `bitshares`
- 添加上游 repo `git remote add cnx https://github.com/cryptonomex/graphene-ui`
- 获取上游 `git fetch cnx`
- 将upstream/master合并到bitshares分支`git merge cnx/master`
- 在`dl/`，`web/`和`electron/`中运行`npm install`
- 使用发布版本标记它
- 编辑`electron/build/package.json`并更新版本
- 提交你的更改并推送提交和标记
- 通过`npm run electron`命令在`web/`中构建它
- 转到 `electron/`
- 通过`npm run release`命令构建轻钱包
- 它将在`releases/`中创建dmg / deb / exe文件
- 确保创建的文件名与标签/版本匹配
- 运行安装程序并测试应用程序
- 转到bitshares-2 repo
- 更新gui_version，commit，tag和push - 这将创建一个新标签
- 打开https://github.com/bitshares/bitshares-2/releases，在上一步创建的标签下创建新版本
- 指定发行说明，上传创建的dmg/deb/exe钱包

Bitshares.org钱包和下载页面
----------------------------------------------

- 转到bitshares.gihub.io repo (`git clone https://github.com/bitshares/bitshares.github.io`)
- 将`bitshares-2-ui/web/dist/*`复制到`wallet/`
- 编辑`_includes / download.html`并更新下载链接和gui发布日期
- 提交推送


