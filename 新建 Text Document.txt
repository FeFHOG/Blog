@echo off
chcp 65001 >nul
echo ============================================
echo      QuantumRose Blog 一键同步工具
echo ============================================

:: 1. 清理旧的构建文件
echo [1/4] 正在清理旧的 site 文件夹...
if exist site rd /s /q site

:: 2. 检查是否有未提交的更改
echo [2/4] 正在提交更改到 GitHub...
git add .
set /p msg="请输入本次更新说明 (直接回车则为 'update blog'): "
if "%msg%"=="" set msg=update blog
git commit -m "%msg%"

:: 3. 推送到远程仓库
echo 正在推送到 GitHub...
git push origin main

:: 4. 本地构建静态文件 (用于同步到你的服务器目录)
echo [3/4] 正在生成静态网页 (mkdocs build)...
mkdocs build

:: 5. (可选) 自动拷贝到 Nginx/IIS 目录
:: 如果你的服务器就在本地，取消下面一行的注释，并修改路径
:: xcopy /e /y .\site\* C:\nginx\html\

echo [4/4] 恭喜！更新已完成。
echo GitHub 已同步，site 文件夹已更新。
pause