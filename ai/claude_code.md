# claude code
## install and start
### linux安装
```
curl -fsSL https://claude.ai/install.sh | bash -s 2.1.128

```

### 跳过登录和国家验证
在～（用户目录）下的.claude.json文件配置中，最后添加这样一行配置：
```
    "hasCompletedOnboarding":true
```

## 禁用更新
- 如果使用包管理安装，如果包管理不更新，也不会自动更新。
```
```

## 使用中
### 指定子代码模型
通过`~/.claude/settings.json`配置env,也可以通过`cc-swtich`可视化进行配置
```
 "env": {
    "CLAUDE_CODE_SUBAGENT_MODEL": "deepseek-v4-pro"
  }
```