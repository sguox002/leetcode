876. Middle of the Linked List
traverse twice

206. Reverse Linked List
curr, prev, next to reverse the list
```cpp
    ListNode* reverseList(ListNode* head) {
        ListNode *curr,*prev;
        curr=head;
        prev=0;
        while(curr) //current node is not null
        {
            ListNode *next=curr->next;
            curr->next=prev; //reverse it
            prev=curr;
            curr=next;
        }
        return prev;
    }
```

237. Delete Node in a Linked List
replace current node with its next

21. Merge Two Sorted Lists
just trick to manage the next pointer

83. Remove Duplicates from Sorted List
remove a node
using delete must generated using new

203. Remove Linked List Elements
remove the head first if necessary and keep the new head
note for freeing the nodes

141. Linked List Cycle
classical. If there is a cycle, using a fast and a slow pointer to traverse the list and they will meet.

234. Palindrome Linked List
O(1) space and O(n) complexity
using a fast and slow pointer to get the mid, and then reverse the second half
then use two pointer to traverse each half list.

160. Intersection of Two Linked Lists
pointer 1 traverse A and then B
pointer 2 traverse B and then A
the two pointers meet at the intersection

707. Design Linked List
support function
get
addHead
AddTail
delHead
delTail
addIndex
DelIndex

we can keep the head and tail and size for convenience

817. Linked List Components
G is a subset array of linked list L, if connected in L then it is considered as a group.
Return the number of groups in G
the order in G does not matter. so we use hash table.
```cpp
    int numComponents(ListNode* head, vector<int>& G) {
        unordered_set<int> myset(G.begin(),G.end());
        int grp=0,cnt=0;
        while(head)
        {
            if(myset.count(head->val)) cnt++;
            else {if(cnt) {grp++;cnt=0;}}
            head=head->next;
        }
        if(cnt) grp++;
        return grp;
    }
```

445. Add Two Numbers II
reverse the both lists so they are aligned on the LSB.
```cpp
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ListNode *r1,*r2;
        r1=reverseList(l1);
        r2=reverseList(l2);
        //print(r1);
        //print(r2);
        ListNode *newh=0;
        int i=0,j=0;
        int cr=0;//carrier
        ListNode *p1=r1,*p2=r2,*p=newh,*prev=0;
        while(p1 || p2)
        {
            int t=(p1?p1->val:0)+(p2?p2->val:0)+cr;
            cr=t/10;
            t-=cr*10;
            if(!p) p=newh=new ListNode(t);
            else p=new ListNode(t);
            if(prev) prev->next=p;
            prev=p;
            if(p1) {p1=p1->next;}
            if(p2) {p2=p2->next;}
        }
        if(cr) {p=new ListNode(cr);prev->next=p;}
        
        newh=reverseList(newh);
        return newh;
    }
    ListNode* reverseList(ListNode* root)
    {
        ListNode* p=root,*prev=0,*next;
        while(p)
        {
            next=p->next; //save its next
            p->next=prev; //reverse its next
            prev=p;//current node becomes prev
            p=next; //go to next node
        }
        return prev;//p will become empty
    }
```

725. Split Linked List in Parts
given k, split the array into k parts with consecuative parts difference <=1 and previous part >= later parts
get the number of nodes and divide by k and the mod assigned to first several parts
using a 2d loop 

328. Odd Even Linked List
use two pointer to alternate, odd even odd even, odd's next is even, even's next is odd
```cpp
    ListNode* oddEvenList(ListNode* head) {
        if(!head) return 0;
        ListNode* even_head=head->next; //saved for the first even node
        ListNode* odd_node=head;
        ListNode* even_node=even_head;
        //involves two nodes, odd, even and separate them into two lists
        while(odd_node->next && even_node->next) //current node and next node is not empty
        {
            //ListNode* temp=odd_node->next;
            odd_node->next=even_node->next;
            odd_node=odd_node->next;
            if(odd_node)
            {
                even_node->next=odd_node->next; //in current loop it is not able to determine event_node's next
                even_node=even_node->next;
            }
            
        }
        //connect two lists
        odd_node->next=even_head;
        return head;
        
    }
```

24. Swap Nodes in Pairs
const space, must swap nodes instead of values
```cpp
    ListNode* swapPairs(ListNode* head) {
        ListNode *ptr=head;
        ListNode *curr=head,*next,*nextnext,*prev=0;
        if(head && head->next) head=head->next;
        while(ptr && ptr->next)
        {
            //swap ptr and ptr->next
            curr=ptr;
            next=ptr->next;
            nextnext=ptr->next->next;

            curr->next->next=curr;
			curr->next=nextnext;
			curr=next; //note the second time pair switch is incorrect!!! the previous node's next shall be modified
            if(prev) prev->next=curr;
            
            //advance to next next node
			prev=curr->next;
            ptr=nextnext; //this invalidates above assignment!
        }
        return head;
    }
```

109. Convert Sorted List to Binary Search Tree
balance tree
traverse and store the value and recursively build the tree (divide by half)
```cpp
    TreeNode* sortedListToBST(ListNode* head) {
        vector<int> nums;
        while(head){nums.push_back(head->val);head=head->next;}
        int n=nums.size();
        if(!n) return 0;
        int left=0,right=n-1;
        int mid=(left+right+1)/2;
        //each root shall be the mid value of the current section
        TreeNode* root=new TreeNode(nums[mid]);
        insert(root,nums,0,mid-1);
        insert(root,nums,mid+1,right);
        return root;
    }
    void insert(TreeNode* root,vector<int>& nums,int l,int r)
    {
        if(r<l) return;//exit condition
        int mid=(l+r+1)/2;
        int val=nums[mid];
        TreeNode *node=new TreeNode(val);
        if(val<=root->val) 
        {
            root->left=node;
            insert(root->left,nums,l,mid-1);
            insert(root->left,nums,mid+1,r);
        }
        else 
        {
            root->right=node;
            insert(root->right,nums,l,mid-1);
            insert(root->right,nums,mid+1,r);
            
        }
    }
```

430. Flatten a Multilevel Doubly Linked List
if it has a child, flatten it
```cpp
    Node* flatten(Node* head)
    {
        Node* last=0;
        return helper(head,last);
    }
    Node* helper(Node* head,Node*& last) {
        if(!head) return 0;
        Node* node=head;
        while(node)
        {
            //cout<<node->val<<endl;
            if(node->child)
            {
                Node* next=node->next;//save the next
                node->next=helper(node->child,last);
                node->next->prev=node;
                node->child=0;
                last->next=next;
                if(next) next->prev=last;
                node=last;
            }
            if(!node->next) last=node;
            node=node->next;
            
        }
        return head;
    }
```
we get the next and last pointer of the child and then connect

147. Insertion Sort List
insertion sort using const space. cannot use multiset
Keep a sorted partial list (head) and start from the second node (head -> next), each time when we see a node with val smaller than its previous node, we scan from the head and find the position that the node should be inserted. Since a node may be inserted before head, we create a dummy head that points to head.

86. Partition List
Given a linked list and a value x, partition it such that all nodes less than x come before nodes greater than or equal to x.
You should preserve the original relative order of the nodes in each of the two partitions.
use one list to get the smaller than x and one list to get the >=x and then connect the two lists

19. Remove Nth Node From End of List
using two pointers, one pointer advance n first and then other pointer advance the same time, when the first pointer reaches the end, the other pointer reaches the nth node from the end

92. Reverse Linked List II
Reverse a linked list from position m to n. Do it in one-pass.
Note: 1 ≤ m ≤ n ≤ length of list.
traverse to mth node, and then reverse n and connect the first and last.

```cpp
    ListNode* reverseBetween(ListNode* head, int m, int n) {
        //first traverse and then reverse the one and then connect the reversed 
        int cnt=1;
        ListNode *curr=head,*prev=0,*next=0;
        while(cnt<m) {prev=curr,curr=curr->next;cnt++;}
        ListNode* nhead=reversen(curr,n-m+1,next);
        if(prev) prev->next=nhead;
        curr->next=next;
        return prev?head:nhead;
    }
    //need return the new head and the next node
    ListNode* reversen(ListNode* head,int n,ListNode*& next)
    {
        int cnt=0;
        ListNode *curr,*prev;
        prev=0;curr=head;
        while(cnt<n)
        {
            //cout<<curr->val<<endl;
            next=curr->next;
            curr->next=prev;
            prev=curr;
            curr=next;
            cnt++;
            
        }
        return prev;
    }
```    

148. Sort List
Sort a linked list in O(n log n) time using constant space complexity
split in half and solve the two smaller subproblem and then merge sort.
```cpp
    ListNode* sortList(ListNode* head) {
        //split list into a single node and then use merge sort
        //stop condition!
        if(!head || !head->next) return head;
        int len=0;
        ListNode* p=head;
        while(p) {len++;p=p->next;}
        ListNode* right=split(head,len/2);
        head=sortList(head);
        right=sortList(right);
        head=mergeSort(head,right);
        return head;
    }
    ListNode* split(ListNode* head,int split_len)
    {
        if(!split_len) return head;
        ListNode* p=head;
        int cnt=0;
        while(cnt<split_len-1) {p=p->next;cnt++;}
        ListNode* newhead=p->next;
        p->next=0;
        return newhead;
    }
    ListNode* mergeSort(ListNode* l1,ListNode* l2)
    {
        ListNode *p1=l1,*p2=l2,*newhead=0,*prev=0;
        while(p1 && p2)
        {
            if(p1->val<p2->val) 
            {
                if(!newhead) newhead=p1;
                if(prev) prev->next=p1;
                prev=p1;
                p1=p1->next;
            }
            else 
            {
                if(!newhead) newhead=p2;
                if(prev) prev->next=p2;
                prev=p2;
                p2=p2->next;
            }
        }
        if(p1) {if(prev) prev->next=p1;}
        else {if(prev) prev->next=p2;}
        return newhead;
    }
```

82. Remove Duplicates from Sorted List II
Given a sorted linked list, delete all nodes that have duplicate numbers, leaving only distinct numbers from the original list.
since the head may be removed, we add a dummy node to point to the head.
when the node val is the same we skip them
```cpp
    ListNode* deleteDuplicates(ListNode* head) {
        if(!head) return 0;
        ListNode *pre,*post;
        ListNode* dummy=new ListNode(INT_MIN);
        dummy->next=head;
        pre=dummy;
        post=head->next;
        ListNode *p=head;
        while(p)
        {
            while(post && post->val==p->val) post=post->next;
            if(post!=p->next) //contain more than one node
            {
                //delete all nodes between pre and post
                pre->next=post;
            }
            else  pre=p;//new pre updated only when no deletion
            p=post;//new p
            if(post) post=post->next;//new post
        }
        return dummy->next;
    }
```

142. Linked List Cycle II
Given a linked list, return the node where the cycle begins. If there is no cycle, return null.

To represent a cycle in the given linked list, we use an integer pos which represents the position (0-indexed) in the linked list where tail connects to. If pos is -1, then there is no cycle in the linked list.

Note: Do not modify the linked list.
use a slow and a fast pointer to detect if there is a cycle
if there is a cycle, starting another pointer at the head and it will meet the slow pointer at the entry.
It is proved using math:
2t=L+P+mC, for fast
t=L+P+nC, for slow
then t=(m-n)C=L+P+nC so we get L+P=(m-2n)C, so the two will meet at L.
```cpp
ListNode *detectCycle(ListNode *head) {
    if (head == NULL || head->next == NULL)
        return NULL;
    
    ListNode *slow  = head;
    ListNode *fast  = head;
    ListNode *entry = head;
    
    while (fast->next && fast->next->next) {
        slow = slow->next;
        fast = fast->next->next;
        if (slow == fast) {                      // there is a cycle
            while(slow != entry) {               // found the entry location
                slow  = slow->next;
                entry = entry->next;
            }
            return entry;
        }
    }
    return NULL;                                 // there has no cycle
}
```

2. Add Two Numbers
number stored in reverse order in list.

143. Reorder List
split the list in half, and reverse the 2nd half and then merge one by one
```cpp
    void reorderList(ListNode* head) {
        //divide the list by half, reverse the second one and merge
        int len=0;
        ListNode* p=head;
        while(p) {len++;p=p->next;}
        ListNode* right=split(head,len/2);
        right=reverseList(right);
        //merge two lists in-place
        p=0;
        ListNode *p1=head,*p2=right;
        ListNode *next1,*next2;
        while(p1&&p2) 
        {
            next1=p1->next; //save its next node
            next2=p2->next; //save its next node
            p=p1;//p1 is now changed the next pointer
            p->next=p2;//
            p->next->next=next1; //this is very important to point to right nodes
            p1=next1;
            p2=next2;
            p=p->next;
        }
        if(p1) p->next=p1;
        if(p2) p->next=p2;
        return;
    }
    ListNode* split(ListNode* head,int split_len)
    {
        if(!split_len) return head;
        ListNode* p=head;
        int cnt=0;
        while(cnt<split_len-1) {p=p->next;cnt++;}
        ListNode* newhead=p->next;
        p->next=0;
        return newhead;
    }

   ListNode* reverseList(ListNode* root)
    {
        ListNode* p=root,*prev=0,*next;
        while(p)
        {
            next=p->next; //save its next
            p->next=prev; //reverse its next
            prev=p;//current node becomes prev
            p=next; //go to next node
        }
        return prev;//p will become empty
    }
```

61. Rotate List
Given a linked list, rotate the list to the right by k places, where k is non-negative.
get the list length, and k%len and then find the new head and connect the 2nd half with the first half.

138. Copy List with Random Pointer
need hashmap to map the pointer to index

25. Reverse Nodes in k-Group
k<2 no change
head will be changed, add a dummy node
reverse in place of n nodes (it needs get the first and the last)
pre, curr, next.

23. Merge k Sorted Lists
using priority_queue or multiset to merge the lists.
```cpp
    ListNode* mergeKLists(vector<ListNode*>& lists) {
       //use a min heap to do the sorting
        //a heap can be built using set or make_heap
        multiset<ListNode*,cmp1> myset;//(lists.begin(),lists.end());
        for(auto it=lists.begin();it!=lists.end();it++)
        {
            if(*it) myset.insert(*it);
        }
        
        //print(myset);
        ListNode* newhead=0,*prev=0,*p;
        while(!myset.empty())
        {
            if(!newhead) newhead=*myset.begin();
            p=*myset.begin();
            if(prev) prev->next=p;
            prev=p;
            myset.erase(myset.begin());
            if(p&&p->next) myset.insert(p->next);
        }
        return newhead;
    }
```
it first stores the starting elements into pq or multiset, then pop the smallest and advance its next and push into it.






