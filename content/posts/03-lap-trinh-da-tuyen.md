---
title: "Bài 3 — Lập trình đa tuyến (Multithreading) trong ứng dụng mạng"
date: 2025-12-26
draft: false
slug: "03-lap-trinh-da-tuyen"
url: "/posts/03-lap-trinh-da-tuyen/"
tags: ["Java", "Thread", "Concurrency", "Network"]
categories: ["Lập trình mạng"]
description: "Thread, Runnable, race condition, synchronized, thread pool cơ bản."
---

## Mục tiêu
- Tạo thread bằng `Thread` / `Runnable`.
- Hiểu vì sao server mạng cần đa tuyến.
- Biết lỗi **race condition** và cách chặn cơ bản.

## 1) Vì sao mạng cần đa tuyến?
Server thường phải phục vụ **nhiều client cùng lúc**.
Nếu xử lý 1 client theo kiểu “độc quyền”, các client khác sẽ bị chờ.

## 2) Demo: tạo thread xử lý việc riêng
```java
public class ThreadDemo {
    public static void main(String[] args) {
        Runnable job = () -> {
            for (int i = 1; i <= 5; i++) {
                System.out.println(Thread.currentThread().getName() + " -> " + i);
            }
        };

        new Thread(job, "Worker-1").start();
        new Thread(job, "Worker-2").start();
    }
}
```

## 3) URL parsing  
Khi nhiều thread cùng sửa một biến chung → kết quả “hên xui”.

Fix đơn giản: synchronized

```java
class Counter {
    private int x = 0;
    public synchronized void inc() { x++; }
    public int get() { return x; }
}
```