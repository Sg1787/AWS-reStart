# Challenge Lab: Prime Number Generator in Python  
> _“In the grim darkness of unoptimized loops… only primes remain pure.”_

##  The Task  
Write a Python script that:  
- Finds **all prime numbers between 1 and 250**  
- Saves them to a file called `results.txt`  
- Runs cleanly on an **Amazon EC2 Linux instance**  

Simple? Yes.  
Foundational? Absolutely.  
A rite of passage for every cloud engineer who’s ever debugged a script at 2 a.m.? **Without a doubt.**

> 💀 *Nurgle tried to inject composite numbers into my list. My `is_prime()` function banished them with square-root efficiency.*

---

##  The Script (`prime_finder.py`)

```python
def is_prime(n):
    if n < 2:
        return False
    if n == 2:
        return True
    if n % 2 == 0:
        return False
    for i in range(3, int(n**0.5) + 1, 2):
        if n % i == 0:
            return False
    return True

# Generate primes from 1 to 250
primes = [str(n) for n in range(1, 251) if is_prime(n)]

# Write to file
with open('results.txt', 'w') as f:
    f.write('\n'.join(primes))

print("Prime numbers between 1 and 250 written to results.txt")

