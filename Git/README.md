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

