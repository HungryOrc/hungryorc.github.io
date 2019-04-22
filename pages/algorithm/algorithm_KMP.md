---
title: "KMP (Knuth Morris Pratt) Pattern Searching Algorithm"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_KMP.html
folder: algorithm
toc: false
---

## Overview
https://en.wikipedia.org/wiki/Knuth%E2%80%93Morris%E2%80%93Pratt_algorithm

KMP 是用 O(n + m) 时间完成在一个 String `source` (长度n) 里寻找一个 Sub String `word` (长度m) 的算法。它的思想内核是：所有信息都已经包含在
`word`里了。我们在`word`里的任何一个位置发生匹配失败的时候，都能在`word`里的某个位置重新开始比对，而未必需要总是回到`word`的第一个char，
那么到底回到哪一位，我们是可以从`word`本身完全知道的，和任何`source`都没有关系

那么对于`word`里的每一位，匹配失败后回到哪一位去继续找，这些信息我们用一个所谓的 Rollback Array `T` 来表示，这个 T 就是 KMP 算法的核心。举例：
```java
String source = "TRY PARTICIPATE IN PARACHUTE, IT WILL THROW THE GUT OUT OF YOU!";
String word = "PARTICIPATE IN PARACHUTE"; // the String to find
```
```
index i   0  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 21 22 23
word[i]   P  A  R  T  I  C  I  P  A  T  E     I  N     P  A  R  A  C  H  U  T  E
T[i]     -1  0  0  0  0  0  0  0  1  2  0  0  0  0  0  0  1  2  3  0  0  0  0  0
```
在source里找word的过程，可以想象为word是一个短尺子，source是一个长的尺子。一开始两个尺子左端对齐。然后不断把短尺word往右边移动。
我们逐位分析上面的array T 里各个元素的意思：
* i = 0 时，word[i] = P，T[i] = -1。这个 -1 的意思是：如果在source里的某一位，和word里的第一位相比都不能匹配，即source里的这一位不是P，那么没办法，只能把整个word向右移动一位。这就是这个 -1 想要表达的意思。
* i = 1 时，word里是A，T里是0。这个0的意思是：如果在source里的某一位，它不匹配word的里的第二位，即source里的这一位不是A。但source里紧邻这一位左边的一位是P，它已经吻合了word里的第一位。那么是否能有取巧的办法？抱歉在这个case里是没有的。但后面其他case里有。就这个case来说，只能回到word的第一位去重新开始比source里的这一位。这个“第一位”就用 0 来表示
* i = 8 时，word里又是A，T里这下不是0了，是1。意思是如果source里的某一位之前的8位都各自符合了这个A前面的8个字母 PARTICIP，然后到了这个A的时候不匹配了，那么重新找匹配的时候，不必从word头部的第一个P开始找，而是从word头部第二位的A开始找就行了。原因是这个不匹配的A前面正好是一个P，那么这个PA和word头部的PA正好是吻合的
* i= 18 时的A是一个更强的例子。它前面的3位 PAR 都和word的前三位PAR 相同，所以如果在这个index为18的A上发生了不匹配（前提是他前面的18个char都匹配了），那么再匹配的时候从word里的index为3的那个char开始check就好了

综合以上我们可以看出，**对于`word[i]`，`T[i]`是多大和`word[i]`本身是毫无关系的，只和 `word[0] ~ word[i - 1]` 有关系**

### Complexity
* Time: 
* Space: 

## Implementation
### Java
```java
public class KMP_Algorithm {
	// return: an integer, the zero-based position in source at which the word is found
	// returning -1 means no occurrence of the word in the source
	public static int KMP(String source, String word) {
		int m = 0; // the beginning of the current match in source string
	    int i = 0; // the position of the current character in the word
	    int[] T = findRollbackArray(word); // the "rollback array" of the word
	    
	    char[] w = word.toCharArray();
	    char[] s = source.toCharArray();

	    while (m + i < s.length)
	    {
	        if (s[m + i] == w[i])
	        {
	        	// reached the end of word, namely the search was successful
	            if (i == w.length - 1) 
	                return m;
				else
					i ++;
	        }
	        else // s[m+i] != w[i]
	        {
	            if (T[i] > -1) // not the first char in the word, namely i != 0
	            {
	            	// reset the starting point of the search in the source
	            	m = m + i - T[i];
	            	// reset the current char to be compared in the word
	            	i = T[i];
	            }
	            else
	            {
	            	m ++;
	            	i = 0;
	            }
	        }
	    }       
	    // if we reach here, we have searched all of S unsuccessfully)
	    return -1; // -1 means failure
	}
	
	// 计算任何一个给定的String: word 的 Rollback Array T
	public static int[] findRollbackArray(String word) {
		int len = word.length();
		int[] T = new int[len];
		
		// 变量 cnd 的意义：
		// 我们当前在 word 里处理的 char，再往前的一个 char 是与 index为cnd的 word里的char 作比较
		// 比如上文举例的 Rollback Array T,  
		// 它里面的 17 号char为R，R的前一个char是A，A的T[i]等于1，这个 1 就是 R 的 cnd
		int cnd = 0; 
		
		if (len >= 1)
			T[0] = -1;
		if (len >= 2)
			T[1] = 0;
		if (len <= 2)
			return;
		
		// the current position we are computing in T
		int pos = 2; 
		char[] w = word.toCharArray();
		
		while (pos < w.length)
		{
	        //first case: the substring continues to match
	        if (w[pos - 1] == w[cnd])
	        {
	            T[pos] = cnd + 1;
	            cnd++;
	            pos++;
	        }
	        // second case: it can not continue, but we have something to fall back
	        else if (cnd > 0) // and w[pos-1] != w[cnd]
	        {
	        	// 全算法的最精华在这一句！！！
	        	// 对比的index(即cnd)，被对比的index的rollback量(即T[cnd])所代替！！！
				/* 举例：上面那个word的例子里，i=19时，cnd=3>0（即C前面的A下面的那个3），
				 这时候cnd就要从3变成 T[3]=0。然后进入下一次while loop，再进入下面的第三个else分支 */
	            cnd = T[cnd];
	                       
	            // we do NOT increase pos here! Since we are not done with pos yet!
	        }
	        // third case: we have run out of candidates, note that we now have cnd == 0
	        else // w[[pos-1] != w[cnd] && cnd == 0
	        {
	        	T[pos] = 0;
	        	pos++;
	        }
		}
	}
	
	
	public static void main(String[] args) {
		
		String source = "TRY PARTICIPATE IN PARACHUTE, IT WILL THROW THE GUT OUT OF YOU!";
		String word = "PARTICIPATE IN PARACHUTE";
		
		int[] T = new int[word.length()];
		findRollbackArray(word, T);
		
		System.out.println("The Rollback Array of the given word \"" + word + "\" is:");
		for (int i : T)
			System.out.print(i + " ");
		System.out.println();
		
		System.out.println("The index of the 1st occurence of the word in the source is: "
				+ KMP(source, word));
	}

}
```

## Reference

{% include links.html %}
