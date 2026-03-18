---
title: "Bài 5 — Lập trình Socket TCP trong Java"
layout: "featured"
date: 2025-12-26
draft: false
tags: ["Java", "TCP", "Socket", "Client-Server"]
categories: ["Lập trình mạng"]
description: "Xây dựng TCP Server/Client, read/write stream, xử lý nhiều client."
---

## Mục tiêu
- Viết được TCP server/client tối thiểu.
- Hiểu vòng đời: `ServerSocket` listen → `accept()` → `Socket` trao đổi dữ liệu.

## 1) TCP Echo Server (1 client)
```java
import java.io.*;
import java.net.*;

public class TcpEchoServer {
    public static void main(String[] args) throws Exception {
        int port = 9000;
        try (ServerSocket server = new ServerSocket(port)) {
            System.out.println("TCP Server listening on " + port);

            try (Socket socket = server.accept();
                 BufferedReader in = new BufferedReader(new InputStreamReader(socket.getInputStream()));
                 BufferedWriter out = new BufferedWriter(new OutputStreamWriter(socket.getOutputStream()))) {

                String line;
                while ((line = in.readLine()) != null) {
                    out.write("ECHO: " + line);
                    out.newLine();
                    out.flush();
                }
            }
        }
    }
}
```
## 2) TCP Client
```java
import java.io.*;
import java.net.*;

public class TcpClient {
    public static void main(String[] args) throws Exception {
        try (Socket socket = new Socket("127.0.0.1", 9000);
             BufferedReader in = new BufferedReader(new InputStreamReader(socket.getInputStream()));
             BufferedWriter out = new BufferedWriter(new OutputStreamWriter(socket.getOutputStream()));
             BufferedReader console = new BufferedReader(new InputStreamReader(System.in))) {

            System.out.print("Nhap: ");
            String msg = console.readLine();
            out.write(msg);
            out.newLine();
            out.flush();

            System.out.println("Server: " + in.readLine());
        }
    }
}
```
## 3) Nâng cấp: nhiều client
Ý tưởng: mỗi lần accept() → tạo thread handle client.

Lỗi hay gặp

- Quên flush() → client/server “đơ”.

- Dùng readLine() nhưng bên kia không gửi newline → treo.

- Port bị chiếm: “Address already in use”.