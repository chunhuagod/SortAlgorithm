# SortAlgorithm

## 简介

实现基于vector的常用比较排序算法，包含归并排序（MergeSort）、堆排序（HeapSort）、快速排序（QuickSort）实现以及原理复杂度分析，参考资料为算法导论。

## 比较、交换函数

```c++
template<typename T>
bool morethan(const T& a,const T& b){
    return a>b;
}
template<typename T>
bool lessthan(const T& a,const T& b){
    return a<b;
}
template<typename T>
void swap(T& a,T& b){
    T temp=a;
    a=b;
    b=temp;
}
```



## 插入排序

### 原理

### 代码实现

```c++
template<typename T>
void InsertionSort(vector<T> & res,bool IsAscend=true){
    bool (*CompareFunc)(const T&,const T&);
    if(IsAscend) CompareFunc=lessthan;
    else CompareFunc=morethan;
    int length=res.size();
    if(length==1) return;
    for(int index{1};index<length;++index){
        int temp_index=index;
        for(int j {index-1};j>=0;--j){
            if (CompareFunc(res[temp_index],res[j])){
                swap(res[j],res[temp_index]);
                temp_index=j;}
            else break;
        }
    }
}
```



### 复杂度分析

## 归并排序

### 原理

### 代码实现

```c++
template<typename T>
void Merge(vector<T>& vec,unsigned StartIndex,unsigned EndIndex,bool (*CompareFunc)(const T&,const T&)){
    unsigned MidIndex=(StartIndex+EndIndex)>>1;
    int LeftIndex=StartIndex,RightIndex=MidIndex+1;
    vector<T> res;

    while(LeftIndex<=MidIndex && RightIndex<=EndIndex){
        if (CompareFunc(vec[LeftIndex],vec[RightIndex])) {res.push_back(vec[LeftIndex]);++LeftIndex;}
        else {res.push_back(vec[RightIndex]);++RightIndex;}
    }
    while(LeftIndex<=MidIndex) {res.push_back(vec[LeftIndex]);++LeftIndex;}
    while(RightIndex<=EndIndex) {res.push_back(vec[RightIndex]);++RightIndex;}
    for(auto i{StartIndex};i<=EndIndex;++i){
        vec[i]=res[i-StartIndex];
    }
}
template<typename T>
void MergeSort(vector<T> &vec,unsigned StartIndex,unsigned EndIndex,bool IsAscend){
    bool (*CompareFunc)(const T&,const T&);
    if(IsAscend) CompareFunc=lessthan;
    else CompareFunc=morethan;
    if(StartIndex<EndIndex){
        auto MidIndex=(StartIndex+EndIndex)>>1;
        MergeSort(vec,StartIndex,MidIndex,IsAscend);
        MergeSort(vec,MidIndex+1,EndIndex,IsAscend);
        Merge(vec,StartIndex,EndIndex,CompareFunc);
    }
    return;
}
template<typename T>
void MergeSort(vector<T> &vec,bool IsAscend=true){
    MergeSort(vec,0,vec.size()-1,IsAscend);
}
```



### 复杂度分析

## 堆排序

### 原理

### 实现代码

```c++
inline unsigned Parent(unsigned index){
    if (index==0) return 0;
    return (index-1)>>1;
}
inline unsigned LeftChild(unsigned index){
    return (index<<1)+1;
}
inline unsigned RightChild(unsigned index){
    return (index+1)<<1;
}
template<typename T>
void Heapify(vector<T> & vec,unsigned index,bool IsMaxHeap=true){
    int length=vec.size();
    if(index>=length) return;
    unsigned leftchild=LeftChild(index);
    unsigned rightchild=RightChild(index);
    bool (*CompareFunc)(const T&,const T&);
    if(IsMaxHeap) CompareFunc=morethan;
    else CompareFunc=lessthan;

    unsigned TargetIndex=index;
    if(leftchild<length && CompareFunc(vec[leftchild],vec[TargetIndex])) TargetIndex=leftchild;
    if(rightchild<length && CompareFunc(vec[rightchild],vec[TargetIndex])) TargetIndex=rightchild;
    if (TargetIndex!=index){
        swap(vec[index],vec[TargetIndex]);
        Heapify(vec,TargetIndex,IsMaxHeap);}

}
template<typename T>
void BuildHeap(vector<T> & vec,bool IsMaxHeap=true){
    auto length=vec.size();
    if(length==0 || length==1) return;
    for(int index=Parent(length-1);index>=0;--index){
        Heapify(vec,index,IsMaxHeap);
    }
}
template<typename T>
void HeapSort(vector<T> &vec,bool IsAscend=true){
    vector<T> res;
    auto length=vec.size();
    BuildHeap(vec,IsAscend);
    for(int i=0;i<length;++i){
        swap(vec[0],vec[vec.size()-1]);
        res.push_back(vec.back());
        vec.pop_back();
        Heapify(vec,0,IsAscend);
    }
    vec=res;
}
```



### 复杂度分析

## 快速排序

### 原理

### 代码实现

```c++
template<typename T>
int Partition(vector<T> &vec,int StartIndex,int EndIndex,bool (*CompareFunc)(const T&,const T&)){
    T& BaseValue=vec[EndIndex];
    int TargetIndex=StartIndex-1;
    for(int j{StartIndex};j<EndIndex;++j){
        if(CompareFunc(vec[j],BaseValue)){
            ++TargetIndex;
            swap(vec[TargetIndex],vec[j]);
        }
    }
    ++TargetIndex;
    swap(vec[TargetIndex],vec[EndIndex]);
    return TargetIndex;
}
template<typename T>
void QuickSort(vector<T> &vec,int StartIndex,int EndIndex,bool IsAscend=true){
    bool (*CompareFunc)(const T&,const T&);
    if(IsAscend) CompareFunc=lessthan;
    else CompareFunc=morethan;
    if(StartIndex<EndIndex){
        auto index=Partition(vec,StartIndex,EndIndex,CompareFunc);
        QuickSort(vec,StartIndex,index-1,IsAscend);
        QuickSort(vec,index+1,EndIndex,IsAscend);
    }
    else return;
}
template<typename T>
void QuickSort(vector<T> &vec,bool IsAscend=true){
    if(vec.size()==0 || vec.size()==1) return;
    QuickSort(vec,0,vec.size()-1,IsAscend);
}
```



### 复杂度分析