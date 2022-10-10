> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [cloud.tencent.com](https://cloud.tencent.com/developer/article/1839744) 原创

NodeJs 获取 Git 用户和邮箱信息
=====================

修改于 2021-06-28 10:31:13 阅读 7240

获取 git config 的路径，一种是项目单独配置的，一种是全局配置的

```
'use strict';

const fs = require('fs');
const os = require('os');
const path = require('path');

module.exports = function (type) {
    let configPath = '';
    const workDir = process.cwd();

    if (type === 'global') {
        configPath = path.join(os.homedir(), '.gitconfig');
    } else {
        configPath = path.resolve(workDir, '.git/config');
    }

    if (!fs.existsSync(configPath)) {
        configPath = path.join(os.homedir(), '.config/git/config');
    }

    return fs.existsSync(configPath) ? configPath : null;
}

```

复制

读取配置文件，使用 `ini` 库解析成对象模式

```
'use strict'

const fs = require('fs');
const filePath = require('./path');
const ini = require('ini');

module.exports = function (type) {
    const path = filePath(type)
    if (path) {
        const file = fs.readFileSync(path, 'utf8');
        return ini.parse(file);
    }
    return '';
};

```

复制![](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADIAAAAyCAYAAAAeP4ixAAACbklEQVRoQ+2aMU4dMRCGZw6RC1CSSyQdLZJtKQ2REgoiRIpQkCYClCYpkgIESQFIpIlkW+IIcIC0gUNwiEFGz+hlmbG9b1nesvGW++zxfP7H4/H6IYzkwZFwQAUZmpJVkSeniFJKA8ASIi7MyfkrRPxjrT1JjZ8MLaXUDiJuzwngn2GJaNd7vyP5IoIYY94Q0fEQIKIPRGS8947zSQTRWh8CwLuBgZx479+2BTkHgBdDAgGAC+fcywoyIFWqInWN9BSONbTmFVp/AeA5o+rjKRJ2XwBYRsRXM4ZXgAg2LAPzOCDTJYQx5pSIVlrC3EI45y611osMTHuQUPUiYpiVooerg7TWRwDAlhSM0TuI+BsD0x4kGCuFSRVzSqkfiLiWmY17EALMbCAlMCmI6IwxZo+INgQYEYKBuW5da00PKikjhNNiiPGm01rrbwDwofGehQjjNcv1SZgddALhlJEgwgJFxDNr7acmjFLqCyJuTd6LEGFttpmkYC91Hrk3s1GZFERMmUT01Xv/sQljjPlMRMsxO6WULwnb2D8FEs4j680wScjO5f3vzrlNJszESWq2LYXJgTzjZm56MCHf3zVBxH1r7ftU1splxxKYHEgoUUpTo+grEf303rPH5hxENJqDKQEJtko2q9zGeeycWy3JhpKhWT8+NM/sufIhBwKI+Mta+7pkfxKMtd8Qtdbcx4dUQZcFCQ2I6DcAnLUpf6YMPxhIDDOuxC4C6djoQUE6+tKpewWZ1wlRkq0qUhXptKTlzv93aI3jWmE0Fz2TeujpX73F9TaKy9CeMk8vZusfBnqZ1g5GqyIdJq+XrqNR5AahKr9CCcxGSwAAAABJRU5ErkJggg==)

原创声明，本文系作者授权腾讯云开发者社区发表，未经许可，不得转载。

如有侵权，请联系 cloudcommunity@tencent.com 删除。

[展开阅读全文](javascript:;)[举报](javascript:;)点赞 5分享

[登录](javascript:;) 后参与评论_0_ 条评论