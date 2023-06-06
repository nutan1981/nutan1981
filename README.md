#include <iostream>
#include <string>
using namespace std;
class Avl
{
public:
    string word;
    string mean;
    Avl *r, *l;
    int height(Avl *T);
    int BF(Avl *T);
    Avl *create();
    Avl *insert(Avl *T, string a, string b);
    void display(Avl *T);
    Avl *RR(Avl *T);
    Avl *LL(Avl *T);
    Avl *RL(Avl *T);
    Avl *LR(Avl *T);
    Avl *rotater(Avl *x);
    Avl *rotatel(Avl *x);
    Avl *del(Avl *T, string x);
    Avl *findmin(Avl *T);
    void search(Avl *T, string x);
    Avl *update(Avl *T, string x);
};

int Avl::height(Avl *T)
{
    int hl, hr;
    if (T == NULL)
    {
        return 0;
    }
    hl = height(T->l);
    hr = height(T->r);
    if (hl > hr)
    {
        return (hl + 1);
    }
    return (hr + 1);
}
int Avl::BF(Avl *T)
{
    int hl, hr;
    hl = height(T->l);
    hr = height(T->r);
    return (hl - hr);
}
Avl *Avl::create()
{
    Avl *root;
    int n;
    string a, b;
    cout << "Enter the number of nodes" << endl;
    cin >> n;
    root = NULL;
    for (int i = 0; i < n; i++)
    {
        cout << "Enter the word" << endl;
        cin >> a;
        cout << "Enter the meaning" << endl;
        cin >> b;
        root = insert(root, a, b);
    }
    return (root);
}

Avl *Avl::insert(Avl *T, string a, string b)
{
    if (T == NULL)
    {
        T = new Avl;
        T->word = a;
        T->mean = b;
        T->l = NULL;
        T->r = NULL;
        return (T);
    }
    if (a > T->word)
    {
        T->r = insert(T->r, a, b);
        if (BF(T) < -1)
        {
            if (a > T->r->word)
            {
                T = RR(T);
            }
            else
            {
                T = RL(T);
            }
        }
        return (T);
    }
    else if (a < T->word)
    {
        T->l = insert(T->l, a, b);
        if (BF(T) > 1)
        {
            if (a < T->l->word)
            {
                T = LL(T);
            }
            else
            {
                T = LR(T);
            }
        }
        return (T);
    }
    return (T);
}

Avl *Avl::LL(Avl *T)
{
    T = rotater(T);
    return (T);
}
Avl *Avl::RR(Avl *T)
{
    T = rotatel(T);
    return (T);
}
Avl *Avl::LR(Avl *T)
{
    T->l = rotatel(T->l);
    T = rotater(T);
    return (T);
}
Avl *Avl::RL(Avl *T)
{
    T->r = rotater(T->r);
    T = rotatel(T);
    return (T);
}
Avl *Avl::rotater(Avl *x)
{
    Avl *y;
    y = x->l;
    x->l = y->r;
    y->r = x;
    return (y);
}
Avl *Avl::rotatel(Avl *x)
{
    Avl *y;
    y = x->r;
    x->r = y->l;
    y->l = x;
    return (y);
}
void Avl::display(Avl *T)
{
    if (T == NULL)
    {
        return;
    }
    display(T->l);
    cout << "The word is " << T->word << " and its meaning is " << T->mean << endl;
    display(T->r);
}
Avl *Avl::findmin(Avl *T)
{
    Avl *p;
    p = T;
    while (p->l != NULL)
    {
        p = p->l;
    }
    return (p);
}
Avl *Avl::del(Avl *T, string x)
{
    Avl *temp;
    if (T == NULL)
    {
        cout << "Element not found" << endl;
        return (T);
    }
    if (x > T->word)
    {
        T->r = del(T->r, x);
        if (BF(T) > 1)
        {
            if (BF(T->l) >= 0)
            {
                T = LL(T);
            }
            else
            {
                T = LR(T);
            }
        }
        return (T);
    }
    if (x < T->word)
    {
        T->l = del(T->l, x);
        if (BF(T) < -1)
        {
            if (BF(T->r) <= 0)
            {
                T = RR(T);
            }
            else
            {
                T = RL(T);
            }
        }
        return (T);
    }
    if (T->l == NULL && T->r == NULL)
    {
        temp = T;
        delete (temp);
        return (NULL);
    }
    if (T->l == NULL)
    {
        temp = T;
        T = T->r;
        delete (temp);
        return (T);
    }
    if (T->r == NULL)
    {
        temp = T;
        T = T->l;
        delete (temp);
        return (T);
    }
    temp = findmin(T->r);
    T->word = temp->word;
    T->mean = temp->mean;
    T->r = del(T->r, temp->word);
    return (T);
}
void Avl::search(Avl *T, string x)
{
    Avl *p;
    static int c = 0;
    p = T;
    while (p != NULL)
    {
        if (x > p->word)
        {
            p = p->r;
            c++;
        }
        else if (x < p->word)
        {
            p = p->l;
            c++;
        }
        else if (x == p->word)
        {
            c++;
            cout << "Number of comparisons: " << c << endl;
            return;
        }
    }
    cout << "Element not found" << endl;
    cout << "Number of comparisons: " << c;
}
Avl *Avl::update(Avl *T, string x)
{
    Avl *p;

    p = T;
    while (p != NULL)
    {
        if (x > p->word)
        {
            p = p->r;
        }
        else if (x < p->word)
        {
            p = p->l;
        }
        else if (x == p->word)
        {
            cout << "Enter the meaning of word:" << endl;
            cin >> p->mean;
            return (T);
        }
    }

    return (NULL);
}
int main()
{
    Avl *r, *m;
    Avl o;
    string a, b, c;
    int ch;
    while (true)
    {
        cout << "1.create" << endl;
        cout << "2.display" << endl;
        cout << "3.delete" << endl;
        cout << "4.search" << endl;
        cout << "5.update" << endl;
        cout << "Enter your choice:" << endl;
        cin >> ch;
        if (ch == 1)
        {
            r = o.create();
        }
        else if (ch == 2)
        {
            cout << "The dicƟonary is " << endl;
            o.display(r);
        }
        else if (ch == 3)
        {
            cout << "Enter the word you want to delete:" << endl;
            cin >> a;
            m = o.del(r, a);
        }
        else if (ch == 4)
        {
            cout << "Enter the word you want to search" << endl;
            cin >> b;
            o.search(m, b);
        }
        else if (ch == 5)
        {
            cout << "Enter the word whos meaning you want to update " << endl;
            cin >> c;
            m = o.update(m, c);
        }
        else
        {
            cout << "Wrong choice!!" << endl;
            break;
        }
    }
    return 0;
}
  
  
 #############################################################################################################
  
  #include <iostream>
using namespace std;
class Bstnode
{
public:
    int data;
    Bstnode *left, *right;

    Bstnode *insert(Bstnode *T, int x);
    Bstnode *create();
    void display(Bstnode *T);
    Bstnode *swap(Bstnode *T);
    Bstnode *find(Bstnode *T, int x);
    Bstnode *findmin(Bstnode *T);
    int longpath(Bstnode *T);
};
Bstnode *Bstnode::insert(Bstnode *T, int x)
{
    if (T == NULL)
    {
        T = new Bstnode;
        T->data = x;
        T->left = NULL;
        T->right = NULL;
        return T;
    }
    if (x > T->data)
    {
        T->right = insert(T->right, x);
        return T;
    }
    T->left = insert(T->left, x);
    return T;
}
Bstnode *Bstnode::create()
{
    int n, x;
    cout << "Enter the number of nodes: " << endl;
    cin >> n;
    Bstnode *root;
    root = NULL;
    for (int i = 0; i < n; i++)
    {
        cout << "Enter element to be store in node:" << endl;
        cin >> x;
        root = insert(root, x);
    }
    return root;
}
void Bstnode::display(Bstnode *T)
{
    if (T == NULL)
    {
        return;
    }
    display(T->left);
    cout << T->data << endl;
    display(T->right);
}
Bstnode *Bstnode::findmin(Bstnode *T)
{
    while (T->left != NULL)
    {
        T = T->left;
    }
    return T;
}
Bstnode *Bstnode::find(Bstnode *T, int x)
{
    if (T == NULL)
    {
        return NULL;
    }
    if (T->data == x)
    {
        return T;
    }
    if (x > T->data)
    {
        return (find((T->right), x));
    }
    else
        return (find((T->left), x));
}
int Bstnode::longpath(Bstnode *T)
{
    int hl, hr;
    if (T == NULL)
    {
        return 0;
    }
    if (T->left == NULL && T->right == NULL)
    {
        return 0;
    }
    hl = longpath(T->left);
    hr = longpath(T->right);
    if (hl > hr)
    {
        return (hl + 1);
    }
    return (hr + 1);
}
Bstnode *Bstnode::swap(Bstnode *T)
{
    Bstnode *p;
    if (T != NULL)
    {
        p = T->left;
        T->left = swap(T->right);
        T->right = swap(p);
    }
    return T;
}
int main()
{
    Bstnode obj;
    Bstnode *m, *n;
    int ch, h, x;
    while (true)
    {
        cout << "....Display Menu...." << endl;
        cout << "1.Create Binary search tree" << endl;
        cout << "2.InserƟon in Binary search tree" << endl;
        cout << "3.Display Binary search tree" << endl;
        cout << "4.longest path in Binary search tree" << endl;
        cout << "5.Minimum data in Binary search tree" << endl;
        cout << "6.Swapping Binary search tree" << endl;
        cout << "7.Searching in Binary search tree" << endl;
        cout << "Enter any choice except 7 choices to exit the program" << endl;
        cout << "Enter choice:" << endl;
        cin >> ch;
        if (ch == 1)
        {
            m = obj.create();
        }
        else if (ch == 2)
        {
            int a;
            cout << "Enter value to be inserted:" << endl;
            cin >> a;
            m = obj.insert(m, a);
        }
        else if (ch == 3)
        {
            cout << "Binary serach tree: " << endl;
            obj.display(m);
        }
        else if (ch == 4)
        {
            h = obj.longpath(m);
            cout << "The number of nodes in longest part is: " << h << endl;
        }
        else if (ch == 5)
        {
            n = obj.findmin(m);
            cout << "Minimum data in BST is: " << n->data << endl;
        }
        else if (ch == 6)
        {
            m = obj.swap(m);
            cout << "Binary serach tree aŌer swapping:" << endl;
            obj.display(m);
        }
        else if (ch == 7)
        {

            cout << "Enter value to be searched: " << endl;
            cin >> x;
            n = obj.find(m, x);
            if (n == NULL)
            {
                cout << "Element not found" << endl;
            }
            else
                cout << "Element found: " << n->data << endl;
        }
        else
        {
            cout << "Wrong choice entered!!" << endl;
            break;
        }
    }

    return 0;
}
  
  #####################################################################################################
  
  #include <iostream> 
#include <string> 
using namespace std; 
class Dict{ 
 public: 
 char word; 
 string meaning; 
 Dict *l,*r; 
 Dict *insert(Dict *T,char x,string a); 
 Dict *create(); 
 void display(Dict *T); 
 Dict *update(Dict *T); 
 void find(Dict *T,char a); 
 Dict *swap(Dict *T); 
 Dict *del(Dict *T,char x); 
 Dict *findmin(Dict *T); 
 
 
}; 
Dict* Dict::insert(Dict *T,char x,string a){ 
 if(T==NULL){ 
 T=new Dict; 
 T->l=NULL; 
 T->r=NULL; 
 T->word=x; 
 T->meaning=a; 
 return T; 
 } 
 if(x>T->word){ 
 T->r=insert(T->r,x,a); 
 return T; 
 } 
 T->l=insert(T->l,x,a); 
 return T; 
} 
Dict* Dict::create(){ 
 Dict *root; 
 char x; 
 string a; 
 root=NULL; 
 int n; 
 cout<<"Enter number of nodes:"<<endl; 
 cin>>n; 
 for(int i=0;i<n;i++){ 
 cout<<"Enter word:"<<endl; 
 cin>>x; 
 cout<<"Enter meaning:"<<endl; 
 cin>>a; 
 root=insert(root,x,a); 
 } 
 return root; 
} 
void Dict::display(Dict *T){ 
 if(T==NULL) 
 return; 
 display(T->l); 
 cout<<"The word is: "<<T->word<<" its meaning is: "<<T->meaning<<endl; 
 display(T->r); 
 
} 
void Dict::find(Dict *T,char x){ 
 
 Dict *p; 
 p=T; 
 static int c=0; //no static then also fine 
 while(p!=NULL){ 
 
 if(x>p->word){ 
 p=p->r; 
 c++; 
 } 
 if(x<p->word){ 
 p=p->l; 
 c++; 
 } 
 if(x==p->word){ 
 c++; 
 cout<<"Number of comparisons to find the word "<<x<<" is: "<<c<<endl; 
 return; 
 } 
 
 } 
 
 cout<<"Word not found"<<endl; 
} 
Dict* Dict::update(Dict *T){ 
 char a; 
 Dict *p; 
 cout<<"Enter the word whose meaning you want to change:"<<endl; 
 cin>>a; 
 p=T; 
 while(p!=NULL){ 
 
 if(a>p->word){ 
 p=p->r; 
 } 
 if(a<p->word){ 
 p=p->l; 
 } 
 if(a==p->word){ 
 cout<<"Enter the meaning of the word: "<<endl; 
 cin>>p->meaning; 
 return T; 
 } 
 
 } 
 
 return(NULL); 
} 
Dict* Dict::swap(Dict *T){ 
 Dict *p; 
 if(T!=NULL){ 
 p=T->l; 
 T->l=swap(T->r); 
 T->r=swap(p); 
 } 
 return T; 
} 
Dict* Dict::del(Dict *T,char x){ 
 Dict *temp; 
 if(T==NULL){ 
 cout<<"Element not found"<<endl; 
 return T; 
 } 
 if(x<T->word){ 
 T->l=del(T->l,x); 
 return T; 
 } 
 if(x>T->word){ 
 T->r=del(T->r,x); 
 return T; 
 } 
 if(T->l==NULL && T->r==NULL){ 
 temp=T; 
 delete(temp); 
 return(NULL); 
 
 } 
 if(T->l==NULL){ 
 temp=T; 
 T=T->r; 
 delete(temp); 
 return T; 
 } 
 if(T->r==NULL){ 
 temp=T; 
 T=T->l; 
 delete(temp); 
 return T; 
 } 
 temp=findmin(T->r); 
 T->word=temp->word; 
 T->meaning=temp->meaning; 
 T->r=del(T->r,temp->word); 
 return T; 
} 
Dict* Dict::findmin(Dict *T){ 
 while(T->l!=NULL){ 
 T=T->l; 
 } 
 return T; 
} 
int main() { 
 Dict o; 
 Dict *m,*n,*q; 
 char x,y; 
 m=o.create(); 
 cout<<"Data in ascending order:"<<endl; 
 o.display(m); 
 cout<<"Enter the word you want to search:"<<endl; 
 cin>>x; 
 o.find(m,x); 
 
 n=o.update(m); 
 o.display(n); 
 cout<<"Enter the word which you want to delete:"<<endl; 
 cin>>y; 
 q=o.del(n,y); 
 cout<<"dictionary after deletion:"<<endl; 
 o.display(q); 
 n=o.swap(n); 
 cout<<"Data in descending order:"<<endl; 
 o.display(n);
 return 0; 
} 
  
  
  #############################################################################
  
  #include <iostream>
#include <string>
using namespace std;
#define MAX 20
typedef struct node
{
    struct node *next;
    string vertex;
    int time;
} node;
node *G1[MAX];
void read_matrix(int G[MAX][MAX], string v[], int n)
{
    char ch;
    for (int i = 0; i < n; i++)
    {
        for (int j = 0; j < n; j++)
        {
            cout << "if path is present between " << v[i] << " and " << v[j] << " then enter y else n" << endl;
            cin >> ch;
            if (ch == 'y')
            {
                cout << "Enter time to reach in hrs: " << endl;
                cin >> G[i][j];
            }
            else if (ch == 'n')
            {
                G[i][j] = 0;
            }
        }
    }
}
void display_matrix(int G[MAX][MAX], int n)
{
    for (int i = 0; i < n; i++)
    {
        for (int j = 0; j < n; j++)
        {
            cout << G[i][j] << " ";
        }
        cout << endl;
    }
}
int getidx(string s1, int n)
{
    for (int i = 0; i < n; i++)
    {
        if (G1[i]->vertex == s1)
        {
            return i;
        }
    }
    return -1;
}
void insert(string city1, string city2, int fcost, int n)
{
    node *source, *destination;
    destination = new node;
    destination->next = NULL;
    destination->vertex = city2;
    destination->time = fcost;
    int idx = getidx(city1, n);
    source = G1[idx];
    while (source->next != NULL)
    {
        source = source->next;
    }
    source->next = destination;
}
void read_list(int n)
{
    string city1, city2;
    int fcost;
    int flight;
    for (int i = 0; i < n; i++)
    {
        G1[i] = new node;
        G1[i]->next = NULL;
        cout << "Enter name of city:" << endl;
        cin >> G1[i]->vertex;
    }
    cout << "Enter the number of flights:" << endl;
    cin >> flight;
    for (int i = 0; i < flight; i++)
    {
        cout << "Enter source city:" << endl;
        cin >> city1;
        cout << "Enter destination city:" << endl;
        cin >> city2;
        cout << "Enter time to reach in hrs:" << endl;
        cin >> fcost;
        insert(city1, city2, fcost, n);
    }
}
void disp_list(int n)
{
    node *p;
    for (int i = 0; i < n; i++)
    {
        p = G1[i];

        while (p != NULL)
        {
            cout << p->vertex << "->";
            p = p->next;
        }
        cout << "NULL" << endl;
    }
}
int main()
{
    int n;
    cout << "\nEnter the number of cities:" << endl;
    cin >> n;
    int c;
    while (true)
    {
        cout << "1.Adjancency Matrix" << endl;
        cout << "2.Adjancency List" << endl;
        cout << "Enter your choice:";
        cin >> c;
        if (c == 1)
        {
            string v[20];
            cout << "Enter cities:" << endl;
            for (int i = 0; i < n; i++)
            {
                cin >> v[i];
            }
            int G[MAX][MAX];
            read_matrix(G, v, n);
            cout << "Adjancency matrix is:" << endl;
            display_matrix(G, n);
        }
        else if (c == 2)
        {
            read_list(n);
            cout << "Adjancency list is:" << endl;
            disp_list(n);
        }
        else
        {
            cout << "Wrong choice entered" << endl;
            break;
        }
    }
    return 0;
}
  
  ######################################################################################################
  
  class TelephoneRecord: 
 def __init__(self, s): 
 #Declaring all 6 arrays 
 self.f=[] 
 self.fd=[] 
 self.lh=[] 
 self.dh=[] 
 self.dh1=[] 
 self.lh1=[] 
#Initializing both the counters 
 self.lp=0 
 self.d=0 
 #Initializing all 6 arrays 
 for i in range (s): 
 self.lh.append(None) 
 self.lh1.append(None) 
 self.dh.append(None) 
 self.dh1.append(None) 
 self.f.append(0) 
 self.fd.append(0) 
 self.size=s 
 def lpinsert(self, num,nam): 
 loc=num%self.size 
 for j in range(self.size): 
 if(self.f[loc]==0): 
 self.lh[loc]=num 
 self.lh1[loc]=nam 
 self.f[loc]=1 
 return loc 
 loc=(loc+1)%self.size 
 return -1 
 def lpsearch(self, num): 
 x=-1 
 loc=num%self.size 
 for j in range(self.size): 
 if(self.f[loc]==1 and self.lh[loc]==num ): 
 self.lp+=1 #Comparison occurs, so increment counter 
 x=loc 
 break 
 loc=(loc+1)%self.size 
 if(x==-1): 
 print("Element not found") 
 
 else: 
 print(num," found at location ",x,"(LP)") 
 print("It belongs to ",self.lh1[x]) 
 def dhinsert(self, num,nam): 
 loc=num%self.size 
 u=self.size-(num%self.size) 
 for j in range(self.size): 
 loc=(loc+(u*j))%self.size 
 if(self.fd[loc]==0): 
 self.dh[loc]=num 
 self.dh1[loc]=nam 
 self.fd[loc]=1 
 return loc 
 return -1 
 def dhsearch(self, num): 
 x=-1 
 loc=num%self.size 
 u=self.size-(num%self.size) 
 for j in range(self.size): 
 loc=(loc+(u*j))%self.size 
 if(self.fd[loc]==1 and self.dh[loc]==num): 
 self.d +=1 #Comparison occurs, so increment counter 
 x=loc 
 break 
 if(x==-1): 
 print("\n",num," not found.") 
 else: 
 print("\n",num," found at location ",x,"(DH)") 
 print("It belongs to ",self.dh1[x]) 
 def ldisp(self): 
 for i in range(self.size): 
 if(self.f[i]==1): 
 print(i,"\t",self.lh[i],"\t",self.lh1[i]) 
 else: 
 print(i,"\t...","\t\t...") 
 def ddisp(self): 
 for i in range(self.size): 
 if(self.fd[i]==1): 
 print(i,"\t",self.dh[i],"\t",self.dh1[i]) 
 else: 
 print(i,"\t...","\t\t...") 
 def comp(self): 
 print("\nNo. of comaprisons:") 
 print("1. Linear Probing Without Chaining: ",self.lp) 
 print("2. Double Hashing: ",self.d) 
 if(self.d<self.lp): 
 print("\nDouble Hashing used less comparisons as compared to Linear Probing Without 
Chaining\n") 
 elif(self.d>self.lp): 
 print("\nDouble Hashing used more comparisons as compared to Linear Probing 
Without Chaining.\n") 
 else: 
 print("\nDouble Hashing used same comparisons as that of Linear Probing Without 
Chaining.\n") 
def main(): 
 
 while True: 
 
 print("Enter your choice:") 
 print("1. Add element") 
 print("2. Search element") 
 print("3. Display Hash Table") 
 print("4. Exit") 
 ch=int(input()) 
 if(ch==4): 
 print("End of program.") 
 quit() 
 elif(ch==1): 
 print("Enter the number to be inserted:") 
 key=int(input()) 
 print("Enter the name to be inserted:") 
 key1=input() 
 x=ob.lpinsert(key,key1) #Calling LP insert function 
 y=ob.dhinsert(key,key1) #Calling DH insert function 
 if(x==-1 or y==-1): 
 print("Sorry! Hash Table is Full.") 
 else: 
 print(key," and ",key1," inserted at location ",x,"(LP)\n") 
 print(key," and ",key1," inserted at location ",y,"(DH)\n") 
 elif(ch==2): 
 print("Enter the number to be searched:") 
 key=int(input()) 
 ob.lpsearch(key) #Calling LP search function 
 ob.dhsearch(key) #Calling DH search function 
 ob.comp() #Calling the compare function 
 elif(ch==3): 
 print("Hash Table(Linear Probing):") 
 ob.ldisp() #Displaying the LP Hash table 
 print("\nHash Table(Double Hashing):") 
 ob.ddisp() #Displaying the DH Hash table 
print("Enter size:") 
s=int(input()) 
 
ob=TelephoneRecord(s) 
 
main()
  
  #######################################################################################################
  
  #include<iostream> 
using namespace std; 
void createmax(int[]); 
void display(int[]); 
void createmin(int[]); 
void downadj1(int[],int); 
void downadj2(int[],int); 
int main() 
{ 
 int heap[30],n,i,l,temp; 
 cout<<"\n Enter the total no. of students "; 
 cin>>n; 
 heap[0]=n; 
 cout<<"\n Enter the online exanimaƟon marks for students ";
 for(i=1;i<=n;i++) 
 cin>>heap[i]; 
 display(heap); 
 createmax(heap); 
 cout<<"\n Maximum marks = "<<heap[1]; 
 createmin(heap); 
 cout<<"\n Minimum marks = "<<heap[1]<<endl; 
 return 0; 
 } 
 
 void createmax(int heap[]) 
 { 
 int i,n; 
 n=heap[0]; 
 for(i=n/2;i>=1;i--) 
 downadj1(heap,i); 
 } 
 void display(int heap[]) 
 { 
 int i, n; 
 n=heap[0]; 
 cout<<"\n List of the marks = \t "; 
 for(i=1;i<=n;i++) 
 cout<<heap[i]<<"\t"; 
 } 
 
 void downadj1(int heap[],int i) 
 { 
 int j,temp,n,flag=1; 
 n=heap[0]; 
 while(2*i<=n&&flag==1) 
 { 
 j=2*i; 
 if(j+1<=n&&heap[j+1]>heap[j]) 
 j=j+1; 
 if(heap[i]>heap[j]) 
 flag=0; 
 else 
 { 
 temp=heap[i]; 
 heap[i]=heap[j]; 
 heap[j]=temp; 
 i=j; 
 } 
 } 
 } 
 
 void createmin(int heap[]) 
 { 
 int i,n; 
 n=heap[0]; 
 for(i=n/2;i>=1;i--) 
 downadj2(heap,i); 
 } 
 
 void downadj2(int heap[],int i) 
 { 
 int j,temp,n,flag=1; 
 n=heap[0]; 
 while(2*i<=n&&flag==1) 
 { 
 j=2*i; 
 if(j+1<=n&&heap[j+1]<heap[j]) 
 j=j+1; 
 if(heap[i]<heap[j]) 
 flag=0; 
 else 
 { 
 temp=heap[i]; 
 heap[i]=heap[j]; 
 heap[j]=temp; 
 i=j; 
 } 
 } 
 } 

  ########################################################################################################
  
  #include <iostream>
#include <string>
using namespace std;
#define MAX 10
class Node
{
public:
    Node *l, *r;
    int data;
    Node *optimal(float p[], float q[], int n);
    int find(float c[MAX][MAX], int i, int j);
    Node *construct(int r[MAX][MAX], int i, int j);
    void inorder(Node *T, string word[]);
};
Node *Node::optimal(float p[], float q[], int n)
{
    Node *root;
    float c[MAX][MAX], w[MAX][MAX];
    int r[MAX][MAX];
    int i, j, k, step;
    for (i = 0; i <= n; i++)
    {
        for (j = 0; j <= n; j++)
        {
            c[i][j] = w[i][j] = r[i][j] = 0;
        }
    }
    for (i = 1; i <= n; i++)
    {
        w[i][i] = q[i - 1] + q[i] + p[i];
        c[i][i] = w[i][i];
        r[i][i] = i;
    }
    for (step = 2; step <= n; step++)
    {
        for (i = 1; i <= n - step + 1; i++)
        {
            j = i + step - 1;
            w[i][j] = w[i][j - 1] + q[j] + p[j];
            k = find(c, i, j);
            c[i][j] = w[i][j] + c[i][k - 1] + c[k + 1][j];
            r[i][j] = k;
        }
    }
    cout << "Root of tree is:" << r[1][n] << endl;
    cout << "Cost of obst " << c[1][n] << endl;
    root = construct(r, 1, n);
    return (root);
}
int Node::find(float c[MAX][MAX], int i, int j)
{
    float min = 9999.0;
    int l, k;
    for (k = i; k <= j; k++)
    {
        if ((c[i][k - 1] + c[k + 1][j]) < min)
        {
            min = c[i][k - 1] + c[k + 1][j];
            l = k;
        }
    }
    return (l);
}
Node *Node::construct(int r[MAX][MAX], int i, int j)
{
    Node *p;
    if (r[i][j] == 0)
    {
        return (NULL);
    }
    else
    {
        p = new Node;
        p->data = r[i][j];
        p->l = construct(r, i, r[i][j] - 1);
        p->r = construct(r, r[i][j] + 1, j);
        return p;
    }
}
void Node::inorder(Node *p, string word[])
{
    if (p == NULL)
    {
        return;
    }
    inorder(p->l, word);
    cout << word[p->data] << endl;
    inorder(p->r, word);
}
int main()
{
    string word[10];
    int n;
    float p[MAX], q[MAX];
    Node *root = NULL;
    Node m;
    cout << "Enter the number of words:" << endl;
    cin >> n;
    cout << "Enter the words in chronological order:" << endl;
    for (int i = 1; i <= n; i++)
    {
        cin >> word[i];
    }
    cout << "Enter probabilities of success:" << endl;
    for (int i = 1; i <= n; i++)
    {
        cin >> p[i];
    }
    cout << "Enter probabilities of failure:" << endl;
    for (int i = 0; i <= n; i++)
    {
        cin >> q[i];
    }
    root = m.optimal(p, q, n);
    cout << "Optimal binary search tree is:" << endl;
    m.inorder(root, word);
    return 0;
}
  
  ########################################################################################
  
  #include <iostream>
using namespace std;
#define MAX 20
#define infinity 9999
int spanning[MAX][MAX];
void read_graph(int G[MAX][MAX], int n)
{
    cout << "Enter the adjancey matrix for phone lines that connect cities" << endl;
    cout << "Enter phone company charges " << endl;
    for (int i = 0; i < n; i++)
    {
        for (int j = 0; j < n; j++)
        {
            cin >> G[i][j];
        }
        cout << endl;
    }
}
void display(int n, int G[MAX][MAX])
{
    cout << "The adjancency matrix for phone lines that connect cities is:" << endl;

    for (int i = 0; i < n; i++)
    {
        for (int j = 0; j < n; j++)
        {

            cout << G[i][j] << " ";
        }
        cout << endl;
    }
}
int prims(int G[MAX][MAX], int n)
{
    int u, v, i, j, cost_matrix[MAX][MAX];
    int visited[MAX], from[MAX], distance[MAX], edges;
    int min_cost = 0;
    for (i = 0; i < n; i++)
    {
        for (j = 0; j < n; j++)
        {
            if (G[i][j] == 0)
            {
                cost_matrix[i][j] = infinity;
            }
            else
            {
                cost_matrix[i][j] = G[i][j];
            }
            spanning[i][j] = 0;
        }
    }
    visited[0] = 1;
    distance[0] = 0;
    for (i = 1; i < n; i++)
    {
        distance[i] = cost_matrix[0][i];
        visited[i] = 0;
        from[i] = 0;
    }
    edges = n - 1;
    while (edges > 0)
    {
        int min_dist = infinity;
        for (i = 1; i < n; i++)
        {
            if (visited[i] == 0 && distance[i] < min_dist)
            {
                min_dist = distance[i];
                v = i;
            }
        }
        u = from[v];
        spanning[u][v] = distance[v];
        spanning[v][u] = distance[v];
        edges--;
        visited[v] = 1;
        for (int i = 1; i < n; i++)
        {
            if (visited[i] == 0 && cost_matrix[i][v] < distance[i])
            {
                distance[i] = cost_matrix[i][v];
                from[i] = v;
            }
        }
        min_cost = min_cost + cost_matrix[u][v];
    }
    return (min_cost);
}
int main()
{
    int n;
    cout << "Enter number of cities:" << endl;
    cin >> n;
    int G[MAX][MAX];

    read_graph(G, n);
    display(n, G);
    int total_cost = prims(G, n);
    cout << "Spanning tree is:" << endl;
    display(n, spanning);

    cout << "Minimum total cost is: " << total_cost << endl;
    return 0;
}
  
  #######################################################################################
  
  class Set: 
 def __init__(self): 
 self.__s=set() 
 a=int(input("Enter size of set :")) 
 for i in range(a): 
 e=int(input("Enter element to be inserted:")) 
 self.__s.add(e) 
 print("Set is: ",self.__s) 
 def Add(self): 
 e1=int(input("Enter element to be added: ")) 
 self.__s.add(e1) 
 print("Set after adding an element: ",self.__s) 
 def Remove(self): 
 e1=int(input("Enter element to be deleted or removed:")) 
 if e1 in self.__s: 
 self.__s.remove(e1) 
 print("Set after deletion of element:",self.__s) 
 def __len__(self): 
 print("Size of set is : ",len(self.__s)) 
 def contains(self): 
 e1=int(input("Enter element who's presence in the set to be checked:")) 
 if e1 in self.__s: 
 return True 
 else: 
 return False 
 def union(self,s1): 
 self.s1=s1 
 new=set() 
 for i in self.__s: 
 new.add(i) 
 for j in self.s1: 
 if j not in self.__s: 
 new.add(j) 
 
 print("Union of two sets is: ",new) 
 def intersection(self,s2): 
 self.s2=s2 
 new=set() 
 for i in self.__s: #if i take s1 again then it will consider s1 set passed in union 
 for j in self.s2: 
 if i==j: 
 new.add(i) 
 print("Intersection of two set is: ",new) 
 def difference(self,s3): 
 self.s3=s3 
 new=set() 
 for i in self.__s: 
 if i not in self.s3: 
 new.add(i) 
 print("Set after difference of Set: ",new) 
 def subset(self,s4): 
 self.s4=s4 
 if (len(self.s4)<=(len(self.__s))): 
 for i in self.s4: 
 if i not in self.__s: 
 return False 
 return True 
 else: 
 return False 
 def __iter__(self): 
 return(iter(self.__s)) 
obj=Set() 
while True: 
 print("....Display Menu....") 
 print("1.Add element") 
 print("2.Remove element") 
 print("3.Size of Set") 
 print("4.Contains Element") 
 print("5.Union ") 
 print("6.Intersection") 
 print("7.Difference") 
 print("8.Subset") 
 print("9.Iterator") 
 print("If choice enter other than 9 choices then program exits") 
 ch=int(input("Enter your choice:")) 
 if(ch==1): 
 obj.Add() 
 elif(ch==2): 
 obj.Remove() 
 elif(ch==3): 
 obj.__len__() 
 elif(ch==4): 
 r=obj.contains() 
 print("Contains Element: ",r) 
 elif(ch==5): 
 s1=set() 
 a=int(input("Enter size of set s1: ")) 
 for i in range(a): 
 e=int(input("Enter element :")) 
 s1.add(e) 
 obj.union(s1) 
 elif(ch==6): 
 s2=set() 
 b=int(input("Enter size of set s2: ")) 
 for i in range(b): 
 e1=int(input("Enter element :")) 
 s2.add(e1) 
 obj.intersection(s2) 
 elif(ch==7): 
 s3=[] 
 c=int(input("Enter size of set s3: ")) 
 for i in range(c): 
 e2=int(input("Enter element :")) 
 s3.append(e2) 
 obj.difference(s3) 
 elif(ch==8): 
 s4=set() 
 d=int(input("Enter size of set s3: ")) 
 for i in range(d): 
 e3=int(input("Enter element :")) 
 s4.add(e3) 
 r1=obj.subset(s4) 
 print("Subset present: ",r1) 
 elif(ch==9): 
 r2=obj.__iter__() 
 print(r2) 
 else: 
 print("Wrong choice enter!!") 
 break
                                    
                          
                                    
  #######################################################################################################
                                    
                                    
  #include <iostream>
#include <fstream>
#include <string.h>
using namespace std;
class student
{
    typedef struct stud
    {
        int roll;
        char name[10];
        char div[2];
        char add[10];
    } stud;
    stud rec;

public:
    void create();
    void display();
    int search();
    void Delete();
};
void student::create()
{
    char ans;
    ofstream fout;
    fout.open("stud.txt", ios::out | ios::binary);
    do
    {
        cout << "\n\tEnter Roll No of Student : ";
        cin >> rec.roll;
        cout << "\n\tEnter a Name of Student : ";
        cin >> rec.name;
        cout << "\n\tEnter a Division of Student : ";
        cin >> rec.div;
        cout << "\n\tEnter the Address of Student : ";
        cin >> rec.add;
        fout.write((char *)&rec, sizeof(stud)) << flush;
        cout << "\n\tDo You Want to Add More Records: ";
        cin >> ans;
    } while (ans == 'y' || ans == 'Y');
    fout.close();
}
void student::display()
{
    ifstream fin;
    fin.open("stud.txt", ios::in | ios::binary);
    fin.seekg(0, ios::beg);
    cout << "\n\tThe Content of File are:\n";
    cout << "\n\tRoll\tName\tDiv\tAddress";
    while (fin.read((char *)&rec, sizeof(stud)))
    {
        if (rec.roll != -1)

            cout << "\n\t" << rec.roll << "\t" << rec.name << "\t" << rec.div << "\t" << rec.add;
    }
    fin.close();
}
int student::search()
{
    int r, i = 0;
    ifstream fin;
    fin.open("stud.txt", ios::in | ios::binary);
    fin.seekg(0, ios::beg);
    cout << "\n\tEnter a Roll No: ";
    cin >> r;
    while (fin.read((char *)&rec, sizeof(stud)))
    {
        if (rec.roll == r)
        {
            cout << "\n\tRecord Found...\n";
            cout << "\n\tRoll\tName\tDiv\tAddress";
            cout << "\n\t" << rec.roll << "\t" << rec.name << "\t" << rec.div << "\t" << rec.add;
            return i;
        }
        i++;
    }
    return -1;
    fin.close();
}
void student::Delete()
{
    int pos;
    pos = search();
    fstream f;
    f.open("stud.txt", ios::in | ios::out | ios::binary);
    f.seekg(0, ios::beg);
    if (pos == -1)
    {
        cout << "\n\tRecord Not Found";
        return;
    }
    int offset = pos * sizeof(stud);
    f.seekp(offset);
    rec.roll = -1;
    strcpy(rec.name, "NULL");
    // rec.div='N';
    strcpy(rec.add, "NULL");
    f.write((char *)&rec, sizeof(stud));
    f.seekg(0);
    f.close();
    cout << "\n\tRecord Deleted";
}
int main()
{
    student obj;
    int ch, key;
    char ans;
    do
    {
        cout << "\n\t***** Student Information *****";
        cout << "\n\t1. Create\n\t2. Display\n\t3. Delete\n\t4.Search\n\t5.Exit ";
        cout << "\n\t..... Enter Your Choice: ";
        cin >> ch;
        switch (ch)
        {
        case 1:
            obj.create();
            break;
        case 2:
            obj.display();
            break;
        case 3:
            obj.Delete();
            break;
        case 4:
            key = obj.search();
            if (key == -1)
                cout << "\n\tRecord Not Found...\n";
            break;
        case 5:
            break;
        }
        cout << "\n\t..... Do You Want to Continue in Main Menu: ";
        cin >> ans;
    } while (ans == 'y' || ans == 'Y');
    return 0;
}
  
  
  
  
  ###############################################################################################
  
  #include<iostream>
using namespace std;

struct tbt
{    int data;
    int lbit;
    int rbit;
    tbt *l, *r;
};
class Tree
{
public:
    int n = 0;
    tbt* in[200];
    tbt *create();
    tbt *insert(tbt *t, int x);
    void inorder(tbt *t);
    void inorderseq(tbt *t);
    void threading(tbt *t);
};
tbt* Tree::insert(tbt *t,int x)
{
	if(t==NULL)
	{
		t=new tbt;
		t->data=x;
		t->l=t->r=NULL;
		return t;
	}
	if(x > t->data)
	{
		t->r=insert(t->r,x);
		return t;
	}
	t->l=insert(t->l,x);
	return t;
}

tbt* Tree::create()
{	int i,x,n;
	tbt *root;
	root=NULL;
	cout<<"\nEnter no. of nodes : ";
	cin>>n;
	for(int i=0;i<n;i++)
	{
		cout<<"\nEnter data : ";
		cin>>x;
		root=insert(root,x);
	}
	return (root);
}



void Tree::inorderseq(tbt *t)
{	if(t!=NULL)
	{	inorderseq(t->l);
		in[n++]=t;
		if(t->l==NULL)
		{	t->lbit=0;		}
		else
		{	t->lbit=1;		}
		if(t->r==NULL)
		{	t->rbit=0;		}
		else
		{	t->rbit=1;		}
		inorderseq(t->r);
	}
}

void Tree::inorder(tbt *t)
{
	if(t!=NULL)
	{
		inorder(t->l);
		cout<<t->data<<" ";
		inorder(t->r);
	}
}

void Tree::threading(tbt *t)
{
	tbt *root,*q;
	root=new tbt;
	root->data=0;
	root->lbit=root->rbit=1;
	root->l=t;
	root->r=root;
	
	q=in[0];
	q->l=root;
	cout<<"thread to left of"<<q->data<<"is"<<q->l->data<<endl;
	if(q->rbit==0)
	{
		q->r=in[1];
		cout<<"thread to right of"<<q->data<<"is"<<q->r->data<<endl;
	}
	
	q=in[n-1];
	if(q->lbit==0)
	{
		q->l=in[n-2];
		cout<<"thread to left of"<<q->data<<"is"<<q->l->data<<endl;
	}
	q->r=root;
	cout<<"thread to right of"<<q->data<<"is"<<q->r->data<<endl;
	
	for(int i=1;i<(n-1);i++)
	{
		q=in[i];
		if(q->lbit==0)
		{
			q->l=in[i-1];
			cout<<"thread to left of"<<q->data<<"is"<<q->l->data<<endl;
		}
		if(q->rbit==0)
		{
			q->r=in[i+1];
			cout<<"thread to right of"<<q->data<<"is"<<q->r->data<<endl;
		}
	}
}

int main()
{
	Tree obj;
	tbt *h;
	h=obj.create();
	cout<<"\nInorder traversal is : ";
	obj.inorder(h);
	obj.inorderseq(h);
	cout<<"Conversion of BST to TBT is :- ";
	obj.threading(h);
	return 0;
}






