---
title: 剑指offer 30：最小K个数
layout: post
categories: 刷题
tags: 快排
---
基于快速排序中的Partition函数来解决这个问题。如果基于数组的第k个数字来调整，使得比第k个数字小的所有数字都位于数组的左边，比第k个数字大的所有数字都位于数组的右边。这样调整之后，位于数组中左边的k个数字就是最小的k个数字（这k个数字不一定是排序的）。

　　But，采用这种思路是有限制的。我们需要修改输入的数组，因为函数Partition会调整数组中数字的顺序。

    

    import java.util.ArrayList;
     public class Solution {
    public ArrayList<Integer> GetLeastNumbers_Solution(int [] input, int k) 
        ArrayList<Integer> list = new ArrayList<Integer>();
         if(input == null || input.length == 0 || k <= 0 || k > input.length)
             return list;
         }
         int start = 0;
         int end = input.length - 1;
         int index = partition(input, start, end);
         while(index != k-1){
             if(index > k-1){
                 end = index - 1;
                 index = partition(input, start, end);
             }
             else{
                 start = index + 1;
                 index = partition(input, start, end);
             }
         }
         for(int i = 0; i < k; i++){
             list.add(input[i]);
         }
         return list;
    }
     
    public int partition(int[] input, int low, int high){
         if(input == null || input.length == 0 || low < 0 || high >= input.length){
             return -1;
         }
         int key = input[low];
         while(low < high){
             while(input[high] >= key && low < high){
                 high --;
             }
             if(input[high] < key){
                 int temp = input[high];
                 input[high] = input[low];
                 input[low] = temp;
             }
             while(input[low] <= key && low < high){
                 low ++;
             }
             if(input[low] > key){
                 int temp = input[low];
                 input[low] = input[high];
                 input[high] = temp;
             }
         }
         return high;
     }
}
可以先创建一个大小为k的数据容器来存储最小的k个数字，从输入的n个整数中一个一个读入放入该容器中，如果容器中的数字少于k个，按题目要求直接返回空；
如果容器中已有k个数字，而数组中还有值未加入，此时就不能直接插入了，而需要替换容器中的值。按以下步骤进行插入： 
先找到容器中的最大值；
将待查入值和最大值比较，如果待查入值大于容器中的最大值，则直接舍弃这个待查入值即可；如果待查入值小于容器中的最小值，则用这个待查入值替换掉容器中的最大值；
重复上述步骤，容器中最后就是整个数组的最小k个数字。
对于这个容器的实现，我们可以使用最大堆的数据结构，最大堆中，根节点的值大于它的子树中的任意节点值。Java中的TreeSet类实现了红黑树的功能，它底层是通过TreeMap实现的，TreeSet中的数据会按照插入数据自动升序排列（按自然顺序）。因此我们直接将数据依次放入到TreeSet中，数组就会自动排序。


    public static ArrayList<Integer> GetLeastNumbers_Solution3(int [] input, int k) {
        if(input == null)
            return null;
        ArrayList<Integer> list = new ArrayList<Integer>(k);
        if(k > input.length)
            return list;
        TreeSet<Integer> tree = new TreeSet<Integer>();
        for(int i = 0 ; i < input.length; i++){
            tree.add(input[i]);
        }
        int i = 0;
        for(Integer elem : tree){
            if(i >= k)
                break;
            list.add(elem);
            i++;
        }
        return list;
    }