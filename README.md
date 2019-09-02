# 自用的博客部署脚本
纯粹是为了练习一下，写了简单的自动化部署脚本（没那么多时间，也就抽了3天时间简单写了一下），还有很多的地方需要完善

# 适用范围
这个自动化脚本目前仅适合我自己，其他人要用的话需要熟悉一下，读一下代码，欢迎提交iss来diss我一下，我会改进。

# 没有架构，思路（简单的文件夹说明）
- `blog`博客文件夹，就相当于把执行`hexo init`后产生的文件夹包装了一下而已
  - `blog\public`执行`hexo g`后生成的文件夹，也就是网站文件，nginx配置的时候要把根目录指向到这儿，网站访问的也是这个
  - `blog\source`博客文章啊，页面啊，都在这里
  - `blog\themes`主题文件夹
    - `blog\themes\next`我现在用的是next主题，没有执行过`npm i`的时候是不会有这个文件夹的，执行了之后会自动拉取最新的。
  - `blog\_config.yml`hexo的配置文件
- `public`本来想把这个当输出文件夹的，但想了一下还是不要乱改了，后来把github page页面的仓库自动克隆到这儿也不错
- `script`脚本文件夹，自动化脚本都在这里面
  - `script\application.log`执行脚本后产生的日志
  - `script\install.js`执行`npm i`后会执行这个脚本，这个脚本的功能有：
    - 看看`blog`文件夹执行过`npm i`没，没有的话执行一下`npm i`装一下hexo
    - 看看有没有主题文件夹，没有就拉一下新的主题，然后将主题配置文件(`blog\themes\next\_config.yml`)替换覆盖成自己自定义的主题配置文件(`themes\theme-config-files\_config.yml`)
  - `script\post-article.js`文章发布脚本
    - 去`blog`文件夹执行以下命令
    - `hexo clean`
    - `hexo g`
    - `hexo d`
    - 然后检查一下github page的仓库(`public\mtrucc.github.io`)在不在，在就拉取一下
    - 拉取之后将`blog\public`里面的文件覆盖到github page仓库文件夹
    - 然后去github page仓库文件夹执行`git add *`，`git commit -m "当前时间"`，`git push`，更新一下仓库
  - `script\utils.js`工具库，里面有啥自己看
  - `script\webhooks.js`服务器上自动拉取代码文件，就是如果仓库更新了会执行git pull自动拉取，拉取之后自动更新文章，更新github page。
- `test`测试脚本文件夹
- `themes\theme-config-files\_config.yml`自定义的主题配置文件夹

# 命令行使用说明
- `npm i` 安装博客，会自动安装依赖，安装完依赖之后会自动拉取最新的主题文件，然后将自定义的主题配置文件拉取下来
- `npm run ut` 更新主题配置文件
- `npm run up` 更新文章
- `npm run ua` 用pm2进程守护的方式执行github webhooks监控，当有git push操作的时候，会自动git pull 并执行文章自动更新。

# 要优化的地方
- 我的仓库啊这些都是写死的，要把这些参数放到新的json文件里让大家可以自己填写
- 自动全局安装pm2
- 这个说明文档
  - 对于小白来说怎么使用