---
name: 🐛 Bug Report
description: 报告游戏中的 Bug 或问题
title: "[Bug] 简要描述问题"
labels: ["bug", "needs-triage"]
body:
  - type: markdown
    attributes:
      value: |
        感谢报告问题！请尽量提供详细信息。
  - type: textarea
    id: description
    attributes:
      label: 问题描述
      description: 清晰简洁地描述问题
      placeholder: 例如：机枪塔在第 3 关不攻击丧尸...
    validations:
      required: true
  - type: textarea
    id: reproduction
    attributes:
      label: 复现步骤
      description: 如何复现这个问题
      placeholder: |
        1. 打开第 3 关
        2. 建造机枪塔
        3. 等待丧尸出现
        4. 观察...
    validations:
      required: true
  - type: input
    id: version
    attributes:
      label: 游戏版本
      placeholder: v0.1.0
    validations:
      required: true
  - type: dropdown
    id: platform
    attributes:
      label: 平台
      options:
        - Windows
        - macOS
        - Android
        - iOS
        - Web
    validations:
      required: true
  - type: textarea
    id: logs
    attributes:
      label: 日志文件
      description: 如有错误日志，请粘贴
      render: shell
    validations:
      required: false
