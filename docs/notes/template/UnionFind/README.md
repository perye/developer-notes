# 并查集

## 简介

并查集（Disjoint set，Union-Find）是一种用来管理元素分组情况的数据结构。  
并查集的一种高效实现是：**使用树的根节点「代表」一个集合**，同一棵树上的所有节点属于一个集合。  
只要两个元素的根节点相同，就视为属于同一个集合。使用树的根节点代表一个集合的方法，称之为「代表元」法。

- 初始化（Init）：将每个元素所在集合初始化为其自身。
- 合并（Union）：将两个元素所属的集合合并为一个集合。
- 查找（Find）：查找元素所在的集合，即根节点。

## 应用

- 最小生成树：Kruskal 算法
- 图的连通分量
- 静态连通性
- 动态连通性：判断图的最早连通时间

## 局限

- 不支持拆分（split）操作：任何节点一旦成为其他节点的子节点后，将永远不可能再成为树根。
- 集合必须是不相交的（disjoint）：同一个元素不能属于多个集合。
- 代表元不记录集合成员信息：
    - 父节点不记录子节点的信息。
    - 查找图中某节点所在的连通分量的所有节点需要再次扫描或者维护额外信息。
    
## 模板

```java
class UnionFind{
    int[] parent;// parent[i]表示i这个元素指向的父亲节点
    int[] size;//size[i]表示以i为根节点的集合中元素个数
    int n;//节点的个数，初始化每一个节点都是一个单独的连通分量
    int setCount;//连通分量的数目
    public UnionFind(int n){
        this.size=new int[n];
        this.parent=new int[n];
        this.n=n;
        this.setCount=n;
        Arrays.fill(size,1);
        for(int i=0;i<n;i++){
            parent[i]=i;
        }
    }

    public int find(int x){
        return parent[x]==x?x:find(parent[x]);
    }

    public boolean unit(int x,int y){
        x=find(x);
        y=find(y);
        if(x==y){
            return false;
        }
        if(size[x]<size[y]){
            int tem=x;
            x=y;
            y=tem;
        }

        parent[y]=x;
        size[x]+=size[y];
        --setCount;
        return true;
    }

    public boolean connected(int x, int y) {
        x = find(x);
        y = find(y);
        return x == y;
    }

}
```