# LRU-Cache-gfg
Design a data structure that works like a LRU Cache. Here cap denotes the capacity of the cache and Q denotes the number of queries. Query can be of two types:

PUT x y: sets the value of the key x with value y
GET x: gets the key of x if present else returns -1.
The LRUCache class has two methods get() and put() which are defined as follows.

get(key): returns the value of the key if it already exists in the cache otherwise returns -1.
put(key, value): if the key is already present, update its value. If not present, add the key-value pair to the cache. If the cache reaches its capacity it should remove the least recently used item before inserting the new item.
In the constructor of the class the capacity of the cache should be initialized.
Examples:

Input: cap = 2, Q = 2, Queries = [["PUT", 1, 2], ["GET", 1]]
Output: [2]
Explanation: Cache Size = 2
["PUT", 1, 2] will insert the key-value pair (1, 2) in the cache,
["GET", 1] will print the value corresponding to Key 1, ie 2.
Input: cap = 2, Q = 8, Queries = [["PUT", 1, 2], ["PUT", 2, 3], ["PUT", 1, 5], ["PUT", 4, 5], ["PUT", 6, 7], ["GET", 4], ["PUT", 1, 2], ["GET", 3]]
Output: [5, -1]
Explanation: Cache Size = 2
["PUT", 1, 2] will insert the pair (1,2) in the cache.
["PUT", 2, 3] will insert the pair (2,3) in the cache: 1->2, 2->3(the most recently used one is kept at the rightmost position) 
["PUT", 1, 5] will replace the value of 1 from 2 to 5 : 2 -> 3, 1 -> 5
["PUT", 4, 5] : 1 -> 5, 4 -> 5 (Cache size is 2, hence we delete the least recently used key-value pair)
["PUT", 6, 7] : 4 -> 5, 6 -> 7 
["GET", 4] : Prints 5 (The cache now looks like 6 -> 7, 4->5)
["PUT", 1, 2] : 4 -> 5, 1 -> 2  (Cache size is 2, hence we delete the least recently used key-value pair)
["GET", 3] : No key value pair having key = 3. Hence, -1 is printed.
# code
class Node{
    int data;
    Node prev,next;
    Node(int x){
        data=x;
        prev=null;
        next=null;
    }
}
class LRUCache {
    // Constructor for initializing the cache capacity with the given value.
   static HashMap<Integer,Node> map;
   static HashMap<Node,Integer> map2;
   static Node head,tail;
    static int size,capacity;
    LRUCache(int cap) {
        // code here
        map=new HashMap<>();
        map2=new HashMap<>();
        head=null;
        tail=null;
        size=0;
        capacity=cap;
    }

    // Function to return value corresponding to the key.
    public static int get(int key) {
        // your code here
        if(map.get(key)==null) return -1;
           if(head==null || head.next==null) return head.data;
         Node temp=map.get(key);
      
         if(temp.prev==null) {
             head=head.next;
             head.prev=null;
             
         }
         else if(temp.next==null){
             tail=temp.prev;
             tail.next=null;
         }
         else{
             Node prev=temp.prev;
             prev.next=temp.next;
             prev.next.prev=prev;
         }
         tail.next=temp;
         temp.next=null;
         temp.prev=tail;
         tail=temp;
         return temp.data;
            
    }

    // Function for storing key-value pair.
    public static void put(int key, int value) {
        // your code here
        if(map.get(key)==null) size++;
        if(size>capacity){
            size--;
             map.remove(map2.get(head));
             map2.remove(head);
            head=head.next;
          if(head!=null)  head.prev=null;
          if(head==null) tail=head;
           
        }
        
        if(map.get(key)!=null){
             Node temp=map.get(key);
               temp.data=value;
            if(head.next==null) {
                return;
            }
              
         if(temp.prev==null) {
             head=head.next;
             head.prev=null;
             
         }
         else if(temp.next==null){
             tail=temp.prev;
             tail.next=null;
         }
         else{
             Node prev=temp.prev;
             prev.next=temp.next;
             prev.next.prev=prev;
         }
         tail.next=temp;
         temp.next=null;
         temp.prev=tail;
         tail=temp;
        }
        else{
            if(head!=null){
            Node newnode=new Node(value);
            tail.next=newnode;
            newnode.prev=tail;
            tail=newnode;
                map.put(key,tail);
                map2.put(tail,key);
            }
            else{
                 Node newnode=new Node(value);
                 head=newnode;
                 tail=newnode;
                 map.put(key,head);
                 map2.put(head,key);
            }
            
        }
        // System.out.println(map);
    }
}
