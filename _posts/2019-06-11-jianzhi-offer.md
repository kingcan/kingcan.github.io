---
title: 剑指offer ：39题
layout: post
categories: 刷题
tags: 二叉树
---
平衡二叉树，是一种二叉排序树，其中每个结点的左子树和右子树的高度差至多等于1。它是一种高度平衡的二叉排序树
判断一棵树是否平衡。
普通思路，求出左右子树的高度做比较。
public class Solution {
    public boolean IsBalanced_Solution(TreeNode root) {
          if(root==null)
              return true;
        int left=Depth(root.left);
        int right=Depth(root.right);
        if(left-right>1||right-left>1)
            return false;
        else
            return true;
        
    }
    public int Depth(TreeNode tree){
        if(tree==null)return 0;
        int maxLeft=Depth(tree.left);
        int maxRight=Depth(tree.right);
        return maxLeft>maxRight?maxLeft+1:maxRight+1;
    }
}
上述代码中，做了很多不必要的操作，如果左子树下面的结点早就不平衡了，我们还往下遍历搞啥？
import java.lang.Math;
public class Solution {
    public boolean IsBalanced_Solution(TreeNode root) {
          if(root==null||(root.left==null&&root.right==null))
              return true;
        int left=Depth(root.left);
        if(left==-1)return false;
        int right=Depth(root.right);
        if(right==-1)return false;
        if(left>0&&right>0)
        {
        if(left-right>1||right-left>1)
            return false;
        else
            return true;
        }
        return false;
        
    }
    public int Depth(TreeNode tree){
        if(tree==null)return 0;
        int maxLeft=Depth(tree.left);
        int maxRight=Depth(tree.right);
        return Math.abs(maxLeft-maxRight)>1?-1:Math.max(maxLeft,maxRight)+1;
        //return maxLeft>maxRight?maxLeft+1:maxRight+1;
    }
}
以下这段为网上借鉴。
public class Solution {
    public boolean IsBalanced_Solution(TreeNode root) {
      
        return hight(root) != -1;
    }
    public int hight(TreeNode root){
        if(root==null){
            return 0;
        }
        int leftHight = hight(root.left);
        if(leftHight==-1){
            return -1;
        }
        int rightHight = hight(root.right);
        if(rightHight==-1){
            return -1;
        }
        if(Math.abs(leftHight-rightHight)>1){
            return -1;
        }
        return Math.max(leftHight,rightHight)+1;
    }
}
原文：https://blog.csdn.net/wmh1152151276/article/details/88320999