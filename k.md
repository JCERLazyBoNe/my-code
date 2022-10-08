# 图论模板
## 0x00 最短路
### 0x01 floyd
```
#include <iostream>
#include <cstdio>
#include <cmath>

using namespace std;

void read(long long &a)
{
	a=0;
	bool k=false;
	char c=getchar();
	while(c<'0'||c>'9')
	{
		if(c=='-')
			k=true;
		c=getchar();
	}
	while(c>='0'&&c<='9')
		a*=10,a+=c-'0',
		c=getchar();
	if(k)a=-a;
}

long long n,m;
long long k[105][105];

int main()
{
	read(n);
	for(int i=1;i<=n;i++)
		for(int l=1;l<=n;l++)
			k[i][l]=1<<30;
	for(int i=1;i<=n;i++)
		k[i][i]=0;
	read(m);
	for(int i=1;i<=m;i++)
	{
		long long k1,k2,k3;
		read(k1);
		read(k2);
		read(k3);
		k[k1][k2]=k3;
		k[k2][k1]=k3;
	}
	for(int l=1;l<=n;l++)
		for(int i=1;i<=n;i++)
			for(int j=1;j<=n;j++)
				k[i][j]=min(k[i][j],k[i][l]+k[j][l]);
	for(int i=1;i<=n;i++){
		for(int j=1;j<=n;j++)
			printf("%d ",k[i][j]);
		printf("\n");
	}	
}
```

### 0x02 dij
```
#include<iostream>
#include <cstdio>
#include<algorithm>
#include<queue>
#include<string.h>
#define maxn 10005
#define inf 0x3f3f3f3f
using namespace std;
struct edge{
	int to;
	int next;
	int w;
	bool operator < (const edge &a) const{
		return a.w<w;
	}
}eg[maxn],Now,Next;
int head[maxn];
int vist[maxn];
int dist[maxn];
int d;
void add(int u,int v,int w){
	
	eg[d].to=v;
	eg[d].w=w;
	eg[d].next=head[u];
	head[u]=d++;
}
int n;
void dijkstra(){
	memset(dist,inf,sizeof(dist));
	memset(vist,0,sizeof(vist));
	priority_queue<edge>q;
	Now.to=1;
	dist[1]=0;
	q.push(Now);
	while(!q.empty()){
		Now=q.top();
		q.pop();
		if(vist[Now.to]) continue;
		vist[Now.to]=1;
		for(int j=head[Now.to];j!=-1;j=eg[j].next){
			int u=eg[j].to;
			if(eg[j].w+dist[Now.to]<dist[u]&&!vist[u]){
				dist[u]=eg[j].w+dist[Now.to];
				Next.to=u;
				Next.w=dist[u];
				q.push(Next);
			} 
		}
	}
	printf("%d\n",dist[n]);
}
int main(){
	int m;
	while(~scanf("%d%d",&n,&m)){
		if(n==0&&m==0)break;
		d=0;
	//	memset(eg,0,sizeof(eg));
		memset(head,-1,sizeof(head));
	for(int i=0;i<m;i++){
		int x,y,w;
		scanf("%d%d%d",&x,&y,&w);
		add(x,y,w);
		add(y,x,w);
	}
	dijkstra();
	}
	return 0;
}
```

### 0x03 bellman ford
```
#include <bits/stdc++.h>
using namespace std;
#define int long long
int n,m,s;
struct edge{
    int u,v,w;
}k[500005];
int dis[10005];

inline int read()
{
    int w=0;
    char c=getchar();
    while(c<'0'||c>'9')
    {
        c=getchar();
    }
    while(c>='0'&&c<='9')
    w=w*10+c-'0',c=getchar();
    return w;
}

void bellman_ford()
{
    memset(dis,0x3f3f3f3f,sizeof(dis));
    dis[s]=0;
    for(int i=1;i<n;i++)
    {
        for(int j=1;j<=m;j++)
        {
            int a=k[j].u;
            int b=k[j].v;
            int c=k[j].w;
            if(dis[a]!=0x3f3f3f3f&&dis[b]>dis[a]+c)
            dis[b]=dis[a]+c;
        }
    }
}
signed main()
{
    cin>>n>>m>>s;
    for(int i=1,u,v,w;i<=m;i++)
    {
        u=read();
        v=read();
        w=read();
        // cin>>u>>v>>w;
        k[i].u=u;
        k[i].v=v;
        k[i].w=w;
    }
    bellman_ford();
    for(int i=1;i<=n;i++)
    cout<<dis[i]<<' ';
    // cout<<((dis[i]==0x3f3f3f3f)?2147483647:dis[i])<<' '; 
    return 0;
}
```

### 0x04 SPFA(死了)
```
#include<bits/stdc++.h>
using namespace std;
#define N 2510
#define M 16300
int w[M],to[M],ne[M],h[N];
int cnt;
int dist[N],vis[N];
int n,m,be,en;
queue<int>q;
void add( int a,int b,int c){
    w[cnt]=c;to[cnt]=b;ne[cnt]=h[a];h[a]=cnt++;
}
void spfa(){
    q.push(be);
    while(!q.empty()){
        int now=q.front();
        vis[now]=0;//出队之后将标记置为0
        q.pop();
        for( int i=h[now];i!=-1;i=ne[i]){
            int j=to[i];
            // dist[j]=min(dist[j],dist[now]+w[i]);
            if(dist[j]>dist[now]+w[i]){
                dist[j]=dist[now]+w[i];
               //j点的最大值得到了更新，如果j不在队中，需要将j重新入队
                if(vis[j]==0){
                    vis[j]=1;
                    q.push(j);
                }
            }
        }
    }
}
int main(){
    cin>>n>>m>>be>>en;
    memset(h,-1,sizeof(h));
    memset(dist,0x3f,sizeof(dist));
    for( int i=1;i<=m;i++){
        int a,b,c;
        cin>>a>>b>>c;
        add(a,b,c);
        add(b,a,c);
    }
    dist[be]=0;
    vis[be]=1;
    spfa();
    cout<<dist[en];
}
```

### 0x05 johnson
```
#include <cstring>
#include <utility>
#include <cstdio>
#include <queue>
using namespace std;
const int N=10000,INF=1e9;

int Head[N],Edge[N],Leng[N],Next[N],tot;
long long H[N],C[N],Dis[N];
bool V[N];
int n,m; 
inline void addedge(int u,int v,int w){ Edge[++tot]=v,Leng[tot]=w,Next[tot]=Head[u],Head[u]=tot; }

inline bool spfa(int s){
	queue<int> q;
	memset(H,0x3f,sizeof(H));
	memset(V,false,sizeof(V));
	H[s]=0,V[s]=true,q.push(s);
	while(!q.empty()){
		int x=q.front(); q.pop(); V[x]=false;
		for(int i=Head[x]; i; i=Next[i]){
			int y=Edge[i],z=Leng[i];
			if(H[y]>H[x]+z){
				H[y]=H[x]+z;
				if(!V[y]){
					V[y]=true,q.push(y),++C[y];
					if(C[y]>n) return false;
				} 
			}
		}
	}
	return true;
}

inline void dijkstra(int s){
	priority_queue<pair<int,int> > q;
	for(int i=1; i<=n; ++i) Dis[i]=INF;
	memset(V,false,sizeof(V));
	Dis[s]=0,q.push(make_pair(0,s));
	while(!q.empty()){
		int x=q.top().second; q.pop();
		if(V[x]) continue;
		V[x]=true;
		for(int i=Head[x]; i; i=Next[i]){
			int y=Edge[i],z=Leng[i];
			if(Dis[y]>Dis[x]+z){
				Dis[y]=Dis[x]+z;
				if(!V[y]) q.push(make_pair(-Dis[y],y));
			}
		}
	}
	return;
}

int main(){
	scanf("%d%d",&n,&m);
	for(int i=1; i<=m; ++i){
		int x,y,z;
		scanf("%d%d%d",&x,&y,&z);
		addedge(x,y,z);
	}
	
	for(int i=1; i<=n; ++i) addedge(0,i,0);
	if(!spfa(0)) return puts("-1")&0;
	
	for(int i=1; i<=n; ++i)
		for(int j=Head[i]; j; j=Next[j])
			Leng[j]=Leng[j]+H[i]-H[Edge[j]];
	for(int i=1; i<=n; ++i){
		dijkstra(i);
		long long ans=0;
		for(int j=1; j<=n; ++j)
			ans+=j*(Dis[j]==INF ? INF : Dis[j]-H[i]+H[j]);
		printf("%lld\n",ans);
	}
	return 0;
}

```