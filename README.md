# Trol-s-programming-Practice
A new programming learner who is practicing
#include<bits/stdc++.h>
using namespace std;
typedef struct ArcNode {
	int v;//弧所指向的顶点
	int l;//弧长度
	struct ArcNode* nextarc;//下一条弧
};
typedef struct VNode {
	int v;//顶点
	ArcNode* firstarc;//这个顶点出发第一条弧
} Adjlist[100];
int main() {
	int n;//顶点数
	Adjlist A;
	cin >> n;
	ArcNode* P=nullptr;//表示是第几个点
	ArcNode* q = nullptr;
	int flag = 1;//用于表示输入的是顶点的编号(奇数)或权值(偶数)
	char ch;//空格表示没换行
	for (int i = 0; i < n; i++) {
		cin >> A[i].v;
		A[i].firstarc = (ArcNode*)malloc(sizeof(ArcNode));
		P = A[i].firstarc;
		while (cin.get(ch) && ch != '\n') {
			if (flag % 2 == 1) {
				cin >> P->v;
			}
			else {
				cin >> P->l;
				P->nextarc = (ArcNode*)malloc(sizeof(ArcNode));//先内存分配然后再指向下一个
				q = P;
				P = P->nextarc;
			}
			flag++;
		}
		q->nextarc = nullptr;
		free(P);
	}
	flag = 1;
	int dist[100], path[100];
	bool final[100] ;
	fill(final, final + n, false);
	fill(dist, dist + n, INT_MAX);
	fill(path, path + n, -1);
	final[0] = true;//假设以v0为起点，对v0初始化
	dist[0] = 0;
	ArcNode* p = A[0].firstarc;
	while (p) {
		dist[p->v] = p->l;
		path[p->v] = 0;
		p = p->nextarc;
	}
	int lowdist = INT_MAX;//第i步所求得的最短路径
	int low = 0;//第i步所求得的最短路径下标
	while (1) {
		flag = 0;
		lowdist = INT_MAX;
		for (int i = 1; i < n; i++) {//第x次找最短路径
			if (!final[i]) {
				if (dist[i] <= lowdist) {
					lowdist = dist[i];
					low = i;
				}
				flag = 1;
			}
		}
		if (!flag) { break; }
		final[low] = true;
		p = A[low].firstarc;
		while (p) {//允许以A[low]为中转点
			if (p->l + lowdist < dist[p->v]) {
				dist[p->v] = p->l + lowdist;
				path[p->v] = low;
			}
			p = p->nextarc;
		}
	}
	for (int i = 0; i < n; i++) {
		cout << dist[i] << endl;
	}
	return 0;
}
