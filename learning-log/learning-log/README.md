# Learning Log · 嵌入式 / 端侧 AI 部署 / Agent

记录我从 2026 年 10 月开始的学习过程：每天的学习笔记、练习代码和项目进展。

## 关于我

- CS 硕士在读，2028 届，有 C 语言基础，正在学习 Python 和 C++
- 目标方向：嵌入式软件、端侧 AI 部署、大模型应用 / Agent 开发
- 目标：2027 年暑期实习，2027 年秋招

## 学习路线

| 阶段 | 时间 | 内容 | 里程碑 | 详细计划 |
| --- | --- | --- | --- | --- |
| 第 1 个月 | 2026.10.26 – 11.22 | Python 工程化、C/C++、Linux、STM32 外设 | 能写多线程 C++ 程序，能独立配置 STM32 常用外设 | [month-01](plans/month-01/README.md) |
| 第 2 个月 | 2026.11.23 – 12.20 | STM32 通信外设、CAN、FreeRTOS、ESP32-S3、开源 RTOS | ESP32 设备能联网上报并响应指令 | [month-02](plans/month-02/README.md) |
| 第 3 个月 | 2026.12.21 – 2027.1.17 | 大模型 API、RAG、LangGraph、MCP、评测 | Agent 部署成服务，有评测结果 | [month-03](plans/month-03/README.md) |
| 第 4 个月 | 2027.1.18 – 2.14 | ONNX、量化、RK3588 部署大模型 | 公开的端侧大模型性能实测报告 | [month-04](plans/month-04/README.md) |
| 第 5 个月 | 2027.2.15 – 3.14 | 嵌入式 Linux 驱动、开源贡献、简历 | 开源 PR 进入评审，首批实习投递发出 | [month-05](plans/month-05/README.md) |
| 第 6 个月 | 2027.3.15 – 4.25 | 基础课复习、面试准备、持续投递 | 拿到暑期实习 offer | [month-06](plans/month-06/README.md) |
| 实习之后 | 2027.5 – 11 | 暑期实习、整合演示、行业知识、秋招 | 秋招拿到满意的 offer | [after-internship](plans/after-internship/README.md) |

## 进度

- [ ] 第 1 个月：共同底座
- [ ] 第 2 个月：嵌入式
- [ ] 第 3 个月：Agent
- [ ] 第 4 个月：端侧部署
- [ ] 第 5 个月：嵌入式 Linux、开源与简历
- [ ] 第 6 个月：暑期实习求职
- [ ] 实习与秋招

## 每日安排

| 时间段 | 工作日（约 4 小时） | 周末（约 6 小时） |
| --- | --- | --- |
| 早上 30 分钟 | 刷 1 道算法题 | 刷 2 道算法题 |
| 第一块（约 2 小时） | 学当天新内容 | 周六做本周小项目，周日补漏 |
| 第二块（约 1 小时） | 动手完成当天练习 | 继续项目或练习（约 2 小时） |
| 最后 15 分钟 | 写记录、提交、列明天任务 | 周日晚周复盘 |

每日记录模板见 [plans/daily-template.md](plans/daily-template.md)。

## 仓库结构

```
learning-log/
├── README.md
├── plans/            # 每月详细计划和每日记录模板
│   ├── month-01/README.md
│   ├── ...
│   ├── month-06/README.md
│   ├── after-internship/README.md
│   └── daily-template.md
├── daily/            # 每日学习记录，如 daily/2026-10-26.md
├── leetcode/         # 算法题解
├── python/           # Python 练习
├── c-cpp/            # C/C++ 练习
├── embedded/         # STM32、ESP32 练习代码
└── notes/            # 专题笔记（如 C 语言易错点、FreeRTOS 总结、面试复盘）
```

## 项目

完成后会在这里链接到各自的独立仓库：

- Agent 项目：待完成
- 端侧部署实测报告：待完成
- 嵌入式项目：待完成
- 开源贡献：待完成
