---
title: "HTTP Request trong Java"
date: 2024-12-19
draft: false
tags: ["Java", "HTTP", "API"]
categories: ["Java"]
---

## HTTP Client trong Java

Java cung cấp nhiều cách để thực hiện HTTP request.

### Sử dụng HttpURLConnection
import java.net.*;
import java.io.*;

public class HttpExample {
    public static void main(String[] args) throws Exception {
        URL url = new URL("https://api.example.com/data");
        HttpURLConnection con = (HttpURLConnection) url.openConnection();
        con.setRequestMethod("GET");
        
        int responseCode = con.getResponseCode();
        System.out.println("Response Code: " + responseCode);
        
        BufferedReader in = new BufferedReader(
            new InputStreamReader(con.getInputStream()));
        String inputLine;
        StringBuffer response = new StringBuffer();
        
        while ((inputLine = in.readLine()) != null) {
            response.append(inputLine);
        }
        in.close();
        
        System.out.println(response.toString());
    }
}
```

### Java 11+ HttpClient
```java
import java.net.http.*;
import java.net.URI;

HttpClient client = HttpClient.newHttpClient();
HttpRequest request = HttpRequest.newBuilder()
    .uri(URI.create("https://api.example.com/data"))
    .build();

HttpResponse<String> response = client.send(request, 
    HttpResponse.BodyHandlers.ofString());
System.out.println(response.body());
