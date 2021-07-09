---
title: Alignment of Code
date: 2021-07-08
categories:
- YRL
tags: STL
---

## Problem

[Link](https://www.luogu.com.cn/problem/UVA1593)

题意：输入若干行代码，要求各列单词的左边界对齐且尽量靠左。

一道很基础的字符串处理题目，我们可以用才学的 **string** 进行处理。

## Solution

一整行的读入可以采用 **getline** 读入。

然后以空格为分界将每个单词放入二维数组中。这样可以便于求得每列单词的最大长度，短的单词用空格补齐，以达到每列对齐。

## Code
```cpp
#include <string>
#include <cstdio>
#include <iostream>

using namespace std;

const int Maxn = 1000;
const int Maxm = 180;

string str, word;
string a[Maxn + 5][Maxm + 5];
int len[Maxn + 5], siz[Maxm + 5];

int Max (int a, int b) {
	return a > b ? a : b;
}

int main () {
	int n = 0;
	while (getline (cin, str)) { //无限输入
		n ++;
		for (int i = 0; i < (int) str.size (); i ++) {
			if (str[i] != ' ') word.push_back (str[i]);
			else {
				if ((int) word.size ()) {
					a[n][++ len[n]] = word; //把单词处理出来
//					cout << endl << n << ',' << len[n] << word << endl;
					word.clear ();
				}
			}
		}
		if ((int) word.size ()) {
			a[n][++ len[n]] = word;
			word.clear ();
		}
	}
	
	for (int i = 1; i <= n; i ++) {
		for (int j = 1; j <= len[i]; j ++) {
			siz[j] = Max (siz[j], (int) a[i][j].length () + 1);
		} //求得每列单词的最大长度
	}

	for (int i = 1; i <= n; i ++) {
		for (int j = 1; j <= len[i]; j ++) {
			cout << a[i][j];
			if (j == len[i]) continue;
			for (int k = (int) a[i][j].length () + 1; k <= siz[j]; k ++) printf (" "); //补齐空格
		}
		printf ("\n");
	}
	return 0;
}
```
