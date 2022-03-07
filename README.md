## 文件图片路径问题

> `hexo-abbrlink`导致`hexo-asset-image`加载`.md`文件图片路径问题
>
> 解决修改`node_modules\hexo-asset-image\index.js`文件，代码如下：

```js
'use strict';
var cheerio = require('cheerio');

// http://stackoverflow.com/questions/14480345/how-to-get-the-nth-occurrence-in-a-string
function getPosition(str, m, i) {
  return str.split(m, i).join(m).length;
}

hexo.extend.filter.register('after_post_render', function (data) {
  var config = hexo.config;
  if (config.post_asset_folder) {
    var link = data.permalink;
    var beginPos = getPosition(link, '/', 3) + 1;
    var appendLink = '';
    // In hexo 3.1.1, the permalink of "about" page is like ".../about/index.html".
    // if not with index.html endpos = link.lastIndexOf('.') + 1 support hexo-abbrlink
    if (/.*\/index\.html$/.test(link)) {
      // when permalink is end with index.html, for example 2019/02/20/xxtitle/index.html
      // image in xxtitle/ will go to xxtitle/index/
      appendLink = 'index/';
      var endPos = link.lastIndexOf('/');
    } else {
      var endPos = link.lastIndexOf('.');
      // var endPos = link.lastIndexOf('/');
    }
    link = link.substring(beginPos, endPos) + '/' + appendLink;
    // console.log(link) // 解决hexo-asset-image图片路径
    var toprocess = ['excerpt', 'more', 'content'];
    for (var i = 0; i < toprocess.length; i++) {
      var key = toprocess[i];

      var $ = cheerio.load(data[key], {
        ignoreWhitespace: false,
        xmlMode: false,
        lowerCaseTags: false,
        decodeEntities: false
      });

      $('img').each(function () {
        if ($(this).attr('src')) {
          // For windows style path, we replace '\' to '/'.
          var src = $(this).attr('src').replace('\\', '/');
          if (!(/http[s]*.*|\/\/.*/.test(src) || /^\s+\//.test(src) || /^\s*\/uploads|images\//.test(src))) {
            // For "about" page, the first part of "src" can't be removed.
            // In addition, to support multi-level local directory.
            var linkArray = link.split('/').filter(function (elem) {
              return elem != '';
            });
            var srcArray = src.split('/').filter(function (elem) {
              return elem != '' && elem != '.';
            });
            // if (srcArray.length > 1) srcArray.shift();
            while(srcArray.length > 1) {
              srcArray.shift();
            }
            src = srcArray.join('/');

            $(this).attr('src', config.root + link + src);
            // $(this).attr('src', '/' + link + src);
            console.info && console.info('update link as:-->' + link + src);
            console.info && console.info('update link as:-->' + config.root + link + src);
          }
        } else {
          console.info && console.info('no src attr, skipped...');
          console.info && console.info($(this));
        }
      });
      data[key] = $.html();
    }
  }
});
```

```shell
# 本地cmd
# 生成 deployment deployment.pub
$ ssh-keygen -t rsa -C autodeployment -f deployment

# 将 deployment 复制到 .ssh 目录下（Git私钥目录）
$ cp ~/deployment .

# CentOS 服务器上
$ cd ~
# 将本地的 deployment.pub 复制到 CentOS 服务器默认路径下 /root

# 进入 /root/.ssh
cd .ssh

# 打开 authorized_keys，没有则自行创建，将公钥写入
$ vim authorized_keys
$ ESC + :wq

# 将 deployment.pub 写入 authorized_keys
$ cd ../
$ cat deployment.pub >> ~/.ssh/authorized_keys

# 配置github，将本地的私钥复制到github上


```

```bash
on: push
name: Deploy
jobs:
  build:
    name: Build
    runs-on: ubuntu-latest
    steps:
    - name: Clean
      uses: heowc/action-hexo@master
      with:
        args: clean
    - name: Generate
      uses: heowc/action-hexo@master
      with:
        args: generate
    - name: Deploy
      uses: heowc/action-hexo@master
      env:
        EMAIL: ""
        NAME: ""
        SSH_PRIVATE_KEY: ${{ secrets.TENGXUNYUN }}
        ARGS: "-rltgoDzvO --delete"
        SOURCE: "public/"
        REMOTE_HOST: "49.235.41.140"
        REMOTE_USER: "root"
        TARGET: "/www/blog/"
      with:
        args: deploy
```
