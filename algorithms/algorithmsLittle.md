[TOC]

# 算法学习 练习

## 示例结构 

1. 题目

1. 说明

1. 代码

1. 用例

## 题目

### 线性表 排序

1. 输出一个数组里最小的前 K 个数

1. 代码

```

    @Test
    public void demo3() {

        ArrayList<Integer> arrayList = new ArrayList<Integer>();
        arrayList.add(5);
        arrayList.add(4);
        arrayList.add(2);
        arrayList.add(3);
        arrayList.add(8);
        arrayList.add(9);
        arrayList.add(6);
        arrayList.add(10);
        arrayList.add(1);

        getMinK(arrayList,0);

    }

	/**
     * 输出一个数组里最小的前 K 个数
     */
    public void getMinK(ArrayList<Integer> arrayList, int k) {

        int len = arrayList.size();
        int max = 0;
        int index = -1;

        if (k <= 0 || k > len) return;

        ArrayList<Integer> arr = new ArrayList<Integer>();
        for (int i = 0; i < len; i++) {
            Integer num = arrayList.get(i);
            if (i < k) {
                arr.add(num);
                if (max < num) {
                    max = num;
                    index = i;
                }
            } else {
                if (num < max) {
                    arr.set(index, num);
                    index = getMax(arr);
                    max = arr.get(index);
                }
            }
        }

        for (Integer n : arr) {
            System.out.println(n);
        }
    }

    // 打到一个数组最大值的下标

    public int getMax(ArrayList<Integer> arr) {

        int len = arr.size();
        int max = 0;
        int index = 0;
        for (int i = 0; i < len; i++) {
            int num = arr.get(i);
            if (max < num) {
                max = num;
                index = i;
            }
        }
        return index;
    }


```
