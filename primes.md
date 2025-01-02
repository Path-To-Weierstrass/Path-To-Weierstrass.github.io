We have all probably heard of prime number in different contexts. 
As a reminder: a number $$x \in \mathbb{N}$$ is called prime if and only if it is greater than 1 and is not divisible by any other numbers except 1 and itself. In other words, a number $$x \in \mathbb{N}$$ is prime if:

$$
x \in \mathbb{N} \text{ is prime} \iff (\forall a, b \in \mathbb{N}: (a \cdot b = x \implies a = 1 \lor b = 1)).
$$

From this definition, we can immediately see that the first few prime numbers are: 2, 3, 5, 7, 11, and so on. At first glance, this may not seem particularly interesting, but after some thought, we realize that prime numbers are the basic building blocks of our number system.

One way to think about this is as follows:

1. Every number is either prime or composite.
2. If a number $$n$$ is not prime, then there exist numbers $$a, b \in \mathbb{N}$$ such that $$a \cdot b = n$$.
3. Now, either $$a$$ is prime or not. If $$a$$ is prime, then it is one of the building blocks of $$n$$. If $$a$$ is not prime, we can decompose $$a$$ further into prime factors. We can repeat this process for $$b$$, and eventually, we obtain the prime factorization of $$n$$.

Let’s see how this works with an example. Consider the number 112:

1. $$112 = 56 \times 2$$ Since 2 is prime, we proceed with 56.
2. $$56 = 2 \times 28$$ Again, 2 is prime, so we proceed with 28.
3. $$28 = 2 \times 14$$.
4. $$14 = 2 \times 7$$ Both 2 and 7 are prime.
5. Thus, we can combine the results to get the prime factorization:
   $$
   112 = 2 \times 2 \times 2 \times 2 \times 7 = 2^4 \times 7.
   $$

The same algorithm can be written in Julia the following way: 
```
using Primes
	function prime_decomposition(n)
		result = []
		if n==1
			return [1]
		end
		if isprime(n)
			push!(result, n)
		else
			a,b = find_factors(n)
			  if a == -1 || b == -1
            	error("Unable to find factors for $n")
       		 end
			push!(result, prime_decomposition(a)...)
			push!(result, prime_decomposition(b)...)
		end
		return result
		
	end

	function find_factors(n)
		if n==1
			return 1
		end
		if iseven(n)
			return (Integer(2), Integer(n/2))
		end
		for i in 3:2:n
			if n%i==0
				return(Integer(i), Integer(n/i))
			end
		end
		return -1
	end
 ````

Now that we have established that every number is either prime or composite, the remaining question is: Is this "decomposition" unique?

The answer is yes, and we can prove it by contradiction:

1. Let $$n \in \mathbb{N}$$ be the smallest number such that its prime factorization is not unique. Assume there are two different factorizations:
   $$
   a_1 \cdot a_2 \dots a_k = n = b_1 \cdot b_2 \dots b_p.
   $$
   Without loss of generality, assume $$a_1$$ divides $$b_1 \cdot b_2 \dots b_p$$. Since all $$b_i$$ are prime, it follows that $$a_1 = b_j$$ for some $$j \in \{1, \dots, p\}$$. We can assume $$j = 1$$ without loss of generality. This means that the number $$ \frac{n}{a_1} $$ has two different prime factorizations, which contradicts the assumption that $$n$$ is the smallest such number.

Thus, the prime factorization of a number is unique.
