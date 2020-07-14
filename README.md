# Dynamic-Programming
Dynamic Programming is mainly an optimization over plain recursion. Wherever we see a recursive solution that has repeated calls for same inputs, we can optimize it using Dynamic Programming. This repository contains all my practice codes on dynamic programming along with the recursive implementations. 

### 🏟 The Staircase Problem 

<b>Problem Statement :</b> A child is running up a staircase with N steps, and can hop either 1 step, 2 steps or 3 steps at a time. Implement a method to count how many possible ways the child can run up to the stairs. You need to return number of possible ways W.

<b>The Recursive Approach:</b>

```cpp
class recursive_approach
{
public:
    int get_ways(int n)
    {
        if(n==0 || n==1)
        {
            return 1;
        }
        else if(n==2)
        {
            return 2;
        }
        else
        {
            return get_ways(n-3)+get_ways(n-2)+get_ways(n-1);
        }
    }
};
```
    
<b>The Dynamic Programming Approach:</b>

```cpp
class dynamic_approach
{
public:
    int get_ways(int n)
    {
        int* array = new int[n+1];
        for(int i=0;i<=n;i++)
        {
            array[i] = 0;
        }
        array[1] = 1;
        array[2] = 2;
        array[3] = 4;
        for(int i=4;i<=n;i++)
        {
            for(int j=i-3;j<i;j++)
            {
                array[i]+=array[j];
            }
        }
        return array[n];
    }
};
```

### 🚶‍ The Minimum Step Problem 

<b>Problem Statement :</b> Given a positive integer n, find the minimum number of steps s, that takes n to 1. You can perform any one of the following 3 steps.

<b>The Recursive Approach:</b>

```cpp
class recursive_approach
{
public:
    int countStepsTo1(int n)
    {
        int small_ans;
        if(n==1)
        {
            return 0;
        }
        small_ans = countStepsTo1(n-1);
        if(n%3==0)
        {
            n = n/3;
            small_ans = min(small_ans,countStepsTo1(n));
        }
        else if(n%2==0)
        {
            n = n/2;
            small_ans = min(small_ans,countStepsTo1(n));
        }
        return small_ans+1;
    }
};
```

<b>The Dynamic Programming Approach:</b>

```cpp
class dynamic_approach
{
public:
    int countStepsTo1(int n)
    {
        int* array = new int[n+1];
        for(int i=0;i<=n;i++)
        {
            array[i] = 0;
        }
        array[1] = 0;
        array[2] = 1;
        array[3] = 1;
        for(int i=4;i<=n;i++)
        {
            if(i%3==0)
            {
                array[i] = min(array[i/3],array[i-1])+1;
            }
            else if(i%2==0)
            {
                array[i] = min(array[i/2],array[i-1])+1;
            }
            else
            {
                array[i] = array[i-1]+1;
            }
        }
        return array[n];
    }
};
```

### 🔠 The Largest Common Substring Problem 

<b>Problem Statement :</b>Given 2 strings of S1 and S2 with lengths m and n respectively, find the length of longest common subsequence.
A subsequence of a string S whose length is n, is a string containing characters in same relative order as they are present in S, but not necessarily contiguous. Subsequences contain all the strings of length varying from 0 to n. E.g. subsequences of string "abc" are - "",a,b,c,ab,bc,ac,abc.

<b>The Recursive Approach:</b>

*Algorithm:*
 * Check if the last character is same, if yes then call recursively in the rest of the string excluding the last character. If No, proceed to the next step.
 * Return the maximum of the two subproblems, one of which includes last character of the first string and excludes the last character second one, another one is just the opposite of the first one.
    For Example - "AXYB" and "AXBY" has last character not equal. Hence we find the **max(LCS("AXY","AXBY"),LCS("AXYB","AXB"))** , where LCS is the recursive function for Lowest Common Substring.
    
```cpp
class recursive_approach
{
public:
    int rec_helper(string first, string second, int m, int n)
    {
        if (m == 0 || n == 0)
        {
            return 0;
        }
        if (first[m-1] == second[n-1])
        {
            return 1 + rec_helper(first, second, m-1, n-1);
        }
        else
        {
            return max(rec_helper(first, second, m, n-1), rec_helper(first, second, m-1, n));
        }
        return 0;
    }
    int largest_common_substring(string first, string second)
    {
        int m = first.size();
        int n = second.size();
        return rec_helper(first,second,m,n);
    }
};    
```

<b>The Dynamic Approach:</b>

Tabulation is done using a two dimensional array in this case. Whose dimensions are same as that of the length of the strings + 1 .

```cpp
class dynamic_approach
{
public:
    int largest_common_substring(string first, string second)
    {
        int m = first.size();
        int n = second.size();
        int table[m+1][n+1];
        for(int i=0;i<=m;i++)
        {
            for(int j=0;j<=n;j++)
            {
                if(i==0||j==0)
                {
                    table[i][j] = 0;
                }
                else if(first[i-1]==second[j-1])
                {
                    table[i][j] = table[i-1][j-1]+1;
                }
                else
                {
                    table[i][j] = max(table[i - 1][j], table[i][j - 1]);
                }
            }
        }
        return table[m][n];
    }
};
```

### 🚗 The edit Distance Problem 

<b>Problem Statement :</b> Given two strings s and t of lengths m and n respectively, find the Edit Distance between the strings. Edit Distance of two strings is minimum number of steps required to make one string equal to other. In order to do so you can perform following three operations only :
1. Delete a character
2. Replace a character with another one
3. Insert a character

<b>The Recursive Approach:</b>

*Algorithm:*

* Base case - If one string is empty, return the length of the other
* If the last character is same, call recursively in the rest of the strings excluding the last character.
* If the last character is not same, call recursively to three different subproblems namely insert, remove, replace:
    Considering m and n as the length of the S1 and S2 string respectively,
    * Insert - Insert the last character of S2 in S1, now last character equal and S1 is of length m+1, hence we will call the recursive function on the rest of the string excluding the last character of **size (m,n-1)**.
    * Remove - Removing the last character of S1, we have the string S1 and S2 of length m-1 and n respectively. Hence a recursive call will be made on the **size (m-1,n)**.
    * Replace - Replacing the last character of the strings will make the last character same, hence a recursive call will be made on the **size (m-1,n-1)**.
    
        ```cpp
        class recursive_approach
        {
        public:
            int rec_helper(string first, string second, int m, int n)
            {
                if(m==0)
                {
                    return n;
                }
                if(n==0)
                {
                    return m;
                }
                if(first[m-1]==second[n-1])
                {
                    return rec_helper(first,second,m-1,n-1);
                }

                return 1 + min(rec_helper(first,second,m,n-1),
                               rec_helper(first,second,m-1,n),
                               rec_helper(first,second,m-1,n-1));
            }
            int editDistance(string first, string second)
            {
                int m = first.size();
                int n = second.size();

                return rec_helper(first,second,m,n);
            }
        };
        ```

<b>The Dynamic Approach:</b>
Tabulation is done using a two dimensional array in this case. Whose dimensions are same as that of the length of the strings + 1 .

```cpp
class dynamic_approach
{
public:
    int editDistance(string first, string second)
    {
        int m = first.size();
        int n = second.size();

        int table[m+1][n+1];
        for(int i=0;i<=m;i++)
        {
            for(int j=0;j<=n;j++)
            {
                if(i==0)
                {
                    table[i][j] = j;
                }
                else if(j==0)
                {
                    table[i][j] = i;
                }
                else if(first[i-1]==second[j-1])
                {
                    table[i][j] = table[i-1][j-1];
                }
                else
                {
                    table[i][j] = 1 + min(table[i][j-1],table[i-1][j],table[i-1][j-1]);
                }
            }
        }
        return table[m][n];
    }
};
```
 
### 🎟 The Lottery Bill Problem 

<b>Problem Statement :</b> Allen has a LOT of money. He has n dollars in the bank. For security reasons, he wants to withdraw it in cash (we will not disclose the reasons here). The denominations for dollar bills are 1, 5, 10, 20, 100. What is the minimum number of bills Allen could receive after withdrawing his entire balance?

<b>The Recursive Approach:</b>

```cpp
class recursive_approach
{
public:
    ll bill_num(ll amount)
    {
        if(amount==0)
        {
            return 0;
        }
        if(amount >= 100)
        {
            amount = amount - 100;
        }
        else if(amount >= 20)
        {
            amount = amount - 20;
        }
        else if(amount >= 10)
        {
            amount = amount - 10;
        }
        else if(amount >= 5)
        {
            amount = amount - 5;
        }
        else if(amount >= 1)
        {
            amount = amount - 1;
        }
        return 1 + bill_num(amount);
    }
};
```

<b>The Dynamic Approach:</b>

```cpp
class dynamic_approach
{
public:
    ll bill_num(ll amount)
    {
        ll arr[amount+1];
        arr[0] = 0;
        for(ll i=1;i<=amount;i++)
        {
            if(i>=100)
            {
                arr[i] = i/100 + arr[i%100];
            }
            else if(i>=20)
            {
                arr[i] = i/20 + arr[i%20];
            }
            else if(i>=10)
            {
                arr[i] = i/10 + arr[i%10];
            }
            else if(i>=5)
            {
                arr[i] = i/5 + arr[i%5];
            }
            else if(i>=1)
            {
                arr[i] = i/1 + arr[i%1];
            }
        }
        return arr[amount];
    }
};
```

<b>The Optimized Dynamic Approach:</b>

```cpp
class optimized_dynamic_approach
{
public:
    ll bill_num(ll amount)
    {
        ll count = 0;
        if(amount/100)
        {
            count+=(amount/100);
            amount -= (amount/100)*100;
        }
        if(amount/20)
        {
            count+=(amount/20);
            amount -= (amount/20)*20;
        }
        if(amount/10)
        {
            count+=(amount/10);
            amount -= (amount/10)*10;
        }
        if(amount/5)
        {
            count+=(amount/5);
            amount -= (amount/5)*5;
        }
        count+=amount;
        return count;
    }
};
```
    
<b>The Python Approach</b>

```python3
amount = int(input())
count = 0

if int(amount/100):
    count+=int(amount/100)
    amount -= int(amount/100)*100
if int(amount/20):
    count+=int(amount/20)
    amount -= int(amount/20)*20
if int(amount/10):
    count+=int(amount/10)
    amount -= int(amount/10)*10
if int(amount/5):
    count+=int(amount/5)
    amount -= int(amount/5)*5
count+=amount
print(count)
```

### 📈 Maximum Increase Problem

<b>Problem Statement :</b> You are given array consisting of n integers. Your task is to find the maximum length of an increasing subarray of the given array.A subarray is the sequence of consecutive elements of the array. Subarray is called increasing if each element of this subarray strictly greater than previous.

<b>The Dynamic Approach:</b>

```cpp
typedef long long int ll;

int main()
{
    ll n;
    cin >> n;
    ll* arr = new ll[n];
    ll* arr2 = new ll[n];
    for(ll i=0;i<n;i++)
    {
        cin >> arr[i];
    }
    arr2[0] = 1;
    for(ll i=1;i<n;i++)
    {
        if(arr[i] > arr[i-1])
        {
            arr2[i] = arr2[i-1]+1;
        }
        else
        {
            arr2[i] = 1;
        }
    }
    sort(arr2,arr2+n);
    cout << arr2[n-1] << endl;
    return 0;
}
```
    
<b>The Python Approach</b>

```python3
n = int(input())
arr = list(map(int,input().split()))


arr2 = [1] * n

for i in range(1,n):
    if arr[i] > arr[i-1]:
        arr2[i] = arr2[i-1]+1
arr2.sort()
print(arr2[n-1])
```
