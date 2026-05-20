---
title: 你好，世界
date: 2026-05-20
summary: 这是我的第一篇博客文章，欢迎来到我的博客！
tags: [Java, Spring Boot, Vue, GitHub]
author: Faded
author_email: 1393270387@qq.com
slug: hello-world
published: true
---
## 欢迎来到我的博客

这是一个基于 **GitHub** 作为内容管理系统的博客平台。

### 技术栈

| 层级 | 技术 |
|------|------|
| 后端 | Spring Boot 3 + Java 17 |
| 前端 | Vue 3 + Vite |
| 内容 | GitHub 仓库 Markdown |
| 渲染 | flexmark (服务端) |

### 工作流程

1. 在 GitHub 仓库中编写 Markdown 文章
2. 后端通过 GitHub API 自动拉取
3. 解析 Front Matter 元数据
4. 渲染 Markdown 为 HTML
5. 前端展示给读者

### 代码示例

```java
@RestController
public class HelloController {
    @GetMapping("/")
    public String hello() {
        return "Hello, GitHub Blog!";
    }
}
```

> 写作即提交——你的 GitHub 仓库就是你的 CMS。

---

*感谢阅读！*
