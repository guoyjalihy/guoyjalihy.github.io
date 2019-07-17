---
layout: post
title:  "修复git已提交代码的name和email"
date:   2019-07-16 21:44:01 +0800
categories: tools
---

#### 背景
当我们更换环境后使用git commit代码的name及email和以前的不一致，导致没有权限执行pull或push。
#### 批量修改已提交代码的name和email
1. 新建git.sh文件，复制粘贴如下内容后保存

    ```cmd
    #!/bin/sh
    
    git filter-branch --env-filter '
    
    OLD_EMAIL="guoyanjun@cmhi.chinamobile.com"
    CORRECT_NAME="guoyjalihy"
    CORRECT_EMAIL="guoyjalihy@icloud.com"
    
    if [ "$GIT_COMMITTER_EMAIL" = "$OLD_EMAIL" ]
    then
        export GIT_COMMITTER_NAME="$CORRECT_NAME"
        export GIT_COMMITTER_EMAIL="$CORRECT_EMAIL"
    fi
    if [ "$GIT_AUTHOR_EMAIL" = "$OLD_EMAIL" ]
    then
        export GIT_AUTHOR_NAME="$CORRECT_NAME"
        export GIT_AUTHOR_EMAIL="$CORRECT_EMAIL"
    fi
    ' --tag-name-filter cat -- --branches --tags
    ```

2. 将git.sh放到需要修复的项目更目录下，执行该脚本
    ```cmd
    $ ./git.sh
    Rewrite 719bb0a26909552752d3e1edc3836dc22291e9b3 (1/62) (0 seconds passed, remai
    Rewrite cdd566a46036b3289aa221ebf577e57d11cde8a3 (2/62) (0 seconds passed, remai
    Rewrite 3c3e306e6bcd2cfcd8ed00b86472734442beba84 (3/62) (1 seconds passed, remai
    Rewrite fcf4d55d26333735576129a417ca05006af0260d (3/62) (1 seconds passed, remai
    Rewrite 7cf8ce744382910e8e933f201908300b43e906db (3/62) (1 seconds passed, remai
    Rewrite 88f20a68f6c00ce34e2f5c0424ee1227eb67788d (3/62) (1 seconds passed, remai
    Rewrite f803ceb6a1396403135ca0359192107bb1af71e3 (7/62) (3 seconds passed, remai
    ...
    ```
    出现如上日志表示修复成功
    
3. 执行如下命令提交代码
    ```cmd
    git push --force --tags origin 'refs/heads/*'
    ```     