---
name: 💡 Feature Request
description: 提出新功能建议或改进意见
title: "[Feature] 简要描述功能"
labels: ["enhancement", "needs-triage"]
body:
  - type: markdown
    attributes:
      value: |
        感谢提出建议！请详细描述你的想法。
  - type: textarea
    id: problem
    attributes:
      label: 相关问题
      description: 这个功能解决了什么问题？
      placeholder: 例如：现在防御塔升级太繁琐，希望能一键升级...
    validations:
      required: true
  - type: textarea
    id: solution
    attributes:
      label: 建议方案
      description: 你希望如何实现这个功能？
      placeholder: 描述你理想中的实现方式
    validations:
      required: true
  - type: textarea
    id: alternatives
    attributes:
      label: 其他方案
      description: 有没有其他替代方案？
    validations:
      required: false
  - type: dropdown
    id: priority
    attributes:
      label: 优先级
      options:
        - 🌟 低 (有了更好)
        - ⭐⭐ 中 (应该要有)
        - ⭐⭐⭐ 高 (必须有)
    validations:
      required: true
