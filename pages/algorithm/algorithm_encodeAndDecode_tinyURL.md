---
title: "Encode and Decode Tiny URL"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_encodeAndDecode_tinyURL.html
folder: algorithm
toc: false
---

## Description
Note: This is a companion problem to the System Design problem: Design TinyURL.

TinyURL is a URL shortening service where you enter a URL such as https://leetcode.com/problems/design-tinyurl and it returns a short URL such as http://tinyurl.com/4e9iAk.

Design the encode and decode methods for the TinyURL service. There is no restriction on how your encode/decode algorithm should work. You just need to ensure that a URL can be encoded to a tiny URL and the tiny URL can be decoded to the original URL.

### Example
略

## Solution
用2个Map 倒来倒去

### Complexity
* Time: O(n)，n是给的string的长度
* Space: O(n)

### Java
```java
public class Codec {
    Map<String, String> longAsKey = new HashMap<>();
    Map<String, String> shortAsKey = new HashMap<>();
    int count = 0;
    
    // Encodes a URL to a shortened URL.
    public String encode(String longUrl) {
        List<Character> lastChars = new ArrayList<>();
        for (int i = longUrl.length() - 1; i >= 0; i--) {
            char c = longUrl.charAt(i);
            if (c != '.' && c != '-') {
                lastChars.add(c);
            } else {
                break;
            }
        }
        
        StringBuilder sb = new StringBuilder();
        sb.append("http://");
        for (int i = lastChars.size() - 1; i >= 0; i--) {
            sb.append(lastChars.get(i));
        }
        sb.append(".com");
        count++;
        sb.append(String.valueOf(count));
        
        String shortUrl = sb.toString();
        
        longAsKey.put(longUrl, shortUrl);
        shortAsKey.put(shortUrl, longUrl);
        
        return shortUrl;
    }

    // Decodes a shortened URL to its original URL.
    public String decode(String shortUrl) {
        return shortAsKey.get(shortUrl);
    }
}

// Your Codec object will be instantiated and called as such:
// Codec codec = new Codec();
// codec.decode(codec.encode(url));
```

## Reference
* [Encode and Decode TinyURL [LeetCode]](https://leetcode.com/problems/encode-and-decode-tinyurl/description/)

{% include links.html %}
