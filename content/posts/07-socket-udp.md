---
title: "Bài 7 — Lập trình mạng UDP trong Java"
layout: "featured"
date: 2025-12-26
draft: false
tags: ["Java", "UDP", "DatagramSocket", "Network"]
categories: ["Lập trình mạng"]
description: "UDP server/client với DatagramSocket, gửi nhận datagram, lưu ý mất gói."
---

## Mục tiêu
- Gửi/nhận UDP packet với `DatagramSocket`.
- Hiểu UDP không kết nối, có thể mất gói.

## 1) UDP Receiver (Server)
```java
import java.net.*;
import java.nio.charset.StandardCharsets;

public class UdpReceiver {
    public static void main(String[] args) throws Exception {
        try (DatagramSocket socket = new DatagramSocket(9200)) {
            byte[] buf = new byte[1024];
            DatagramPacket packet = new DatagramPacket(buf, buf.length);

            System.out.println("UDP Receiver on 9200...");
            socket.receive(packet);

            String msg = new String(packet.getData(), 0, packet.getLength(), StandardCharsets.UTF_8);
            System.out.println("From " + packet.getAddress() + ":" + packet.getPort() + " -> " + msg);
        }
    }
}
```
## 2) UDP Sender (Client)
```java
import java.net.*;
import java.nio.charset.StandardCharsets;

public class UdpSender {
    public static void main(String[] args) throws Exception {
        try (DatagramSocket socket = new DatagramSocket()) {
            byte[] data = "Hello UDP".getBytes(StandardCharsets.UTF_8);
            InetAddress host = InetAddress.getByName("127.0.0.1");

            DatagramPacket packet = new DatagramPacket(data, data.length, host, 9200);
            socket.send(packet);
        }
    }
}
```
Lỗi hay gặp

- Buffer quá nhỏ → bị cắt message

- UDP “không nhận” do firewall/chặn port

- Tưởng UDP luôn nhận đủ → không, phải tự thiết kế retry/ACK nếu cần