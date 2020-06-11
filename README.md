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
