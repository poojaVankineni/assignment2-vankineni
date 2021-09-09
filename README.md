# Pooja Vankineni
#### My favorite place is Switzerland

Switzerland is a country of **Chocolates**, **watches** and **banks**.<br>
It has about 7000 lakes and 208 Mountains. The main reason<br> of why I like Switzerland is that
they make high-quality chocolates.

---

## Directions from Maryville to Zurich
1. Maryville to Kansas International Airport (MCI).
    1. Travel from Maryville to MCI by Car.
2. Take flight from MCI to Zurich Airport.
    1. MCI Airport to Newark Liberty International Airport.
    2. Connecting Flight - Newark Liberty International Airport to Zurich Airport.<br>
### List of Items that should be carried to Zurich for maximum enjoyment
* SunGlasses
* Camera
* Jacket
* AirDopes
[Link to AboutMe](./AboutMe.md)

---

## My Favorite Food and Drinks

The below table gives information about 4 Food Items and Drinks that I would suggest everyone to try. The approximate price of the Food Items or Drinks is also mentioned in the table.
| Items | Available at | Price |
|:------:|:----------:|:------:|
| CheezeBurster Pizza | PizzaHut | $10 |
| Chocolate Sizzlers | StarBucks | $5 |
| Cold Coffee | StarBucks | $5 |
| Chicken Wings | KFC | $5 |
---
## Pithy Quotes  
> Don't cry because it's over. Smile because it happened. — *Dr. Seuss*.   

> Everything is hard before it is easy. —*Johann Wolfgang von Goethe*
----
## Quick info about Spanning trees
>The LCA of v and w in T is the shared ancestor of v and w that is located farthest from the root. Computation of lowest common ancestors may be useful, for instance, as part of a procedure for determining the distance between pairs of nodes in a tree: the distance from v to w can be computed as the distance from the root to v, plus the distance from the root to w, minus twice the distance from the root to their lowest common ancestor. In ontologies, the lowest common ancestor is also known as the least common ancestor.

<br>[Quick-link for the source](https://en.wikipedia.org/wiki/Lowest_common_ancestor)

## Sample code for Lowest Common Ancestor - O(N−−√) and O(logN) with O(N) preprocessing
```
struct LCA {
    vector<int> height, euler, first, segtree;
    vector<bool> visited;
    int n;

    LCA(vector<vector<int>> &adj, int root = 0) {
        n = adj.size();
        height.resize(n);
        first.resize(n);
        euler.reserve(n * 2);
        visited.assign(n, false);
        dfs(adj, root);
        int m = euler.size();
        segtree.resize(m * 4);
        build(1, 0, m - 1);
    }

    void dfs(vector<vector<int>> &adj, int node, int h = 0) {
        visited[node] = true;
        height[node] = h;
        first[node] = euler.size();
        euler.push_back(node);
        for (auto to : adj[node]) {
            if (!visited[to]) {
                dfs(adj, to, h + 1);
                euler.push_back(node);
            }
        }
    }

    void build(int node, int b, int e) {
        if (b == e) {
            segtree[node] = euler[b];
        } else {
            int mid = (b + e) / 2;
            build(node << 1, b, mid);
            build(node << 1 | 1, mid + 1, e);
            int l = segtree[node << 1], r = segtree[node << 1 | 1];
            segtree[node] = (height[l] < height[r]) ? l : r;
        }
    }

    int query(int node, int b, int e, int L, int R) {
        if (b > R || e < L)
            return -1;
        if (b >= L && e <= R)
            return segtree[node];
        int mid = (b + e) >> 1;

        int left = query(node << 1, b, mid, L, R);
        int right = query(node << 1 | 1, mid + 1, e, L, R);
        if (left == -1) return right;
        if (right == -1) return left;
        return height[left] < height[right] ? left : right;
    }

    int lca(int u, int v) {
        int left = first[u], right = first[v];
        if (left > right)
            swap(left, right);
        return query(1, 0, euler.size() - 1, left, right);
    }
};

```
<br> [Quick Link for the code source](https://cp-algorithms.com/graph/lca.html)