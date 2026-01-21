# Bash 常用命令片段

## 文件操作

```bash
# 创建目录并进入
mkdir -p project/src && cd project

# 批量创建文件
touch file{1..10}.txt

# 查找文件
find . -name "*.md" -type f

# 查找并删除
find . -name "node_modules" -type d -exec rm -rf {} \;

# 查看文件大小
du -sh ./*

# 批量重命名
for f in *.md; do mv "$f" "prefix-${f}"; done
```

## 文本处理

```bash
# 搜索文件内容
grep -r "keyword" . --include="*.js"

# 统计代码行数
find . -name "*.js" | xargs wc -l

# 替换文本
sed -i '' 's/old/new/g' file.txt

# 去除空白行
cat file.txt | grep -v '^$' > new_file.txt
```

## Git 操作

```bash
# 查看分支差异
git diff main...feature-branch

# 暂存所有修改
git add -A

# 强制推送
git push -f origin main

# 查看被删除的文件
git log --diff-filter=D --name-only | head -20
```

## 网络

```bash
# 检查端口占用
lsof -i :3000

# 下载文件
curl -O https://example.com/file.zip

# 本地服务
python3 -m http.server 8000
```
