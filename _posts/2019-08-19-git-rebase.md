---
layout: post
title:  "正确使用git rebase"
date:   2019-08-19 09:10:10 +0800
# categories: 
tags: [git]

---
> git rebase 功能及相关使用方法

* 合并多个commit
* 分支合并，避免分支混乱

### 合并多个commit
git log 查看commit_id
```
commit 34480597a0fd13339c5108b810efa5c10b20d870 (HEAD -> develop)
Author: menghuanwd <651019063@qq.com>
Date:   Mon Aug 19 09:32:14 2019 +0800

    dev

commit 166c376a3c018e6bc7f1a6ab36820ac9091d7c26 (master)
Author: menghuanwd <651019063@qq.com>
Date:   Mon Aug 19 09:32:39 2019 +0800

    master

commit 5925b95fe3343c22ce5160ee1d39c12bea13b86c
Author: menghuanwd <651019063@qq.com>
Date:   Mon Aug 19 09:31:26 2019 +0800

    test

commit 15bd115058946a2e8e40f0698df89e766da55e26
Author: menghuanwd <651019063@qq.com>
Date:   Wed Jul 24 11:16:13 2019 +0800

    rtyuio

commit 221a3e06e5da979e8d8039fd41b5ae2d9fc678de
Author: menghuanwd <651019063@qq.com>
Date:   Wed Jul 24 11:15:54 2019 +0800

    4

commit 12e7bddca947a96eb2ddb8f51d255b4b8d3402dd
Author: menghuanwd <651019063@qq.com>
Date:   Wed Jul 24 11:15:28 2019 +0800

    2

commit ca020802d9e879d11f77742c363b7bd2121af87d
Author: menghuanwd <651019063@qq.com>
Date:   Wed Jul 24 11:15:06 2019 +0800

    init
```
目的： 把master test rtyuio合并成一个commit, comment为 合并
```
git rebase -i 221a3e06e5da979e8d8039fd41b5ae2d9fc678dddd
```

把 需要合并的 pick 改成 squash(s) 保存退出
编辑comment  保存退出
```
commit d81c110ce816052c7ce3821e9d5a94430fd2d1f8 (HEAD -> develop)
Author: menghuanwd <651019063@qq.com>
Date:   Mon Aug 19 09:32:14 2019 +0800

    dev

commit 9f13159f34266fd2ebd5f48caf193fb5d9afb69a
Author: menghuanwd <651019063@qq.com>
Date:   Wed Jul 24 11:16:13 2019 +0800

    合并

commit 221a3e06e5da979e8d8039fd41b5ae2d9fc678de
Author: menghuanwd <651019063@qq.com>
Date:   Wed Jul 24 11:15:54 2019 +0800

    4

commit 12e7bddca947a96eb2ddb8f51d255b4b8d3402dd
Author: menghuanwd <651019063@qq.com>
Date:   Wed Jul 24 11:15:28 2019 +0800

    2

commit ca020802d9e879d11f77742c363b7bd2121af87d
Author: menghuanwd <651019063@qq.com>
Date:   Wed Jul 24 11:15:06 2019 +0800

    init
```

### 分支合并，避免分支混乱

```
git rebase master

如果有冲突
git add .
git rebase --continue
```
