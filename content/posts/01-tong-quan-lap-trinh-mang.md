---
title: "Bài 1 — Tổng quan về lập trình mạng"
date: 2025-12-26
draft: false
tags: ["Java", "Lập trình mạng", "Network", "Overview"]
categories: ["Lập trình mạng"]
description: "Khái niệm cơ bản: client-server, TCP/UDP, port, socket, mô hình OSI/TCP-IP, flow dữ liệu."
---

## Mục tiêu
- Hiểu **client-server** là gì, dữ liệu đi như thế nào trên mạng.
- Phân biệt **TCP vs UDP**.
- Nắm các khái niệm: **IP, Port, Socket, Protocol**.

## 1) Lập trình mạng là gì?
Lập trình mạng là viết chương trình cho phép **2 (hoặc nhiều) máy** trao đổi dữ liệu qua mạng bằng các **giao thức** (protocol) như TCP/UDP/HTTP.

Một mô hình phổ biến:
- **Server**: lắng nghe (listen) tại một **port**
- **Client**: kết nối (connect) đến IP:port của server

## 2) Các mảnh ghép quan trọng
- **IP address**: địa chỉ máy (IPv4/IPv6)
- **Port**: cổng dịch vụ (0–65535). Ví dụ HTTP 80/443.
- **Socket**: “đầu nối” để app giao tiếp mạng
- **Protocol**: luật giao tiếp (TCP: tin cậy; UDP: nhanh)

## 3) TCP vs UDP (nhớ bằng 1 câu)
- **TCP**: “Gửi chắc chắn, có thứ tự, có kết nối” (reliable, ordered, connection-oriented)
- **UDP**: “Gửi nhanh, không hứa gì” (connectionless, best-effort)

## 4) Ví dụ cực nhỏ: kiểm tra DNS (InetAddress)
```java
import java.net.InetAddress;

public class DnsDemo {
    public static void main(String[] args) throws Exception {
        InetAddress addr = InetAddress.getByName("google.com");
        System.out.println("Host: " + addr.getHostName());
        System.out.println("IP: " + addr.getHostAddress());
    }
}
