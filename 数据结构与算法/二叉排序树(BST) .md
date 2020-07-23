# 介绍

对《数据结构与算法 Java语言描述》一书中的二叉排序树代码进行扩展和简化，所有的代码都可以在[WarMj的代码仓库](https://github.com/WarMj/SFC_Algorithm/tree/master/Tree)中找到，均有详细的注释。

具体如下：

**简化：**

1. 没有泛型，节点中保存的值为 `int` 类型。

**扩展：**

1. 添加 `printTree()` 方法，实现前、中、后序遍历
2. 实现了书中所简略的 `removeMin()`方法


# BinaryNode 节点类

```java
	/**
     * 节点内部类
     */
    private static class BinaryNode {
        int val; // 值
        BinaryNode left; // 左节点
        BinaryNode right; // 右节点

        BinaryNode(int val) {
            this.val = val;
        }

        BinaryNode(int val, BinaryNode left, BinaryNode right) {
            this.val = val;
            this.left = left;
            this.right = right;
        }

        /**
         * 前序遍历
         *
         * @param t 开始遍历的节点
         */
        private void preOrder(BinaryNode t) {
            if (t != null) {
                System.out.println(t.val);
                preOrder(t.left);
                preOrder(t.right);
            }
        }

        /**
         * 中序遍历
         *
         * @param t 开始遍历的节点
         */
        private void infixOrder(BinaryNode t) {
            if (t != null) {
                infixOrder(t.left);
                System.out.println(t.val);
                infixOrder(t.right);
            }
        }

        /**
         * 后序遍历
         *
         * @param t 开始遍历的节点
         */
        private void postOrder(BinaryNode t) {
            if (t != null) {
                postOrder(t.left);
                postOrder(t.right);
                System.out.println(t.val);
            }
        }
    }
```

# BinarySearchTree 类初始化

## 根节点
```java
    /**
     * 根节点，初始化为null
     */
    private BinaryNode root;
```

## 构造方法

```java
    /**
     * 构造方法
     */
    public BinarySearchTree() {
        root = null;
    }

```

# makeEmpty() 格式化二叉树

```java
    /**
     * 判树是否为空
     *
     * @return true为空 false不为空
     */
    public boolean isEmpty() {
        return root == null;
    }
```

# contains() 判断是否存在节点

```java
    /**
     * 是否有值为x的节点
     * 
     * @param x 查询节点的值
     * @return 有为true 否则为false
     */
    public boolean contains(int x) {
        return contains(x, root);
    }
    
    /**
     * 是否有值为x的节点
     *
     * @param x 查询节点的值
     * @param t 起始节点
     * @return 匹配到为true，否则为false
     */
    private boolean contains(int x, BinaryNode t) {
        if (t == null) {
            return false; // 遍历完了也没有查到
        }

        if (x < t.val) {
            return contains(x, t.left); // 在左子树递归查询
        } else if (x > t.val) {
            return contains(x, t.right); // 在右子树递归查询
        } else {
            return true; // 匹配到了
        }
    }
```

# printTree() 遍历树

注意，下面的代码仅仅为类中的选择哪种遍历的方法，具体遍历的方法在节点类中。

```
    /**
     * 遍历打印树 默认为前序遍历
     *
     * @param k 0前序  1中序  2后序
     */
    public void printTree(int k) {
        if (isEmpty()) {
            throw new NullPointerException("无法遍历，当前树为空");
        } else {
            switch (k) {
                case 1:
                    root.infixOrder(root);
                    break;
                case 2:
                    root.postOrder(root);
                    break;
                default:
                    root.preOrder(root);
            }
        }
    }
```


# insert() 插入新节点

```java
    /**
     * 插入新节点
     *
     * @param x 新节点的值
     */
    public void insert(int x) {
        root = insert(x, root);
    }
    
	/**
     * 插入新节点
     *
     * @param x 新节点的值
     * @param t 起始节点
     * @return 如果插入成功 返回插入后的节点，否则返回节点 t
     */
    private BinaryNode insert(int x, BinaryNode t) {
        // 如果被插入的节点为空，则直接创建新节点，值为x
        if (t == null) {
            return new BinaryNode(x, null, null);
        }

        if (x < t.val) {
            // x小于当前节点的值，说明要插入到左子树中
            t.left = insert(x, t.left);
        } else if (x > t.val) {
            // x大于当前节点的值，说明要插入到右子树中
            t.right = insert(x, t.right);
        } else {
            // 没有找到，直接返回t，这一段不写也可以
            return t;
        }

        return t;
    }
```

# 查询树中的最值

## findMax() 返回最大值

```
    /**
     * 查询树中的最大值
     *
     * @return 最大值
     */
    public int findMax() {
        if (isEmpty()) {
            throw new NullPointerException("无法遍历，当前树为空");
        }

        return findMax(root).val;
    }

    /**
     * 递归返回最大节点
     *
     * @param t 起始节点
     * @return 返回最大节点
     */
    private BinaryNode findMax(BinaryNode t) {
        if (t == null) {
            return null; // 起始节点为空
        } else if (t.right == null) {
            return t; // 找到最大节点
        }
        return findMax(t.right); // 没有找到最大节点，右递归遍历
    }
```

## findMin() 返回最小值

```java
    /**
     * 查询树中的最小值
     *
     * @return 最小值
     */
    public int findMin() {
        if (isEmpty()) {
            throw new NullPointerException("无法遍历，当前树为空");
        }

        return findMin(root).val;
    }
    
    /**
     * 递归返回最小节点
     *
     * @param t 起始节点
     * @return 返回最小节点
     */
    private BinaryNode findMin(BinaryNode t) {
        if (t == null) {
            return null; // 起始节点为空
        } else if (t.left == null) {
            return t; // 找到最小节点
        }
        return findMin(t.left); // 没有找到最小节点，左递归遍历
    }
```

# remove() 删除节点

需要使用特殊的removeMin方法，进行树的旋转
详见removeMin方法

```java
    /**
     * 具体实现
     * 根据值移除元素
     *
     * @param x 待移除元素的值
     * @param t 从哪个节点开始执行
     * @return 待移除元素右子树中的最小子节点
     */
    private BinaryNode remove(int x, BinaryNode t) {
        if (t == null) return t; // 节点为空

        if (x < t.val) {
            /*
             * 如果 x 小于当前节点的值，说明待删除节点可能在左树，不可能在右树
             * 左递归删除
             */
            t.left = remove(x, t.left);
        } else if (x > t.val) {
            /*
             * 如果 x 大于当前节点的值，说明待删除节点可能在右树，不可能在左树
             * 右递归删除
             */
            t.right = remove(x, t.right);
        } else if (t.left != null && t.right != null) {
            /*
             * x等于当前节点的值，并且当前节点有左右子树
             * 需要使用特殊的removeMin方法，进行树的旋转
             * 详见removeMin方法
             */
//            t.val = findMin(t.right).val;
//            t.right = remove(t.val, t.right);
            removeMin(t); // 效率高一些
        } else {
            /*
             * x等于当前节点的值，但当前节点只有左子树或者右子树
             * 返回不为null的那个子树
             */
            t = (t.left != null) ? t.left : t.right;
        }

        return t;
    }
```


# removeMin() 删除值最小的节点

对 remove() 方法的补充

```java
    /**
     * 特殊的 removeMin 方法
     * 将 t 节点的值替换为右节点中最小节点的值，并删除右节点中的最小节点
     * 即删除节点 t ，并旋转树
     *
     * @param t 待删除节点
     */
    private void removeMin(BinaryNode t) {
        if (t == null) return; // 如果待删除节点为空，则直接返回。

        BinaryNode n = t.right; // 从右侧开始删除
        BinaryNode p = null; // 保存最小节点的父节点

        while (true) {
            if (n.left != null) {
                // 表示不是最小节点，继续遍历
                p = n;
                n = n.left;
            } else {
                /*
                 * 找到最小节点
                 * 此时将最小节点的值给予待删除节点（树的旋转）
                 * 最小节点的父节点的左节点指向最小节点的右节点（可能为 null）
                 */
                t.val = n.val;
                p.left = n.right;
                break;
            }
        }
    }
```

# 测试类
> 演示代码

```java
public class TestBinarySearchTree {
    public static void main(String[] args) {
        BinarySearchTree bst = new BinarySearchTree();
        bst.insert(6);
        bst.insert(2);
        bst.insert(8);
        bst.insert(1);
        bst.insert(5);
        bst.insert(3);
        bst.insert(4);

        System.out.println("删除前");
        System.out.println(bst.contains(2));
        bst.printTree(1);

        bst.remove(2);

        System.out.println("删除后");
        System.out.println(bst.contains(2));
        bst.printTree(1);

    }
}
```