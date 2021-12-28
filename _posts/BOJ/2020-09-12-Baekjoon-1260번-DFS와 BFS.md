---
layout: article
title: Baekjoon 1260번 DFS와 BFS
tags: BOJ
#comments: true
#article_header:
#  type: cover
#  image:
#    src:
aside:
  toc: true
key: page-aside
---

## C++ Code

```cpp
#include <iostream>
#include <queue>
using namespace std;

int map[1001][1001];
int visited[1001];
queue<int> q;
int N, M, V;

void dfs(int v)
{
	cout << v << " ";
	visited[v] = 1;

	for (int i = 1; i <= N; i++)
	{
		if (map[v][i] && !visited[i])
		{
			dfs(i);
		}
	}
}

void bfs(int v)
{
	visited[v] = 1;
	q.push(v);

	while (!q.empty())
	{
		v = q.front();
		q.pop();

		cout << v << " ";
		for (int i = 1; i <= N; i++)
		{
			if (map[v][i] && !visited[i])
			{
				q.push(i);
				visited[i] = 1;
			}
		}
	}
}

int main()
{
	ios_base::sync_with_stdio(false);
	cin.tie(NULL); cout.tie(NULL);

	cin >> N >> M >> V;

	memset(map, 0, sizeof(map));
	memset(visited, 0, sizeof(visited));

	for (int i = 0; i < M; i++)
	{
		int a, b;
		cin >> a >> b;
		map[a][b] = 1;
		map[b][a] = 1;
	}

	dfs(V);
	cout << '\n';

	memset(visited, 0, sizeof(visited));

	bfs(V);
	cout << '\n';
}
```
