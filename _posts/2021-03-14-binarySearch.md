---
title : "Binary Search"

categories :
    - algorithm

tags :
    - binary search, contains, first, last, leastGreater, greatestLesser

---

## Binary Search

```c
int binarySearch(int arr[], int l, int r, int x){
    while(l <= r){
        int m = l + (r - l)/2;
        if(arr[m] < x) l = m + 1;
        else if(arr[m] > x) r = m - 1;
        else return m;
    }
    return -1;
}
```

## Contains

```c
bool contains(int arr[], int l, int r, int x){
    while(l <= r){
        m = l + (r - l)/2;
        if(arr[m] == x) return True;
        else if(arr[m] < x) l = m + 1;
        else r = m - 1;
    }
    return False;
}
```

## First

```c
int first(int arr[], int l, int r, int x){
    int ans = -1;
    while(l <= r){
        int m = l + (r - l)/2;
        if(arr[m] < x) l = m + 1;
        else if(arr[m] > x) r = m - 1;
        else if(arr[m] == x){
            ans = m;
            r = m - 1;
        }
    }
    return ans;
}
```

## Last

```c
int last(int arr[], int l, int r, int x){
    int ans = -1;
    while(l <= r){
        int m = l + (r - l)/2;
        if(arr[m] < x) l = m + 1;
        else if(arr[m] > x) r = m - 1;
        else if(arr[m] == x){
            ans = m;
            l = m + 1;
        }
    }
    return ans;
}
```

## leastGreater

```c
int leastGreater(int arr[], int l, int r, int x){
    int ans = -1;
    while(l <= r){
        int m = l + (r - l)/2;
        if(arr[m] <= x) l = m + 1;
        else if(arr[m] > x){
            ans = m;
            r = m - 1;
        }
    }
    return ans;
}
```

## greatestLesser

```c
int greatestLesser(int arr[], int l, int r, int x){
    int ans = -1;
    while(l<=r){
        int m = l + (r - l)/2;
        if(arr[m] < x){
            ans = m;
            l = m - 1;
        }
        else if(arr[m] >= x) r = m - 1;
    }
}
```