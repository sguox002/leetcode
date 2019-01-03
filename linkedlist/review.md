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








