---
title: "Bài 9 — Phân tán đối tượng trong Java bằng RMI"
layout: "featured"
date: 2025-12-26
draft: false
tags: ["Java", "RMI", "Distributed", "Remote"]
categories: ["Lập trình mạng"]
description: "Khái niệm Remote object, registry, stub/skeleton, ví dụ RMI Hello."
---

## Mục tiêu
- Hiểu RMI (Remote Method Invocation) = gọi method của object ở máy khác.
- Viết demo RMI Hello tối thiểu.

## 1) RMI là gì?
RMI giúp bạn gọi:
```java
remoteObj.sayHello()
```
như gọi local, nhưng thực tế chạy ở server.
## 2) Tạo Remote Interface
```java
import java.rmi.Remote;
import java.rmi.RemoteException;

public interface HelloService extends Remote {
    String hello(String name) throws RemoteException;
}
```
## 3) Implement service
```java 
import java.rmi.RemoteException;
import java.rmi.server.UnicastRemoteObject;

public class HelloServiceImpl extends UnicastRemoteObject implements HelloService {
    public HelloServiceImpl() throws RemoteException {}

    @Override
    public String hello(String name) throws RemoteException {
        return "Hello " + name + " from RMI!";
    }
}
```
## 4) Server: bind vào registry
```java
import java.rmi.registry.LocateRegistry;
import java.rmi.registry.Registry;

public class RmiServer {
    public static void main(String[] args) throws Exception {
        Registry registry = LocateRegistry.createRegistry(1099);
        registry.rebind("hello", new HelloServiceImpl());
        System.out.println("RMI Server ready");
    }
}
```
## 5) Client: lookup và gọi
```java 
import java.rmi.registry.LocateRegistry;
import java.rmi.registry.Registry;

public class RmiClient {
    public static void main(String[] args) throws Exception {
        Registry registry = LocateRegistry.getRegistry("127.0.0.1", 1099);
        HelloService svc = (HelloService) registry.lookup("hello");
        System.out.println(svc.hello("Boss"));
    }
}
```
Lỗi hay gặp

- Firewall chặn port 1099

- Sai hostname/IP khi getRegistry

- Classpath không đúng → ClassNotFoundException