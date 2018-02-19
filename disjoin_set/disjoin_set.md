######  并查集的实现
    其实就是用了一个数组；
```
int father[N];
```
    其中father[i]表示元素i的父亲结点，而父亲结点本身也是这个集合内的元素，另外如果father[i]==i则说明元素i是集合的根结点，但是对于同一个集合来说只有一个根结点
######  并查集的基本操作
1.初始化
```
    for (int i = 0; i <= N; i++)
    {
        father[i] = i;//令father[i]为-1也可，此处以father[i]=i为例
    }
```
2.查找
```
//findFather函数返回元素x所在集合的根结点
//递推查找
int findFather(int x)
{
    while (x != father[x])//如果不是根结点，继续循环
    {
        x = father[x];//获得自己的父亲结点
    }
    return x;
}
//递归查找
int findFather(int x)
{
    if (x == father[x])
        return x;//如果找到根结点，则返回根结点编号x
    else
        return findFather(father[x]);//否则，递归判断x的父亲结点是否是根结点
}
```
3.合并
```
void merge(int a, int b)
{
    int fa = findFather(a);//查找a的根结点，记为fa
    int fb = findFather(b);//查找b的根结点，记为fb
    if (fa != fb)//如果不属于同一个集合
        father[fa] = fb;//合并他们
}
`并查集产生的每一个集合都是一棵树`
```
###### 路径压缩
```
//递推
int findFather(int x)
{
    //由于x在下面的while中会变成根结点，因此先把原先的x保存一下
    int a = x;
    while (x != father[x])//寻找根结点
    {
        x = father[x];
    }
    //到这里，x存放的是根结点,下面把路径上的所有节点的father都改成根结点
    while (a != father[a])
    {
        int z = a;//因为a要被father[a]覆盖，所以先保存a的值，以修改father[a]
        a = father[a];//a回溯父亲结点
        father[z] = x;//将原先结点a的父亲改为根结点
    }
    return x;//返回根结点
}
//递归
int findFather(int v)
{
    if (v == father[v])
        return v;
    else
    {
        int f = findFather(father[v]);
        father[v] = f;
        return f;
    }
}
```
