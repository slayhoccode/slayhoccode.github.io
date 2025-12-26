---
title: "Bài 4 — Quản lý địa chỉ kết nối mạng (IP, Port, DNS, URL)"
date: 2025-12-26
draft: false
tags: ["Java", "InetAddress", "URL", "DNS", "Network"]
categories: ["Lập trình mạng"]
description: "Làm việc với InetAddress, URL/URI, port, localhost, subnet cơ bản."
---

## Mục tiêu
- Dùng `InetAddress` lấy IP, host.
- Dùng `URL` đọc thông tin endpoint.
- Hiểu localhost/127.0.0.1 và port.

## 1) Localhost là ai?
`localhost` → thường map tới `127.0.0.1` (IPv4) hoặc `::1` (IPv6)  
Nó là **máy của chính bạn**.

## 2) InetAddress
```java
import java.net.InetAddress;

public class AddressDemo {
    public static void main(String[] args) throws Exception {
        InetAddress local = InetAddress.getLocalHost();
        System.out.println("Local: " + local.getHostAddress());

        InetAddress google = InetAddress.getByName("google.com");
        System.out.println("Google IP: " + google.getHostAddress());
    }
}
```

## 3) URL parsing
```java
import java.net.URL;

public class UrlDemo {
    public static void main(String[] args) throws Exception {
        URL url = new URL("https://example.com:8080/path?q=1");
        System.out.println(url.getProtocol()); // https
        System.out.println(url.getHost());     // example.com
        System.out.println(url.getPort());     // 8080
        System.out.println(url.getPath());     // /path
        System.out.println(url.getQuery());    // q=1
    }
}
```