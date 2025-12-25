---
title: "Socket Programming trong Java"
date: 2024-12-20
draft: false
tags: ["Java", "Network", "Socket"]
categories: ["Java"]
---

## Giới thiệu về Socket Programming

Socket là endpoint của một kết nối hai chiều giữa hai chương trình chạy trên mạng.

### Các loại Socket

1. **TCP Socket**: Kết nối tin cậy, có trình tự
2. **UDP Socket**: Không kết nối, nhanh hơn

### Ví dụ TCP Server
import java.net.*;
import java.io.*;

public class SimpleServer {
    public static void main(String[] args) throws IOException {
        ServerSocket serverSocket = new ServerSocket(8080);
        System.out.println("Server đang chờ kết nối...");
        
        Socket clientSocket = serverSocket.accept();
        System.out.println("Client đã kết nối!");
        
        BufferedReader in = new BufferedReader(
            new InputStreamReader(clientSocket.getInputStream()));
        PrintWriter out = new PrintWriter(
            clientSocket.getOutputStream(), true);
        
        String inputLine;
        while ((inputLine = in.readLine()) != null) {
            System.out.println("Nhận: " + inputLine);
            out.println("Echo: " + inputLine);
        }
    }
}

### Kết luận

Socket programming là nền tảng quan trọng trong lập trình mạng với Java.