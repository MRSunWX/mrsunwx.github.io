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