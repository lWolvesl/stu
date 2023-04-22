# Git

[回到主页](../README.md)

## .gitignore

- 删除`Mac`索引 `.DS_Store`

  - `.gitignore`

  - ```gitignore
    .DS_Store
    
    **/.DS_Store
    
    .DS_Store?
    ```

  - 对于已经存在仓库的`.DS_Store`文件

  - ```shell
    find . -name .DS_Store -print0 | xargs -0 git rm -f --ignore-unmatch
    ```

  - 

## 一次Push多个远程仓库

在`.git/config`中的`remote`增加多个url即可

```config
[remote "origin"]
	url = url1
	url = url2
	fetch = +refs/heads/*:refs/remotes/origin/*
```

## Vscode显示`.git`文件目录

在设置中

- 快捷键
  - mac：Cmd+，
  - Win/Linux：Ctrl+，

搜索`files.exclude`，更改隐藏规则

`"**/.git""`



## 代理

- 单次代理

```shell
git -c http.proxy=<> ...
git -c https.proxy=<> ...
```

