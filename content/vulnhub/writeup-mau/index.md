---
title: "Writeup mẫu: khung một bài Vulnhub"
date: 2026-11-06
draft: false
description: "Khung chuẩn: dò quét, khai thác, leo quyền và bài học."
tags: ["vulnhub", "writeup", "linux"]
categories: ["Vulnhub"]
---

> **Khung bài mẫu**, chưa phải máy thật. Mỗi lần làm máy mới, copy khung này rồi điền vào. Chỉ đăng writeup cho máy đã công khai.

## Thông tin máy

| Mục | Giá trị |
|---|---|
| Tên máy | ghi tên |
| Độ khó | Easy / Medium / Hard |
| Hệ điều hành | Linux |

## Quy trình

{{< mermaid >}}
graph LR
  A[Recon] --> B[Enumeration]
  B --> C[Foothold]
  C --> D[Privilege Escalation]
  D --> E[Root flag]
{{< /mermaid >}}

## 1. Recon

```bash
nmap -sC -sV -p- -oN nmap-full.txt <IP-MAY>
```

## 2. Enumeration

```bash
gobuster dir -u http://<IP-MAY> -w /usr/share/wordlists/dirb/common.txt
```

## 3. Foothold

Mô tả lỗ hổng tìm thấy và cách khai thác. Chèn ảnh bằng `![mô tả](ten-anh.png)`.

## 4. Privilege Escalation

Liệt kê cách leo quyền: `sudo -l`, SUID, cron, kernel.

## 5. Bài học

- Điều đã làm đúng
- Điều làm chậm và cách tránh lần sau
- Công cụ mới đã học
