---
title: Bill Total Value
date: 2021-07-08
categories:
- WJC
tags: 
- codeforces
---

<a href="https://codeforces.com/problemset/problem/727/B" target="_blank">原题链接</a>
# 题目翻译
这里有多张钞票， 每张钞票都由名字加面额表示， 钞票之间没有空格， 两张名字相同的钞票面额相同。对于面额， 一定满足以下条件：

  1. 只由数字和 '.' 组成
  2. 小数部分与整数部分由 '.' 连接
  3. 小数部分有且仅有两位， 没有的用 $0$ 补全。 如： $0.10$ , $14.03$ , $124.42$
  4. 整数部分每三位由 '.' 隔开。 如： $12.390$ $23.543.475$
  5. '.' 不能作为开头
 
例：

  * "$234$", "$1.544$", "$149.431.10$", "$0.99$", "$123.05$" 满足条件
  * "$.333$", "$3.33.11$", "$12.00$", "$.33$", "$0.1234$", "$1.2$" 不满足条件

请求出所有钞票的总面额， 并按照上述规则输出

# 思路详解

这道题其实比较明显的就是一道大模拟。 首先可以将名字给直接过滤掉， 就把它当成面额之间的分割就行了。 每个面额就直接用一个 *string* 先存着， 接着开始用 '.' 把面额分成几个部分。 对于前面的部分， 直接不看 '.' 当成普通整数来算就行了。 最后一部分， 因为有小数（两位）和整数（三位）的区别， 所以需要特别判断一下。

最后是输出。整数可以用类似于快输的方法：

```cpp
void print (int x) {
	if (x >= 1000) {
		print (x / 1000);
		putchar ('.');
		printf ("%03d", x % 1000);
	}
	else {
		printf ("%d", x % 1000);
	}
}
```

而小数部分， 由于精度问题， 可能凑不出来， 所以只能乘 $100$ 以后四舍五入了：
```cpp
printf (".%02.0lf", ((x - (int ) (x)) * 100.0));
```
这样， 这道题也就完成了。
代码：

<details>

```cpp
#include <cstdio>
#include <algorithm>
#include <string>
using namespace std;

#define MAXN 100000

string s;

void print (int x) {
	if (x >= 1000) {
		print (x / 1000);
		putchar ('.');
		printf ("%03d", x % 1000);
	}
	else {
		printf ("%d", x % 1000);
	}
}
void print (double x) {
	print ((int) (x));
	if (int (x) != x) {
		printf (".%02.0lf", ((x - (int ) (x)) * 100.0));
	}
}

int main () {
	double sum = 0;
	
	while (1) {
		char c = getchar ();
		
		s.clear();
		while ((c < '0' || c > '9') && c != '.' && c != EOF) {
			c = getchar ();
		}
		if (c == EOF) {
			break;
		}
		while ((c >= '0' && c <= '9') || c == '.') {
			s.push_back(c);
			c = getchar ();
		}
		
		int h = s.rfind('.');
		
		if (h == string::npos) {
			int x = 0;
			
			for (int i = 0; i < s.size(); i ++) {
				if (s[i] != '.') {
					x = x * 10 + (s[i] ^ 48);
				}
			}
			sum += x;
		}
		else {
			if (s.size() - h - 1 == 3){
				int x = 0;
				
				for (int i = 0; i < s.size(); i ++) {
					if (s[i] != '.') {
						x = x * 10 + (s[i] ^ 48);
					}
				}
				sum += x;
			}
			else {
				int x = 0;
				int y = 0;
				
				for (int i = 0; i <= h; i ++) {
					if (s[i] != '.') {
						x = x * 10 + (s[i] ^ 48);
					}
				}
				sum += x;
				for (int i = h + 1; i < s.size(); i ++) {
					if (s[i] != '.') {
						y = y * 10 + (s[i] ^ 48);
					}
				}
				sum += 1.0 * y / 100;
			}
		}
		if (c == EOF) {
			break;
		}
	}
	print (sum);
}
```

</details>
因为这道题目是保证了面额是正确的， 如果不保证， 我们也可以判断一下解决掉：

<details>

```cpp
#include <cstdio>
#include <algorithm>
#include <string>
using namespace std;

#define MAXN 100000

string s;

void print (int x) {
	if (x >= 1000) {
		print (x / 1000);
		putchar ('.');
		printf ("%03d", x % 1000);
	}
	else {
		printf ("%d", x % 1000);
	}
}
void print (double x) {
	print ((int) (x));
	if (int (x) != x) {
		printf (".%02.0lf", ((x - (int ) (x)) * 100.0));
	}
}

int main () {
	double sum = 0;
	
	while (1) {
		char c = getchar ();
		
		s.clear();
		while ((c < '0' || c > '9') && c != '.' && c != EOF) {
			c = getchar ();
		}
		if (c == EOF) {
			break;
		}
		while ((c >= '0' && c <= '9') || c == '.') {
			s.push_back(c);
			c = getchar ();
		}
		
		int h = -1;
		bool bl = 0;
		
		while (s.find ('.', h + 1) != string::npos) {
			int t = s.find ('.', h + 1);
			
			if (t - h - 1 > 3 || (h != -1 && t - h - 1 != 3)) {
				bl = 1;
				
				break;
			}
			h = t;
		}
		if (bl) {
			continue;
		}
		if (h == -1) {
			if (s.size() > 3) {
				continue;
			}
			
			int x = 0;
			
			for (int i = 0; i < s.size(); i ++) {
				if (s[i] != '.') {
					x = x * 10 + (s[i] ^ 48);
				}
			}
			sum += x;
		}
		else {
			if (s.size() - h - 1 == 1 || s.size() - h - 1 > 3) {
				continue;
			}
			else if (s.size() - h - 1 == 3){
				int x = 0;
				
				for (int i = 0; i < s.size(); i ++) {
					if (s[i] != '.') {
						x = x * 10 + (s[i] ^ 48);
					}
				}
				sum += x;
			}
			else {
				int x = 0;
				int y = 0;
				
				for (int i = 0; i <= h; i ++) {
					if (s[i] != '.') {
						x = x * 10 + (s[i] ^ 48);
					}
				}
				sum += x;
				for (int i = h + 1; i < s.size(); i ++) {
					if (s[i] != '.') {
						y = y * 10 + (s[i] ^ 48);
					}
				}
				sum += 1.0 * y / 100;
			}
		}
		if (c == EOF) {
			break;
		}
	}
	print (sum);
}
```

</details>
