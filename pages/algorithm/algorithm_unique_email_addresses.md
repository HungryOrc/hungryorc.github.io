---
title: "Unique Email Addresses"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_unique_email_addresses.html
folder: algorithm
toc: false
---

## Description
Every email consists of a local name and a domain name, separated by the @ sign.

For example, in alice@leetcode.com, alice is the local name, and leetcode.com is the domain name.

Besides lowercase letters, these emails may contain '.'s or '+'s.

If you add periods ('.') between some characters in the local name part of an email address, mail sent there will be forwarded to the same address without dots in the local name.  For example, "alice.z@leetcode.com" and "alicez@leetcode.com" forward to the same email address.  (Note that this rule does not apply for domain names.)

If you add a plus ('+') in the local name, everything after the first plus sign will be ignored. This allows certain emails to be filtered, for example m.y+name@email.com will be forwarded to my@email.com.  (Again, this rule does not apply for domain names.)

It is possible to use both of these rules at the same time.

Given a list of emails, we send one email to each address in the list.  How many different addresses actually receive mails? 

Note:
* 1 <= emails[i].length <= 100
* 1 <= emails.length <= 100
* Each emails[i] contains exactly one '@' character.
* All local and domain names are non-empty.
* Local names do not start with a '+' character.

### Example
* Input: ["test.email+alex@leetcode.com","test.e.mail+bob.cathy@leetcode.com","testemail+david@lee.tcode.com"]
  * Output: 2
    * "testemail@leetcode.com" and "testemail@lee.tcode.com" actually receive mails

## Solution：朴素的想法，速度前7%
题目不难，情况也不多。仔细想清楚各种情况就好
* 先发现'+'还没发现'@'
* 先发现'@'还没发现'+'
* 发现'@'之后发现'+'或者发现'.'

### Complexity
* Time: O(n * m)，n是有多少个email string，m是每个email string的平均长度
* Space: O(n * m)，hashset的size

### Java
```java
class Solution {
    public int numUniqueEmails(String[] emails) {
        if (emails == null || emails.length == 0) {
            return 0;
        }
        
        Set<String> set = new HashSet<>();
        
        for (String s : emails) {
            StringBuilder sb = new StringBuilder();
            boolean foundAtMark = false;
            boolean foundFirstPlus = false;
                
            for (char c : s.toCharArray()) {
                if (!foundAtMark) {
                    if (!foundFirstPlus) {
                        if (c == '+') {
                            foundFirstPlus = true;
                        } else if (c != '.') {
                            sb.append(c);
                        }
                    }
                    
                    if (c == '@') {
                        foundAtMark = true;
                        sb.append('@');
                    } 
                } 
                else { // foundAtMark
                    sb.append(c);
                }
            }
            set.add(sb.toString());
        }
        return set.size();
    }
}
```

## Reference
* [Unique Email Addresses [LeetCode]](https://leetcode.com/problems/unique-email-addresses/description/)

{% include links.html %}
