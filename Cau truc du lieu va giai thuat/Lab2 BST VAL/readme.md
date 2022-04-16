# НАЦИОНАЛЬНЫЙ ИССЛЕДОВАТЕЛЬСКИЙ УНИВЕРСИТЕТ ИНФОРМАЦИОННЫХ ТЕХНОЛОГИЙ, МЕХАНИКИ И ОПТИКИ

### Факультет инфокоммуникационных технологий
### **Дисциплина “Алгоритмы и структуры данных”**
### *Отчет к лабораторной работе №2 (семестр 2) «Двоичные деревья поиска»*
**Выполнил:**  студент группы К3139 -  Лам Ву Минь Ньят

**Принял:** преподаватель Татьяна Александровна Харьковская

*Санкт-Петербург*

*20/04/2022 год*

___

**Задание 4.**

**Create Node:**

```python
class node():
    def __init__(self, data = None, length =0, height = 1):
        self.data = data
        self.length = length
        self.height = height
        self.left = None
        self.right = None
```
**Build VAL tree:**

```python
class AVL():
    def __init__(self,data=None):
        self.root = None
        self.COUNT = 10
    def getLength(self,root):
        if root is None:
            return 0
        return root.length
    def getHeight(self, root = None):
        if root is None:
            return 0
        return root.height
    def rightRotate(self, root = None):
        x = root.left
        temp = x.right
        x.right = root 
        root.left = temp
        root.height = 1+ max (self.getHeight(root.left), self.getHeight(root.right))
        root.length = 1 + self.getLength(root.left) + self.getLength(root.right)
        x.height = 1 + max(self.getHeight(x.left),self.getHeight(x.right))
        x.length = 1 + self.getLength(x.left) + self.getLength(x.right)
        return x
    def leftRotate(self, root):
        y = root.right 
        temp = y.left
        y.left = root 
        root.right = temp
        root.height = 1+ max (self.getHeight(root.left), self.getHeight(root.right))
        root.length = 1 + self.getLength(root.left) + self.getLength(root.right)
        y.height = 1 + max(self.getHeight(y.left),self.getHeight(y.right)) 
        y.length = 1 + self.getLength(y.left) + self.getLength(y.right)
        return y 
    def insert(self,root,value):
        if root is None:
            return node(value,1,1)
        if (value > root.data):
            root.right = self.insert(root.right,value)
        if (value < root.data):
            root.left = self.insert(root.left,value)
        if (value == root.data):
            return root
        
        root.height = max(self.getHeight(root.left),self.getHeight(root.right)) + 1

        root.length = self.getLength(root.left) + self.getLength(root.right) + 1

        valBalance = self.getHeight(root.left) - self.getHeight(root.right)

        # 3.1. Left left
        if (valBalance > 1 and value < root.left.data):
            return self.rightRotate(root)
 
        # 3.2. Right Right
        if (valBalance < -1 and value > root.right.data):
            return self.leftRotate(root)
 
        # 3.3. Left Right
        if (valBalance > 1 and value > root.left.data):
            root.left = self.leftRotate(root.left)
            return self.rightRotate(root)
    
    
        # 3.4. Right Left
        if (valBalance < -1 and value < root.right.data):
            root.right = self.rightRotate(root.right)
            return self.leftRotate(root)
    
        return root
    def addition(self , value):
        self.root = self.insert(self.root,value)
    def find(self, k, root = -1):
        if root == -1: 
            root = self.root
        left = root.left
        right = root.right
        if (left == None): 
            left = node()
        if (left.length +1 == k):
            return root.data
        if (k<=left.length):
            return self.find(k , left)
        else: return self.find( k - left.length -1, right)
    def print(self, root = -1, spaces = 0): 
        if root == -1: 
            root = self.root
        if (root is None):
            return 
        spaces = spaces + self.COUNT
    
        self.print(root.right, spaces)
        print()
        print("".join((spaces-self.COUNT)*[' ']),end = '')
        print(f"{root.data} H({root.height}) L({root.length})")
    
        self.print(root.left, spaces)
```

**Solution**
```python
def main(): 
    dataInput = "input.txt"
    dataOutput = "output.txt"
    fi = open(dataInput,"r",encoding="utf-8")
    fo = open(dataOutput,"w",encoding= "utf-8")
    requests = fi.readlines()
    avl = AVL()
    for request in requests:
        req = request.split(" ")
        if (req[0] == "+"):
            avl.addition(int(req[1]))
            avl.print()
            print(''.join(80*['=']))
        if (req[0] == "?"):
            fo.write(str(avl.find(int(req[1])))+"\n")
```
**Описание решения:**
- I use BST tree and VAL tree to solve this problem:

- VAL: I do techniques to balance the tree.

- Node: I add a Length value: value to store the number of elements that the node contains.

- request '+': add value to tree 
- request '?': Finds and print the Kth element of the tree by looking at the length of node to determine where K is located.

____


**Задание 6.**

**Solution**
```python

def isBST(st,root, _min = -10000000, _max= 10000000):
    if len(st) == 0: return True #
    if root<0:return True
    val, l,r = st[root][0],st[root][1],st[root][2]
    if (val < _min or val > _max): return False
    return isBST(st,l,_min,val -1) and isBST(st,r,val +1,_max)

def main(): 
    fi = open("input.txt", "r")
    fo = open("output.txt", "w")
    n= int(fi.readline())
    temp = list(fi.readlines())
    st=[]
    for i in temp:
        t = list(map(int,i.split()))
        st.append(t)
    print(isBST(st,0))
    fi.close()
    fo.close()

if __name__ == "__main__":
    main()

```
**Описание решения:**

- If the node is empty then it is a BST tree.
- I check if the node satisfies the conditions of the BST tree?
 + left child node < root node 
 + right child node > root node
i'm tried with test
```
6
4 1 2
2 3 4 
5 -1 5 
1 -1 -1 
4 -1 -1  
9 -1 -1  

```
it look at:

____


**Задание 7.**

**Solution**
```python


def isBST(st,root, _min = -10000000, _max= 10000000):
    if len(st) == 0: return True #
    if root<0:return True
    val, l,r = st[root][0],st[root][1],st[root][2]
    if (val < _min or val > _max): return False
    return isBST(st,l,_min,val -1) and isBST(st,r,val,_max)

def main(): 
    fi = open("input.txt", "r")
    fo = open("output.txt", "w")
    n= int(fi.readline())
    temp = list(fi.readlines())
    st=[]
    for i in temp:
        t = list(map(int,i.split()))
        st.append(t)
    print(isBST(st,0))
    fi.close()
    fo.close()

if __name__ == "__main__":
    main()
    

```
**Описание решения:**

- If the node is empty then it is a BST tree.
- I check if the node satisfies the conditions of the BST tree?
 + left child node < root node 
 + right child node >= root node

____


**Задание 9.**

**Solution**
```python
class BST():
    def __init__(self,nodes):
        self.nodes = nodes
    def findChild(self,root):
        val,l,r = self.nodes[root]
        if (root == 0): return 0
        return 1+ self.findChild(l) + self.findChild(r)
    def delete(self, key, root = 1):
        if root == 0: return 0
        val,l,r = self.nodes[root]
        if (self.nodes[l][0] == key):
            x = self.findChild(l)
            self.nodes[root] = [val,0,r]
            return x
        if (self.nodes[r][0] == key):
            x = self.findChild(r)
            self.nodes[root] = [val,l,0]
            return x
        if (val > key): return self.delete(key, l) 
        if (val < key): return self.delete(key, r)
        return 0
def main(): 
    fi = open("input.txt", "r")
    fo = open("output.txt", "w")
    n= int(fi.readline())
    
    nodes = [[0,0,0]]
    for i in range(n):
        temp = fi.readline()
        nodes.append(list(map(int,temp.split())))
    
    bst = BST(nodes)
    m = int(fi.readline())
    requests = list(map(int,fi.readline().split()))
    #print(requests)
    for request in requests:
        
        x = bst.delete(request)
        #bst.print()
        #print("".join(80*["="]))
        n= n-x
        fo.write(str(n)+'\n')
    fi.close()
    fo.close()

if __name__ == "__main__":
    main()

```
**Описание решения:**

- Because the input is already a BST tree, I don't need to rebuild the tree.
- Before delete node[i] i will check how many child nodes it has. and update the total number of nodes in the tree.
____










**Задание .**

**Solution**
```python

```
**Описание решения:**

____