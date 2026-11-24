---
title: "Lab 02: Cấu hình HA Firewall Fortigate"
date: 2026-11-23
draft: false
description: "HA hai FW Fortigate"
tags: ["Fortigate", "HA"]
---

>**Lưu ý.** Thiết bị sử trong bài lab là Image Fortigate 7.0.3, các phiên bản image khác thao tác tương tự.

## Mục tiêu
Cấu hình HA hai FW Fortigate để dự phòng khi 1 trong 2 FW gặp sự cố, đảm bảo dịch vụ không bị gián đoạn.

## Sơ đồ đấu nối
![alt text](images/image.png)
Để có thể cấu hình HA, hai FortiGate cần:
- Cùng model VM và phiên bản FortiOS tương thích, tốt nhất giống hệt nhau.
- Các interface có tên tương ứng: port1 đến port5.
- Có kết nối heartbeat trực tiếp như topology.
- HA group ID, group name và password giống nhau.
- Không cấu hình hai firewall độc lập cùng dùng IP 10.16.94.70 trước khi ghép HA.

## Cấu hình để truy cập giao diện GUI Fortigate

Vào CLI của Fortigate và cấu hình như sau
```
config system interface
    edit "port1"
        set mode static
        set ip 10.10.94.69 255.255.255.0
        set allowaccess ping https ssh
        set status up
    next
end
```
Cấu hình default route trên Fortigate
```
config router static
    edit 1
        set dst 0.0.0.0 0.0.0.0
        set gateway 10.16.94.1
        set device "port1"
    next
end
```
Từ một máy có thể truy cập tới dải 10.10.94.0/24, test ping tới IP wan của Fortigate
![alt text](images/image-1.png)

## Setup ban đầu và cấu hình LACP xuống Sw Core.
Chọn mục System => Setting => Đổi hostname 2 FW thành FGT-1, FGT-2.
![alt text](images/image-3.png)![alt text](images/image-5.png)

Tích vào mục Network => Interface => Create New để tạo LACP với 2 member là port2, port3.
![alt text](images/image-6.png)
Tiếp tục cấu hình 2 VLAN trên FW và đưa chúng vào LACP, Create new => Interface 
![alt text](images/image-7.png)![alt text](images/image-8.png) ![alt text](images/image-9.png)
Tạo policy để 2 VLAN đi được ra INTERNET
![alt text](images/image-18.png)
## Cấu hình HA hai Firewall Fortigate
Trên trình duyệt gõ https://10.16.94.70 để truy cập GUI, đối với FW còn lại, set 1 IP khác, ví dụ 10.16.94.71 cho cổng WAN để truy cập GUI, sau khi chúng HA, IP cổng WAN của FW passive sẽ được đồng bộ từ IP FW active.
![alt text](images/image-2.png)

Trong mục system, chọn HA và chuyển sang mode active-passive
![alt text](images/image-10.png)

| Thông số| Giải thích|
|--|--|
| Device Priority| Cao hơn sẽ làm active|
| Group name| Hai FW phải cùng Group name|
| Password| Password HA phải giống nhau|
| Monitor Interface| Port giám sát failover|
| Heartbeat| Đồng bộ HA|

Sang FGT-2 truy cập System => HA và cấu hình tương tự, chỉ thay đổi priority thấp hơn FGT-1.

Sau đó GUI FGT-2 sẽ không thể load được nữa
![alt text](images/image-12.png)
Chuyển qua FGT-1 vào HA để kiểm tra
![alt text](images/image-13.png)
Dùng câu lệnh sau để ép đồng bộ HA nhanh trên cả 2 FW
```
execute ha synchronized start
```
Cả 2 FW đều hiện tích xanh như này là đồng bộ thành công
![alt text](images/image-14.png)
Dùng câu lệnh sau để xem chi tiết trạng thái HA
```
get system ha status
```
![alt text](images/image-15.png)
Để từ FW active muốn SSH vào CLI của FW passive ta làm như sau
```
execute ha manage <member-id> <username>
```
![alt text](images/image-16.png)
## Cấu hình LACP trên Sw-Core
Trên Sw-Core tạo 2 Port-channel lên 2 FW tương ứng
![alt text](images/image-20.png)
![alt text](images/image-19.png)
## Test HA
Từ VPC ping -t ra internet và shutdown FGT-1 chỉ thấy time out 2 gói.
![alt text](images/image-24.png)
![alt text](images/image-22.png)
Bật FGT-1 lên và kiểm tra port-channel trên Sw-Core
![alt text](images/image-23.png)
Đến đây ta đã hoàn tất cấu hình LACP trên cặp FW Fortinet để dự phòng, đảm bảo dịch vụ hoạt động ổn định khi 1 trong 2 FW gặp sự cố.