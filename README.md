# Dynamic-Programming
Dynamic Programming is mainly an optimization over plain recursion. Wherever we see a recursive solution that has repeated calls for same inputs, we can optimize it using Dynamic Programming. This repository contains all my practice codes on dynamic programming along with the recursive implementations. 

### The Staircase Problem

<b>Problem Statement :</b> A child is running up a staircase with N steps, and can hop either 1 step, 2 steps or 3 steps at a time. Implement a method to count how many possible ways the child can run up to the stairs. You need to return number of possible ways W.

<b>The Recursive Approach:</b>

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
    
<b>The Dynamic Programming Approach:</b>

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

### The Staircase Problem

<b>Problem Statement :</b> Given a positive integer n, find the minimum number of steps s, that takes n to 1. You can perform any one of the following 3 steps.

<b>The Recursive Approach:</b>

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

<b>The Dynamic Programming Approach:</b>

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

### The Largest Common Substring Problem

<b>Problem Statement :</b>Given 2 strings of S1 and S2 with lengths m and n respectively, find the length of longest common subsequence.
A subsequence of a string S whose length is n, is a string containing characters in same relative order as they are present in S, but not necessarily contiguous. Subsequences contain all the strings of length varying from 0 to n. E.g. subsequences of string "abc" are - "",a,b,c,ab,bc,ac,abc.

<b>The Recursive Approach:</b>

*Algorithm:*
 * Check if the last character is same, if yes then call recursively in the rest of the string excluding the last character. If No, proceed to the next step.
 * Return the maximum of the two subproblems, one of which includes last character of the first string and excludes the last character second one, another one is just the opposite of the first one.
    For Example - "AXYB" and "AXBY" has last character not equal. Hence we find the **max(LCS("AXY","AXBY"),LCS("AXYB","AXB"))** , where LCS is the recursive function for Lowest Common Substring.
    
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
    
<b>The Dynamic Approach:</b>

Tabulation is done using a two dimensional array in this case. Whose dimensions are same as that of the length of the strings.

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





