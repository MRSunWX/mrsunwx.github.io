---
date: '2025-08-26T16:10:39+08:00'
draft: false
title: '测试文章'
categories: ["技术", "测试"]
tags: ["hugo", "测试", "博客"]
---

这是一个测试文章.
行内数学公式：$a^2 + b^2 = c^2$。

块公式，

$$
a^2 + b^2 = c^2
$$

<div>
$$
\boldsymbol{x}_{i+1}+\boldsymbol{x}_{i+2}=\boldsymbol{x}_{i+3}
$$
</div>

当然有 😎，PaperMod 真的是 Hugo 里功能非常丰富的主题，我帮你总结一些 **额外好用、可能你还没用到的功能**，可以让你的网站更现代、更实用：

---

## 🔹 1. **分页和文章导航**

* 支持自动分页，文章多时会显示“上一页 / 下一页”按钮
* 配置简单：

```yaml
params:
  paginate: 10  # 每页显示10篇文章
```

⚠️ 注意：新版 Hugo 0.148+ 要用 `pagination.pagerSize` 替代 `paginate`

---

## 🔹 2. **多语言支持**

* PaperMod 支持 i18n，方便做多语言网站

```yaml
languages:
  en:
    title: "Sun Weixiao's Blog"
    weight: 1
  zh:
    title: "孙韦校的个人主页"
    weight: 2
```

* Hugo 会根据用户浏览器自动切换语言

---

## 🔹 3. **快速返回顶部按钮**

* 页面滚动很长时会自动出现“返回顶部”按钮

```yaml
params:
  showBackToTop: true
```

---

## 🔹 4. **站内搜索**

* PaperMod 内置 Lunr.js 或 Fuse.js 搜索

```yaml
params:
  enableSearch: true
```

* 可以搜索文章标题、内容，用户体验好

---

## 🔹 5. **自定义 CSS / JS**

* 你可以在 `assets/css/custom.css` 或 `assets/js/custom.js` 写自己样式或交互
* Hugo + PaperMod 会自动加载这些文件

---

## 🔹 6. **文章阅读进度条**

* 顶部显示滚动进度条

```yaml
params:
  showReadingProgress: true
```

* 对长文章尤其友好

---

## 🔹 7. **评论系统增强**

* 除了 giscus、Utterances、Disqus，还可以用 **Valine / Twikoo / Waline**（国内可用）
* 甚至支持 **匿名 + GitHub OAuth 登录**

---

## 🔹 8. **RSS / Atom 支持**

* PaperMod 自动生成 RSS

```yaml
params:
  enableRSS: true
```

* 订阅者可以用 Feed 阅读文章

---

## 🔹 9. **文章摘要与封面图**

* 可以设置每篇文章的摘要（列表页显示）
* 支持封面图、背景图、文章卡片样式

---

## 🔹 10. **社交分享按钮**

* 每篇文章底部自动生成分享到 Twitter / Weibo / QQ / LinkedIn 的按钮

```yaml
params:
  enableShare: true
```

---

## 🔹 11. **Dark Mode 自动切换**

* 可以根据用户系统自动切换深色模式

```yaml
params:
  defaultTheme: auto
```

---

## 🔹 12. **弹窗提示 / 小组件**

* 可以自定义小弹窗，比如显示邮箱、公告、二维码
* 很适合展示联系方式或短期消息

---

💡 **总结**
PaperMod 可以让你的个人主页像大厂博客一样丰富：

* 外观：夜间模式、封面图、进度条、返回顶部
* 功能：评论、搜索、RSS、社交分享、统计
* 互动：弹窗、邮箱、二维码、通知

---

如果你愿意，我可以帮你写一份 **完整的 PaperMod 高级配置示例**，包括：

* 首页介绍 + 封面图
* 社交图标 + 邮箱弹窗
* Plausible 统计
* 夜间模式、进度条、返回顶部
* 评论系统（giscus）
* 搜索、RSS

这样你直接替换 `hugo.yaml` 就能马上生效。

你希望我帮你写吗？


```python
import torch
```
```cpp
#include <iostream>
#include <vector>
#include <string>
using namespace std;

struct Student {
    string name;
    int score;
};

int main() {
    int n;
    cout << "请输入学生数量: ";
    cin >> n;

    vector<Student> students;
    for (int i = 0; i < n; i++) {
        Student s;
        cout << "输入第 " << i + 1 << " 个学生的姓名: ";
        cin >> s.name;
        cout << "输入成绩: ";
        cin >> s.score;
        students.push_back(s);
    }

    int sum = 0;
    for (auto &s : students) {
        cout << "学生: " << s.name << " 成绩: " << s.score << endl;
        sum += s.score;
    }

    double avg = (n > 0) ? (double)sum / n : 0.0;
    cout << "平均成绩: " << avg << endl;
    return 0;
}

```