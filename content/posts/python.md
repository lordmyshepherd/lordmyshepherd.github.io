---
title: "python algorithm easy"
date: "2020-01-18T22:44:32.169Z"
template: "post"
draft: false
slug: "/posts/python"
category: ""
tags:
description: "#print_function_end_keyword"
socialImage: "/media/42-line-bible.jpg"
---
*The Lord is my shepherd, I lack nothing. PSLAM 23:1*

### Print Function end keyword
Read an integer N.  
Without using any string methods, try to print the following:
1,2,3 ... N   
Note that "" represents the values in between.  

Input Format  
The first line contains an integer N.  

Output Format  
Output the answer as explained in the task.  

Sample Input
```python
3
```
Sample Output   
```python
123
```

**Solution**  
The end key of print function will set the string that needs to be appended when printing is done.  

By default the end key is set by newline character. So after finishing printing all the variables, a newline character is appended. Hence, we get the output of each print statement in different line. 
```python
if __name__ == '__main__':
    n = int(input())

    arr = range(1,n+1)
    for i in arr :
        print(i,end="")
```