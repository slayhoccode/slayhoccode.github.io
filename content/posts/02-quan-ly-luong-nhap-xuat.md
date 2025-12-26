---
title: "Bài 2 — Quản lý các luồng nhập xuất (I/O Streams)"
layout: "featured"
date: 2025-12-26
draft: false
tags: ["Java", "I/O", "Stream", "Buffered", "Lập trình mạng"]
categories: ["Lập trình mạng"]
description: "InputStream/OutputStream, Reader/Writer, Buffered, đóng stream đúng cách, text vs binary."
---

## Mục tiêu
- Dùng **InputStream/OutputStream** và **Reader/Writer** đúng ngữ cảnh.
- Biết vì sao cần **Buffered**.
- Biết cách **đóng stream** an toàn bằng try-with-resources.

## 1) Text vs Binary
- **Binary**: ảnh, file zip, bytes thô → dùng `InputStream/OutputStream`
- **Text**: chuỗi có encoding (UTF-8) → dùng `Reader/Writer`

## 2) Buffered giúp gì?
Giảm số lần gọi hệ điều hành → nhanh hơn, mượt hơn.

## 3) Demo đọc/ghi text UTF-8
```java
import java.io.*;
import java.nio.charset.StandardCharsets;

public class TextIODemo {
    public static void main(String[] args) throws Exception {
        String path = "note.txt";
        try (Writer w = new BufferedWriter(new OutputStreamWriter(
                new FileOutputStream(path), StandardCharsets.UTF_8))) {
            w.write("Hello mạng máy tính!\n");
        }

        try (BufferedReader br = new BufferedReader(new InputStreamReader(
                new FileInputStream(path), StandardCharsets.UTF_8))) {
            System.out.println(br.readLine());
        }
    }
}
