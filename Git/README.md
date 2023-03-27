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

