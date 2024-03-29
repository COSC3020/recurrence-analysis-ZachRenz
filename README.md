[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-24ddc0f5d75046c5622901739e7c5dd533143b0c8e959d652212380cedb1ea36.svg)](https://classroom.github.com/a/OlW38W4k)
# Recurrence Analysis -- Mystery Function

Analyze the running time of the following recursive procedure as a function of
$n$ and find a tight big $O$ bound on the runtime for the function. You may
assume that each operation takes unit time. You do not need to provide a formal
proof, but you should show your work: at a minimum, show the recurrence relation
you derive for the runtime of the code, and then how you solved the recurrence
relation.

```javascript
function mystery(n) {
    if(n <= 1)
        return;
    else {
        mystery(n / 3);
        var count = 0;
        mystery(n / 3);
        for(var i = 0; i < n*n; i++) {
            for(var j = 0; j < n; j++) {
                for(var k = 0; k < n*n; k++) {
                    count = count + 1;
                }
            }
        }
        mystery(n / 3);
    }
}
```

Add your answer to this markdown file. [This
page](https://docs.github.com/en/get-started/writing-on-github/working-with-advanced-formatting/writing-mathematical-expressions)
might help with the notation for mathematical expressions.

## Runtime Solution
With three for loops, two running through $n*n$ and one only running through one $n$, we have $n^5$, along with three mystery calls with an arguement of $n / 3$. 
So the recurrence relationship is $1$ if $n <= 1$ and $mystery(n-3) + n^5$ if $n>1$ and we can start solving. 

$T(n/3) = 3T(n/3/3) + (n/3)^5 \Rightarrow 3T(n/9) + n^5/3^5$ and then we can insert that into $3T(n/3) + n^5$
$3(3T(n/9) + n^5/3^5) + n^5 \Rightarrow 9T(n/9) + n^5/3^4 + n^5$ 
Then we can do another iteration and get to $27T(n/27) + n^5/3^5 + n^5/3^4 + n^5$ and we have a pattern!

$3^iT(n/3^i) + \displaystyle\sum_{j=0}^i n^5/3^{i+2} + n^5$ which is awfully familiar, I was able to find my $i$ fairly easily: $log_3n$

$nT(n/n) + \displaystyle\sum_{j=0}^{\log_{3} n} n^5/(n+2) + n^5 \Rightarrow n + \displaystyle\sum_{j=0}^{\log_{3} n} n^5/(n+2) + n^5$ and only $n^5$ will dominate.
And when we solve a recurrence relation we find $T(n) \in \Theta(n)$ where $\Theta(n^5)$, but $\Theta$ is defined as both $O$ and $\Omega$ so we can also say $T(n) \in O(n^5)$.
