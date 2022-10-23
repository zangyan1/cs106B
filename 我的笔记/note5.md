# Part 1

无代码，只需要对下述五类函数，进行复杂度分析。

![](D:\desktop\cs106B\cs106B\我的笔记\cs106_pic\5_1.png)

## 1. Printing Chip

要注意，其中printH和printC虽然看起来很相似，但是二者if语句不同导致了复杂度不同。

![](D:\desktop\cs106B\cs106B\我的笔记\cs106_pic\5_2.png)

## 2.Counting Triples  (Printing Cycles)

这2部分主要是需要参考文档，[Stanford C++ Library Documentation](https://web.stanford.edu/dept/cs_edu/resources/cslib_docs/)

## 3.Recursive Mysteries

略

## 4.Maximum single-sell Profit

其中，`maximumSingleSellProfit_v2`这是归并算法，所以复杂度应该是O(nlogn)，但是从图表来看，好像复杂度是O(n)，

这个地方我不太确定。

![1666355488215](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1666355488215.png)

# Part 2，Part 3

这两部分依旧没有代码，part2 是估计`fastMaxWeightMatching` 函数的时间复杂度，part3是`ethics problems`,一个关于工程效率和人们利益之间关系的思考题。

# Part 4



```c++
Vector<DataPoint> merge(Vector<DataPoint>&part1,Vector<DataPoint>&part2) {
    Vector<DataPoint>ans;
    int size1=part1.size();
    int size2=part2.size();
    for(int j=0,k=0;(j<size1||k<size2);){
        if((j<size1&&k<size2&&part1[j].weight<=part2[k].weight)||(j<size1&&k>=size2)){
            ans+=part1[j];j++;
        }
        if((k<size2&&j<size1&&part1[j].weight>part2[k].weight)||(k<size2&&j>=size1)){
            ans+=part2[k];k++;
        }
    }
    return ans;
}
Vector<DataPoint>mergeSort(Vector<DataPoint>&ans){
    if(ans.size()==1){
        return ans;
    }
    int len=ans.size();
    Vector<DataPoint>part1;
    Vector<DataPoint>part2;
    part1=ans.subList(0,len/2);
    part2=ans.subList(len/2,len-len/2);
    auto ans1=mergeSort(part1);
    auto ans2=mergeSort(part2);
    return merge(ans1,ans2);
}
Vector<DataPoint> combine(const Vector<Vector<DataPoint>>& sequences) {
    /* TODO: Delete the next few lines and implement this. */
    Vector<DataPoint>ans;
    for(int l=0;l<sequences.size();l++){
        for(DataPoint d:sequences[l]){
            auto n=d.name;auto w=d.weight;
            ans+=d;
        }
    }
    return mergeSort(ans);
}
```



但是发现数据量一大，就运行不通过

![](D:\desktop\cs106B\cs106B\我的笔记\cs106_pic\A5.png)



```


Vector<DataPoint> merge(Vector<DataPoint>part1,Vector<DataPoint>part2) {
    Vector<DataPoint>ans;
    int size1=part1.size();
    int size2=part2.size();
    for(int j=0,k=0;(j<size1||k<size2);){
        if((j<size1&&k<size2&&part1[j].weight<=part2[k].weight)||(j<size1&&k>=size2)){
            ans+=part1[j];j++;
        }
        if((k<size2&&j<size1&&part1[j].weight>part2[k].weight)||(k<size2&&j>=size1)){
            ans+=part2[k];k++;
        }
    }
    return ans;
}

Vector<DataPoint> combine(const Vector<Vector<DataPoint>>& sequences) {
    /* TODO: Delete the next few lines and implement this. */
     if(sequences.size()==0){
        return {};
    }
    if(sequences.size()==1){  //说明只有一个序列，直接返回就行
        return sequences[0];
    }
    if(sequences.size()==2){
        return merge(sequences[0],sequences[1]);
    }
    else{
        int size=sequences.size();
        auto part1=sequences.subList(0,size/2);
        auto part2=sequences.subList(size/2,size-size/2);
        auto ans1=combine(part1);
        auto ans2=combine(part2);
        return merge(ans1,ans2);
    }

}
```

