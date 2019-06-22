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

Your Codec object will be instantiated and called as such:
```java
Codec codec = new Codec();
codec.decode(codec.encode(url));
```
这题其实算是 easy 题。

### Example
略

## Solution
用2个Map 倒来倒去

下面的做法是比较学院派的做法，做出来的short url是这种样子：`http://hello.com/x6yZ8`。如果不把 int count 转成 `x6yZ8` 这样的形式，则可以快一些。这样出来的结果像 `http://hello.com/2416780`。还可以更精简，比如 `http://2416780.com`

如果用62位来缩短int count，即转成 `x6yZ8` 这样的形式，则6位62位数就够了。因为 人类迄今为止的url的个数 << 62^6

这题可以问的clarification questions:
* `https` 的网站是否需要转成https，还是都转成http就行
* 会不会出现 `ftp://` 之类的东西

### Complexity
* Time: O(n)，n是给的string的长度
* Space: O(n)

### Java
代码看着不短，其实思路很简单
```java
public class Codec {
    Map<String, String> longAsKey = new HashMap<>();
    Map<String, String> shortAsKey = new HashMap<>();
    int count = 0;
    char[] chars;
    
    public Codec() {
        this.chars = new char[62]; // 62 = 26 * 2 + 10
        for (int i = 0; i < 10; i++) {
            chars[i] = (char)('0' + i);
        }
        for (int i = 0; i < 26; i++) {
            chars[i + 10] = (char)('a' + i);
        }
        for (int i = 0; i < 26; i++) {
            chars[i + 36] = (char)('A' + i);
        }
    }
    
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
        sb.append(".com/");
        count++;
        sb.append(convertCountToStr(count));
        
        String shortUrl = sb.toString();
        
        longAsKey.put(longUrl, shortUrl);
        shortAsKey.put(shortUrl, longUrl);
        
        return shortUrl;
    }

    // Decodes a shortened URL to its original URL.
    public String decode(String shortUrl) {
        return shortAsKey.get(shortUrl);
    }
    
    private String convertCountToStr(int count) {
        StringBuilder sb = new StringBuilder();
        sb.append(chars[count % 62]);
        count /= 62;
        
        while (count >= 62) {
            sb.append(chars[count % 62]);
            count /= 62;
        }
        if (count > 0) sb.append(chars[count]); 

        return sb.toString();
    }
}
```

## Reference
* [Encode and Decode TinyURL [LeetCode]](https://leetcode.com/problems/encode-and-decode-tinyurl/description/)

{% include links.html %}
