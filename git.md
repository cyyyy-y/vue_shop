### 创建新的分支
```
git checkout -b login
```

### 查看源代码状态
```
git status
```

### 添加到暂存区
```
git add .
```

### 再次查看源代码状态
```
git status
```

### 提交到本地仓库
```
git commit -m "XXX"
```

### 查看分支
```
git branch
```

###合并login至main分支
#### 1. 切换分支
```
git checkout main
```
#### 2. 合并分支
```
git merge login
```

### 推送main分支
```
git push
```

### 本地login分支推送至云端login分支
#### 1. 切换分支
```
git checkout login
```
#### 2. 推送分支
```
git push -u origin login
```




