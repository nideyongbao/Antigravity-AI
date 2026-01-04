# Antigravity 安全限制

## 🚫 禁止操作
- 删除系统文件或根目录
- 执行 `rm -rf /` 或类似命令
- 直接修改 `.git/` 目录内容
- 执行未经审查的远程脚本
- 访问或修改其他用户目录

## ⚠️ 需审批操作
以下操作需要用户明确确认：
- 安装系统级依赖 (`apt install`, `brew install`)
- 修改环境变量文件 (`.env`, `.bashrc`)
- 执行数据库迁移
- 推送代码到远程仓库
- 创建/删除 Git 分支

## ✅ 允许自动执行
- `.antigravity-output/` 目录下的文件操作
- `npm install` / `pip install` (项目级)
- 运行测试 (`pytest`, `npm test`)
- 代码格式化 (`prettier`, `black`)
- 读取项目文件
