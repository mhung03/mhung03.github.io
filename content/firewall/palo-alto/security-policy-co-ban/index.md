---
title: "Security Policy cơ bản trên Palo Alto"
date: 2026-11-08
draft: false
description: "Zone, rule và thứ tự khớp rule trong PAN-OS (bài mẫu)."
tags: ["pan-os", "security-policy", "zone"]
categories: ["Firewall"]
---

> **Bài mẫu.** Thay nội dung bằng ghi chú thật của bạn.

## Ý chính

Palo Alto quyết định cho phép hay chặn dựa trên **zone** và **application**, không chỉ cổng và IP.

{{< mermaid >}}
graph LR
  A[Gói tin vào] --> B[Zone nguồn]
  B --> C{Khớp rule?}
  C -- Có --> D[Allow / Deny]
  C -- Không --> E[Default rule]
  D --> F[Zone đích]
{{< /mermaid >}}

## Các thành phần cần nhớ

- **Zone**: nhóm interface có cùng mức tin cậy (trust, untrust, dmz).
- **Security rule**: nguồn, đích, application, service, action.
- **Thứ tự**: khớp rule đầu tiên từ trên xuống.

## Ví dụ rule

| Tên | Từ | Đến | Application | Action |
|---|---|---|---|---|
| Allow-Web | trust | untrust | web-browsing, ssl | allow |
| Block-Rest | any | any | any | deny |

## Việc cần làm tiếp

- [ ] Thêm ảnh chụp giao diện
- [ ] Ghi lại lệnh `show running security-policy`
