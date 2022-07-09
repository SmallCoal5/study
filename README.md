### 前缀树
```C++
class Trie {
public:
    bool isWord;
    vector<Trie*> children;
    Trie () : isWord(false), children(26, nullptr) {}

    void insert(const string& str) {
        Trie* node = this;
        for (auto& ch : str) {
            if (node->children[ch - 'a'] == nullptr) {
                node->children[ch - 'a'] = new Trie();
            }
            node = node->children[ch - 'a'];
        }
        node->isWord = true;
    }
};

```

### 线段树
``` C++
class Node {
public:
    int l,r,mid,v,add;
    Node* left;
    Node* right;
    Node(int l, int r){
        this->l = l;
        this->r = r;
        this->mid = (l+r)>>1;
        this->left = this->right = nullptr;
        v = add = 0;
    }

};

class STree {
public:
    Node* root;
    STree(){
        this->root = new Node(0,1e9);
    }
    void update(int l,int r, int v, Node* node){
        if(l>r) return;
        if(node->l>=l&&node->r<=r) {
            node->v = v;
            node->add = v;
            return;
        }
        pushdown(node);
        if(l<=node->mid) update(l,r,v,node->left);
        if(r>node->mid) update(l,r,v,node->right);
        pushup(node);
    }
    void update(int l,int r, int v){
        return update( l, r, v, root);
    }
    int query(int l,int r) {
        return query(l,r,root);
    }
    int query(int l,int r,Node* node){
        if(l>r) return 0;
        if(node->l>=l&&node->r<=r) return node->v;
        pushdown(node);
        int res = 0;
        if(l<=node->mid) res = max(query(l,r,node->left),res);
        if(r>node->mid) res = max(res,query(l,r,node->right));
        return res;
    }
    void pushdown(Node* node){
        if(!node->left) node->left = new Node(node->l,node->mid);
        if(!node->right) node->right = new Node(node->mid+1,node->r);
        if(node->add!=0){
            int v = node->add;
            node->left->v = node->right->v = node->left->add = node->right->add = v;
            node->add = 0;
        }
    }
    void pushup (Node* node){
        node->v = max(node->left->v,node->right->v);
    }
};
```

### 树状数组
``` C++
    int lowbit(int x) { return x&(-x);}

    void update(int i,int val,vector<int>&b){
        for(int j=i;j<b.size();j+=lowbit(j)){
            b[j]+=val;
        }
    }
    int sums(int i,vector<int>&b){
        int res = 0;
        for(int j=i;j>0;j-=lowbit(j))
        res += b[j];
        return res;

    }
```
### 快速选择

``` C++
    int quickSelect(int start, int end, vector<int>& nums, int n){
        if(start>end) return 0;
        int i = start;
        int j = start;
        int k = end;
        int temp = nums[rand()%(end-start+1)+start];
        while(i<=k){
            if(nums[i]<temp) swap(nums[i++],nums[j++]);
            else if(nums[i]>temp) swap(nums[i],nums[k--]);
            else i++;
        }
        if (j==n) return nums[j];  //j指向第一个等于temp的数
        else if(j<n) return quickSelect(j+1,end,nums,n);
        else return quickSelect(start,j-1,nums,n);
    }

```

### 桶ID

``` C++
    //桶ID
    int getID(int x, long t) {
        if(x < 0) 
            return (x + 1ll) / t ;
        else 
            return x / t + 1ll;
    }

```

### 有序集合

``` C++
    // 返回比x大的最近一个数，如果相等返回本身。
    int n = nums.size();
    set<int> rec;
    for (int i = 0; i < n; i++) {
        auto iter = rec.lower_bound(nums[i] - t);
        if (iter != rec.end() && *iter <= nums[i] + t) {
            return true;
        }
        rec.insert(nums[i]);
        if (i >= k) {
            rec.erase(nums[i - k]);
        }
    }
    return false;
    
```

### stringstream分割字符串

``` C++

	string s = "This,is,a,test";
	stringstream ss;
	ss << s;
	vector<string> vec;
	string temp;
	while (getline(aa,temp,','))
	{
		vec.push_back(temp);
	}
	for (int i = 0; i < vec.size(); i++)
	{
		cout << vec[i] << endl;
	}
    //清空缓冲区
    ss.str("");
    //重置标识符
    ss.clear();

```

### 组合数dfs

``` C++
    // [1,1,2,3,3]  <- [2,1,2] (balls)
    double cnt = 0;
    for(int k=0;k<=balls[i];k++){
        temp[i]+=k;
        cnt+= (dfs(i+1,temp,balls,n-k))*(C(k,balls[i]));
        temp[i]-=k;
    }
    return cnt;

```