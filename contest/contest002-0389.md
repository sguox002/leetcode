## contest 2
### 389. find the difference (**)
find the extra char, using hashmap

### 390. elimination game (****)
dir change, step change
do not have to do the actual elimination.
```cpp
    int lastRemaining(int n) {
        //
        int head=1,dir=1,rem=n,step=1;
        while(rem>1){
            if(dir || rem%2) head+=step;
            dir^=1;
            rem/=2;
            step*=2;
        }
        return head;
    }
```

### 391. perfect rectangle (****)
the outside 4 points shall appear only one time, inside shall appear even times
total area shall equal to sum of all rectangles
```cpp
    bool isRectangleCover(vector<vector<int>>& rectangles) {
        //get the 4 points outside and the total area
        //inner side points shall mod 2, outside point shall =1
        int total=0;
        int x0=INT_MAX,y0=INT_MAX,x1=INT_MIN,y1=INT_MIN;
        unordered_map<string,int> mp;
        for(auto r: rectangles){
            x0=min(x0,r[0]);
            y0=min(y0,r[1]);
            x1=max(x1,r[2]);
            y1=max(y1,r[3]);
            mp[forms(r[0],r[1])]++;
            mp[forms(r[0],r[3])]++;
            mp[forms(r[2],r[1])]++;
            mp[forms(r[2],r[3])]++;
            total+=(r[0]-r[2])*(r[1]-r[3]);
        }
        if(total!=(x0-x1)*(y0-y1)) return 0;
        if(mp[forms(x0,y0)]!=1 || mp[forms(x0,y1)]!=1 ||
          mp[forms(x1,y0)]!=1 || mp[forms(x1,y1)]!=1) return 0;
        //all other shall be even
        mp.erase(forms(x0,y0));
        mp.erase(forms(x0,y1));
        mp.erase(forms(x1,y0));
        mp.erase(forms(x1,y1));
        for(auto t: mp) if(t.second%2) return 0;
        return 1;
    }
    string forms(int x,int y){
        return to_string(x)+","+to_string(y);
    }
```
