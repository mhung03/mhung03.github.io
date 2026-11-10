---
title: "VLAN và Trunking cơ bản"
date: 2026-11-07
draft: false
description: "Tạo VLAN, gán port access và cấu hình trunk trên switch Cisco (bài mẫu)."
tags: ["ccna", "vlan", "trunk", "cisco"]
categories: ["Network"]
---

> **Bài mẫu** cho mục CCNA.

## Khái niệm

**VLAN** chia một switch thành nhiều mạng broadcast riêng. **Trunk** là đường nối mang nhiều VLAN giữa hai switch, gắn thẻ 802.1Q.

## Cấu hình

```text
Switch(config)# vlan 10
Switch(config-vlan)# name KETOAN
Switch(config)# interface fa0/1
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 10
Switch(config)# interface gi0/1
Switch(config-if)# switchport mode trunk
Switch(config-if)# switchport trunk allowed vlan 10,20
```

## Kiểm tra

```text
show vlan brief
show interfaces trunk
```

{{< alert "circle-info" >}}
Nhớ: access port thuộc **một** VLAN, trunk port mang **nhiều** VLAN.
{{< /alert >}}
