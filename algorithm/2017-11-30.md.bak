题目描述:
给定一个句子（只包含字母和空格）， 将句子中的单词位置反转，单词用空格分割, 单词之间只有一个空格，前后没有空格。 比如： （1） “hello xiao mi”-> “mi xiao hello”

输入描述:

输入数据有多组，每组占一行，包含一个句子(句子长度小于1000个字符)

输出描述:

对于每个测试示例，要求输出句子中单词反转后形成的句子

示例

输入
```
hello xiao mi
```
输出
```
mi xiao hello
```
代码:
```


import java.util.Scanner;
public class Main {

//句子反转
public static void main(String[] args) {
    public static void main(String args[])
    {
        Scanner sc = new Scanner(System.in);
        String s = sc.nextLine();
        StringBuilder sb = new StringBuilder();

        String arr[] = s.split(" ");
        String t = "";
        for(int i=arr.length -1;i >= 0;i--) {
            sb.append(arr[i]).append(" ");
        }
        System.out.println(sb.toString().substring(0,sb.toString().length()-1));
    }
}

```
- - -
<br><br>
题目：
现在有一棵合法的二叉树，树的节点都是用数字表示，现在给定这棵树上所有的父子关系，求这棵树的高度

输入描述:

输入的第一行表示节点的个数n（1 ≤ n ≤ 1000，节点的编号为0到n-1）组成，
下面是n-1行，每行有两个整数，第一个数表示父节点的编号，第二个数表示子节点的编号

输出描述:

输出树的高度，为一个整数

```
5
0 1
0 2
1 3
1 4
```
输出

```
3
```

```

import java.util.HashMap;
import java.util.Scanner;

public class test
{
    public static void main(String[] args)
    {
        Scanner sc = new Scanner(System.in);
        int numOfNode = sc.nextInt();
        HashMap<Integer,TreeNode>nodeMap = new HashMap<>();
        int rootIndex = sc.nextInt();
        TreeNode root = new TreeNode(rootIndex);
        int firstChildIndex = sc.nextInt();
        TreeNode firstChild = new TreeNode(firstChildIndex);
        root.left = firstChild;
        nodeMap.put(rootIndex,root);
        nodeMap.put(firstChildIndex,firstChild);
        for(int i=0;i < numOfNode - 2;i++)
        {
            int parentIndex = sc.nextInt();
            int childIndex = sc.nextInt();
            TreeNode child = new TreeNode(childIndex);
            TreeNode parent = nodeMap.get(parentIndex);
            if(parent!=null)
            {
                if(parent.left==null)
                {
                    parent.left = child;
                }
                else if(parent.right == null)
                {
                    parent.right = child;
                }
            }
            else
            {
                parent = new TreeNode(parentIndex);
                parent.left = child;
            }
            nodeMap.put(parentIndex,parent);
            nodeMap.put(childIndex,child);
        }

        System.out.println(getDeep(nodeMap.get(rootIndex)));
    }

    static int getDeep(TreeNode node)
    {
        if(node == null)
        {
            return 0;
        }
        int left = getDeep(node.left);
        int right = getDeep(node.right);
        return (left >right ? left : right)+1;
    }

    static class TreeNode
    {
        public TreeNode(int index) {
            this.index = index;
        }

        int index;
        TreeNode left = null;
        TreeNode right = null;
    }
}
```
- - -
<br><br>




















