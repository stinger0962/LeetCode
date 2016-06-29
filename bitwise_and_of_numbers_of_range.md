# Bitwise AND of Numbers of Range

 
    Given a range [m, n] where 0 <= m <= n <= 2147483647, return the bitwise AND of all numbers in this range, inclusive.
    

    For example, given the range [5, 7], you should return 4.





---


  This is a brain storming problem rather than a data structure one after we figure out what it asks for.
  
   The easiest way to understand the problem is through an example.  Let's say we have [8,11], a total of 4 numbers.

   * 1000   8
   * 1001   9
   * 1010  10
   * 1011  11
 
Applying bitwise-AND on the numbers, we get 1000. 

Let's analyze the result bit by bit from the right most.

(After some inspiration) It comes to true that **the right most bit** is always 0 if m is not equal to n.

  If the right most bit of m is 1, then that of m+1 is 0. 
  
  If the right most bit of m is 0, then that of m+1 is 1.
  
  Either way, the right most bit is already determined to be 0.
  
  After dealing with the right most bit, we use right shifting ```m>>1``` to deal with next bit. 
  
  We see that since ```m>>1``` is not equal to ```n>>1```, the right most bit is 0 again.
  
  This process repeats until ```m>>k``` is finally equal to ```n>>k```, at which points, the algorithm is finished. The rest (left) bits are exactly the same as those of ```m>>k``` (or ```n>>k```). 
  
  Why? Because if ```m>>k``` and ```n>>k``` are same, then any number q of which ```q>>k``` is different to ```m>>k``` will jump out of the range of [m, n]. (Think of the above example again.)
  

---


  
  The implementation is quite simple.
  
  
  ```
  class Solution {
public:
    int rangeBitwiseAnd(int m, int n) 
    {
        int bit = 0;
        while(m != n)
        {
            m = m >> 1;
            n = n >> 1;
            ++bit;
        }
        return m<<bit; // return m>>bit + 000... (bit X 0)
    }
};```
  





  
    

