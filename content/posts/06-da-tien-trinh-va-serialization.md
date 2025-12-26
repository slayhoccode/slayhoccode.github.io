---
title: "Bài 6 — Đa tiến trình & Tuần tự hoá đối tượng (Serialization) trong ứng dụng mạng"
date: 2025-12-26
draft: false
tags: ["Java", "Process", "Serialization", "ObjectStream", "Network"]
categories: ["Lập trình mạng"]
description: "ProcessBuilder cơ bản + gửi object qua mạng bằng ObjectInputStream/ObjectOutputStream."
---

## Mục tiêu
- Hiểu “tiến trình” vs “luồng”.
- Biết cách **serialize object** để gửi qua TCP.

## 1) Tiến trình vs luồng
- **Process**: chương trình độc lập, bộ nhớ riêng
- **Thread**: chạy trong cùng process, chia sẻ bộ nhớ

## 2) Serialization là gì?
Biến object thành bytes để gửi/ghi file.

### Ví dụ: class gửi qua mạng
```java
import java.io.Serializable;

public class Message implements Serializable {
    private static final long serialVersionUID = 1L;
    public String from;
    public String text;

    public Message(String from, String text) {
        this.from = from; this.text = text;
    }

    @Override public String toString() {
        return from + ": " + text;
    }
}
```
## 3) Gửi object qua TCP
Server

```java
import java.io.*;
import java.net.*;

public class ObjServer {
    public static void main(String[] args) throws Exception {
        try (ServerSocket server = new ServerSocket(9100);
             Socket s = server.accept();
             ObjectInputStream ois = new ObjectInputStream(s.getInputStream())) {

            Object obj = ois.readObject();
            System.out.println("Received: " + obj);
        }
    }
}
```
Client
```java
import java.io.*;
import java.net.*;

public class ObjClient {
    public static void main(String[] args) throws Exception {
        try (Socket s = new Socket("127.0.0.1", 9100);
             ObjectOutputStream oos = new ObjectOutputStream(s.getOutputStream())) {

            oos.writeObject(new Message("Boss", "Hello serialization!"));
            oos.flush();
        }
    }
}
```

Lỗi hay gặp
- Quên implements Serializable

- Không khai báo serialVersionUID → dễ lệch version khi sửa class

- Tạo ObjectInputStream/ObjectOutputStream sai thứ tự → deadlock (cả hai chờ header)