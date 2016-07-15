# Count Primes

Count the number of prime numbers less than a non-negative number, n.



---


Simple and familiar(?). Have seen it somewhere before?

**Don't confuse** it with determining if a number is a prime. The problem will become tough if we approach in the same way we determine if a number is prime or not.

In fact, we need to solve the problem **in the opposite way**.

Still we need some inspiration.

**Assume all the odd numbers are prime. With an efficient way, we count the primes, meanwhile cross out those not qualified. **



---


```
class Solution {
public:
    int countPrimes(int n) 
    {
        if(n==0 || n == 1 || n == 2)
        {
            return 0;
        }
        // concise explanation: 
        // first, we mark all numbers are prime.
        // then, we loop through all the odd numbers, which are also possible prime numbers.
        // in each loop, we do two things.
        //   first, check if an odd is marked as prime, if so then increment the count by 1
        //   then unmark all the odds that are multiply of i, but starting from i*i, not i. 
        //   (b/c i*k, where k < i, is checked in loop of k)
        

        vector<bool> notPrime(n, false); // the vector is used to store info if a number is prime.  
        
        int count = 1; // start from 1 to include 2, which is a prime number
        for(int i = 3; i < n; i+=2)
        {
            if(!notPrime[i])
            {
                ++count;
            }
            if(i > sqrt(n))
            {
                continue;
            }
            for(int j = i*i; j<n; j+=i)
            {
                notPrime[j] = true;
            }
        }
        return count;
    }
};```



