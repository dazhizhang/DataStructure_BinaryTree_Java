# DataStructure_BinaryTree_Java
二叉树的遍历 递归非递归 思路和 java实现


http://blog.csdn.net/clam_clam/article/details/6845399


二叉树遍历首先弄清遍历流程。


中序遍历:第一次经过从它找左，第二次经过找右，第三次经过回来。第二次经过访问它。
算法实现：


第二次访问是 pop（&S,&p）;Visit（p->data）;因为中序遍历跟第三次经过没关系。所以第二次就弹出栈了。
Java版的程序如下：
[java] view plain copy
package com.tree;  
import java.util.Stack;  
public class BinaryTree {  
    protected Node root;    
        
    public BinaryTree(Node root) {    
        this.root = root;    
    }    
    
    public Node getRoot() {    
        return root;    
    }    
    
    /** 构造树 */    
    public static Node init() {    
        Node a = new Node('A');    
        Node b = new Node('B', null, a);    
        Node c = new Node('C');    
        Node d = new Node('D', b, c);    
        Node e = new Node('E');    
        Node f = new Node('F', e, null);    
        Node g = new Node('G', null, f);    
        Node h = new Node('H', d, g);    
        return h;// root    
    }    
    
    /** 访问节点 */    
    public static void visit(Node p) {    
        System.out.print(p.getKey() + " ");    
    }    
    
    /** 递归实现前序遍历 */    
    protected static void preorder(Node p) {    
        if (p != null) {    
            visit(p);    
            preorder(p.getLeft());    
            preorder(p.getRight());    
        }    
    }    
    
    /** 递归实现中序遍历 */    
    protected static void inorder(Node p) {    
        if (p != null) {    
            inorder(p.getLeft());    
            visit(p);    
            inorder(p.getRight());    
        }    
    }    
    
    /** 递归实现后序遍历 */    
    protected static void postorder(Node p) {    
        if (p != null) {    
            postorder(p.getLeft());    
            postorder(p.getRight());    
            visit(p);    
        }    
    }    
  /**********************************************************************************************/  
    /** 非递归实现前序遍历 */    
    protected static void iterativePreorder(Node p) {    
        Stack<Node> stack = new Stack<Node>();    
        if (p != null) {    
            stack.push(p);    
            while (!stack.empty()) {    
                p = stack.pop();    
                visit(p);    
                if (p.getRight() != null)    
                    stack.push(p.getRight());    
                if (p.getLeft() != null)  //为什么p.getLeft() 在后，getRight()在前应为while 循环第一句就是pop visit所以要把left放上，先访问。之中方法是即压即访问法。  
                    stack.push(p.getLeft());    
            }    
        }    
    }    
      
    /** 非递归实现中序遍历 */  //思路与上面iterativePreorder 一致。  
    protected static void iterativeInorder(Node p) {    
        Stack<Node> stack = new Stack<Node>();    
        while (p != null) {    
            while (p != null) {    
                if (p.getRight() != null)    
                    stack.push(p.getRight());// 当前节点右子入栈    
                    stack.push(p);// 当前节点入栈    
                    p = p.getLeft();    
            }    
            p = stack.pop();    
            while (!stack.empty() && p.getRight() == null) {    
                visit(p);    
                p = stack.pop();    
            }    
            visit(p);    
            if (!stack.empty())    
                p = stack.pop();    
            else    
                p = null;    
        }    
    }  
  
/*******************************************************************************************/  
      
/*******************************************************************************************/    
    /** 非递归实现前序遍历2 */    
    protected static void iterativePreorder2(Node p) {    
        Stack<Node> stack = new Stack<Node>();    
        Node node = p;    
        while (node != null || stack.size() > 0) {    
            while (node != null) {//压入所有的左节点，压入前访问它。左节点压入完后pop访问右节点。像这样算法时思考规律性的东西在哪。不管哪个节点都要压所节点判断右节点。    
                visit(node);    
                stack.push(node);    
                node = node.getLeft();    
            }    
            if (stack.size() > 0) {//    
                node = stack.pop();    
                node = node.getRight();    
            }    
        }    
    }    
      
    /** 非递归实现中序遍历2 */    
    protected static void iterativeInorder2(Node p) {    
        Stack<Node> stack = new Stack<Node>();    
        Node node = p;    
        while (node != null || stack.size() > 0) {    
            while (node != null) {    
                stack.push(node);    
                node = node.getLeft();    
            }    
            if (stack.size() > 0) {    
                node = stack.pop();    
                visit(node);   //与iterativePreorder2比较只有这句话的位置不一样，弹出时再访问。  
                node = node.getRight();    
            }    
        }    
    }  
      
 /*******************************************************************************************/  
    
    /** 非递归实现后序遍历 */    
    protected static void iterativePostorder(Node p) {    
        Node q = p;    
        Stack<Node> stack = new Stack<Node>();    
        while (p != null) {    
            // 左子树入栈    
            for (; p.getLeft() != null; p = p.getLeft())    
                stack.push(p);    
            // 当前节点无右子或右子已经输出    
            while (p != null && (p.getRight() == null || p.getRight() == q)) {    
                visit(p);    
                q = p;// 记录上一个已输出节点    
                if (stack.empty())    
                    return;    
                p = stack.pop();    
            }    
            // 处理右子    
            stack.push(p);    
            p = p.getRight();    
        }    
    }    
    
    /** 非递归实现后序遍历 双栈法 */    
    protected static void iterativePostorder2(Node p) {//理解左子树   右子树 根递归性质，把它运用到循环当中去。    
        Stack<Node> lstack = new Stack<Node>();//左子树栈    
        Stack<Node> rstack = new Stack<Node>();//右子树栈  
        Node node = p, right;    
        do {    
            while (node != null) {    
                right = node.getRight();    
                lstack.push(node);    
                rstack.push(right);    
                node = node.getLeft();    
            }    
            node = lstack.pop();    
            right = rstack.pop();    
            if (right == null) {    
                visit(node);    
            } else {    
                lstack.push(node);    
                rstack.push(null);    
            }    
            node = right;    
        } while (lstack.size() > 0 || rstack.size() > 0);    
    }    
    
    /** 非递归实现后序遍历 单栈法*/    
    protected static void iterativePostorder3(Node p) {    
        Stack<Node> stack = new Stack<Node>();    
        Node node = p, prev = p;    
        while (node != null || stack.size() > 0) {    
            while (node != null) {    
                stack.push(node);    
                node = node.getLeft();    
            }    
            if (stack.size() > 0) {    
                Node temp = stack.peek().getRight();    
                if (temp == null || temp == prev) {    
                    node = stack.pop();    
                    visit(node);    
                    prev = node;    
                    node = null;    
                } else {    
                    node = temp;    
                }    
            }    
    
        }    
    }    
    
    /** 非递归实现后序遍历4 双栈法*/    
    protected static void iterativePostorder4(Node p) {    
        Stack<Node> stack = new Stack<Node>();    
        Stack<Node> temp = new Stack<Node>();    
        Node node = p;    
        while (node != null || stack.size() > 0) {    
            while (node != null) {    
                temp.push(node);    
                stack.push(node);    
                node = node.getRight();    
            }    
            if (stack.size() > 0) {    
                node = stack.pop();    
                node = node.getLeft();    
            }    
        }    
        while (temp.size() > 0) {//把插入序列都插入到了temp。  
            node = temp.pop();    
            visit(node);    
        }    
    }    
    
    /**  
     * @param args  
     */    
    public static void main(String[] args) {    
        BinaryTree tree = new BinaryTree(init());   
        System.out.print(" 递归遍历 \n");    
        System.out.print(" Pre-Order:");    
        preorder(tree.getRoot());    
           
        System.out.print(" \n In-Order:");    
        inorder(tree.getRoot());  
          
        System.out.print("\n Post-Order:");    
        postorder(tree.getRoot());    
          
        System.out.print(" \n非递归遍历");  
        System.out.print(" \n Pre-Order:");    
        iterativePreorder(tree.getRoot());    
          
        System.out.print("\n Pre-Order2:");    
        iterativePreorder2(tree.getRoot());    
           
        System.out.print(" \n In-Order:");    
        iterativeInorder(tree.getRoot());  
          
        System.out.print("\n In-Order2:");    
        iterativeInorder2(tree.getRoot());    
          
        System.out.print("\n Post-Order:");    
        iterativePostorder(tree.getRoot());    
         
        System.out.print("\n Post-Order2:");    
        iterativePostorder2(tree.getRoot());    
           
        System.out.print("\n Post-Order3:");    
        iterativePostorder3(tree.getRoot());    
           
        System.out.print("\n Post-Order4:");    
        iterativePostorder4(tree.getRoot());    
        
    
    }    
  
}  
   
  
class Node {    
    private char key;    
    private Node left, right;    
    
    public Node(char key) {    
        this(key, null, null);    
    }    
    
    public Node(char key, Node left, Node right) {    
        this.key = key;    
        this.left = left;    
        this.right = right;    
    }    
    
    public char getKey() {    
        return key;    
    }    
    
    public void setKey(char key) {    
        this.key = key;    
    }    
    
    public Node getLeft() {    
        return left;    
    }    
    
    public void setLeft(Node left) {    
        this.left = left;    
    }    
    
    public Node getRight() {    
        return right;    
    }    
    
    public void setRight(Node right) {    
        this.right = right;    
    }    
}    

运行结果如下：
[java] view plain copy
 递归遍历   
 Pre-Order:H D B A C G F E    
 In-Order:B A D C H G E F   
 Post-Order:A B C D E F G H    
非递归遍历   
 Pre-Order:H D B A C G F E   
 Pre-Order2:H D B A C G F E    
 In-Order:B A D C H G E F   
 In-Order2:B A D C H G E F   
 Post-Order:A B C D E F G H   
 Post-Order2:A B C D E F G H   
 Post-Order3:A B C D E F G H   
 Post-Order4:A B C D E F G H   

交换左右子树结点：
如下图：
   交换后：

[java] view plain copy
protected  void reverserLeftAndRight(Node p) {  
        if(p==null){//这句话绝对不能丢。不然有空指针异常，因为后面操作了p.getLeft();递归的话递归结束条件很重要，所以p==null 判断很重要，不要根节点就不会。要考虑所有情况。  
            return;  
        }  
        if(null==p.getLeft()&&null==p.getRight())  
            return;  
        Node temp=p.getLeft();  
        p.setLeft(p.getRight());  
        p.setRight(temp);  
        reverserLeftAndRight(p.getLeft());  
        reverserLeftAndRight(p.getRight());  
    }  
      
    protected  void reverserLeftAndRight1(Node root) {  
        if(root==null){  
            return;  
        }  
        if(null==root.getLeft()&&null==root.getRight())  
            return;  
        Queue<Node> qu=new LinkedList<Node>();  
        qu.add(root);  
        Node temp;  
        Node q=root;  
        while(!qu.isEmpty()){  
            if(null!=q.getLeft()){//这个必须有。不然把是null 的也加进来了。  
                qu.add(q.getLeft());  
            }  
            if(null!=q.getRight()){  
                qu.add(q.getRight());  
            }  
            temp=q.getLeft();  
            q.setLeft(q.getRight());  
            q.setRight(temp);  
            q=qu.remove();  
        }  
    }  
添加上面两个函数交换左右子树，一个递归算法，一个非递归（不一定用栈这里用的是队列）。
运行结果如下：

[java] view plain copy
未交换前的路径：  
路径：  
  H  D  B  A  
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>  
路径：  
  H  D  C  
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>  
路径：  
  H  G  F  E  
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>  
 递归交换子树>>>>>>  
  
路径：  
  H  G  F  E  
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>  
路径：  
  H  D  C  
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>  
路径：  
  H  D  B  A  
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>  
 非递归交换左右子树（又回到原来状态）  
  
路径：  
  H  G  F  E  
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>  
路径：  
  H  D  C  
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>  
路径：  
  H  D  B  A  
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>  


转自：http://www.iteye.com/topic/459058
关于二叉树的遍历还说一点：
前序序列获得的是根(第一个节点);
中序序列获得的是左右子树划分;
后序序列获得的是根(最后一个节点)。
前序序列和后序序列只能获得根，不能获得左右子树划分,所以不能唯一确定一颗二叉树。
(前序+中序)或者(后序+中序)都能唯一确定一颗二叉树。
如下图所示：

可以看到首先就通过先序a确立了根。通过中序确立了a 的左右子树。
遍历应用1：输出叶子跟结点到叶子结点的路径。
代码如下：
[java] view plain copy
/** 得到根结点到叶子结点的路径*/    
    protected  void getPathFromRootToLeaf(Node p) {    
        //this.path;  
        if(null==p.getLeft()&&null==p.getRight()){  
            path[pathLength++]=p.getKey();  
            System.out.print("\n路径：\n");  
            for(int i=0;i<pathLength;i++){  
                System.out.print("  "+path[i]);  
            }  
            System.out.print("\n>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>");  
            pathLength--;//注意：每pathLength++，就要对应pathLength--;  
            return;  
        }else{  
            if(null!=p.getLeft()){  
                path[pathLength++]=p.getKey();  
                getPathFromRootToLeaf(p.getLeft());  
                pathLength--;//每pathLength++，就要对应pathLength--;  
            }  
            if(null!=p.getRight()){  
                path[pathLength++]=p.getKey();  
                getPathFromRootToLeaf(p.getRight());  
                pathLength--;//每pathLength++，就要对应pathLength--;  
            }  
        }  
        //pathLength--;  
    }  
main 函数里：
[java] view plain copy
tree.getPathFromRootToLeaf(tree.getRoot());  
所以结果输出为：
[java] view plain copy
路径：  
  H  D  B  A  
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>  
路径：  
  H  D  C  
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>  
路径：  
  H  G  F  E  
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>  
注意:每pathLength++,就要对应一个pathLength- -;
