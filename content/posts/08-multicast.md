---
title: "Bài 8 — Lập trình Multicast trong Java"
layout: "featured"
date: 2025-12-26
draft: false
tags: ["Java", "Multicast", "UDP", "MulticastSocket"]
categories: ["Lập trình mạng"]
description: "Gửi 1 gói cho nhiều máy: joinGroup, multicast address, lưu ý mạng nội bộ."
---

## Mục tiêu
- Hiểu multicast dùng cho broadcast nhóm.
- Viết demo join group + nhận message.

## 1) Multicast là gì?
Multicast = gửi dữ liệu tới **một nhóm địa chỉ** (ví dụ 239.x.x.x) để nhiều client cùng nhận.

## 2) Multicast Receiver
```java
import java.net.*;
import java.nio.charset.StandardCharsets;

public class MulticastReceiver {
    public static void main(String[] args) throws Exception {
        InetAddress group = InetAddress.getByName("239.1.2.3");
        int port = 9300;

        try (MulticastSocket socket = new MulticastSocket(port)) {
            socket.joinGroup(group);

            byte[] buf = new byte[1024];
            DatagramPacket packet = new DatagramPacket(buf, buf.length);

            System.out.println("Joined multicast group " + group + ":" + port);
            socket.receive(packet);

            String msg = new String(packet.getData(), 0, packet.getLength(), StandardCharsets.UTF_8);
            System.out.println("Received: " + msg);

            socket.leaveGroup(group);
        }
    }
}
```
## 3) Multicast Sender
```java
import java.net.*;
import java.nio.charset.StandardCharsets;

public class MulticastSender {
    public static void main(String[] args) throws Exception {
        InetAddress group = InetAddress.getByName("239.1.2.3");
        int port = 9300;

        try (DatagramSocket socket = new DatagramSocket()) {
            byte[] data = "Hello Multicast".getBytes(StandardCharsets.UTF_8);
            DatagramPacket packet = new DatagramPacket(data, data.length, group, port);
            socket.send(packet);
        }
    }
}
```
Lưu ý thực tế

- Multicast thường chạy tốt trong LAN, router có thể chặn.

- Nếu không nhận được: kiểm tra network adapter / firewall.