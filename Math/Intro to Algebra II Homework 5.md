> **Let $K$ be a finite field. Show that every algebraic closure $\bar{K}$ of $K$ is Galois over $K$.**

To prove that the algebraic closure $\bar{K}$ is Galois over a finite field $K$, we must demonstrate that the field extension $\bar{K}/K$ satisfies two conditions: it must be both **separable** and **normal**.

Here is the step-by-step proof.

### 1. Proof of Separability

An algebraic extension is separable if the minimal polynomial of every element in the extension is a separable polynomial (i.e., it has distinct roots in an algebraic closure).

- **Finite fields are perfect:** A field is called perfect if every algebraic extension of it is separable. It is a standard result in abstract algebra that all finite fields are perfect. This is because, for a finite field of characteristic $p$, the Frobenius endomorphism ($x \mapsto x^p$) is an automorphism, meaning every element has a $p$-th root. See [[perfect.pdf]] for details
    
- **Application to $\bar{K}$:** By definition, the algebraic closure $\bar{K}$ is an algebraic extension of $K$. Since $K$ is a finite field (and therefore perfect), every algebraic extension of $K$ is automatically separable.
    
- **Conclusion:** The extension $\bar{K}/K$ is separable.
    

### 2. Proof of Normality

An algebraic extension $L/K$ is normal if every irreducible polynomial in $K[x]$ that has at least one root in $L$ splits completely into linear factors in $L[x]$.

- Let $f(x) \in K[x]$ be an arbitrary irreducible polynomial, and suppose it has a root $\alpha \in \bar{K}$.
    
- Because $\alpha$ is algebraic over $K$, the simple extension $K(\alpha)$ is a finite extension of $K$. Let the degree of this extension be $n = [K(\alpha) : K]$, and let $q = |K|$.
    
- The field $K(\alpha)$ is a finite field with $q^n$ elements. A fundamental property of finite fields is that any finite field of order $q^n$ is the splitting field of the polynomial $x^{q^n} - x$ over its base field.
    
- Because $K(\alpha)$ is a splitting field of a polynomial over $K$, the extension $K(\alpha)/K$ is a normal extension.
    
- By the definition of normality for the finite extension $K(\alpha)/K$, the minimal polynomial of $\alpha$ over $K$—which is exactly $f(x)$ multiplied by some constant—must split completely into linear factors within $K(\alpha)[x]$.
    
- Since $K(\alpha)$ is a subfield of $\bar{K}$ (i.e., $K(\alpha) \subseteq \bar{K}$), the polynomial $f(x)$ splits completely in $\bar{K}[x]$ as well.
    
- **Conclusion:** Since every irreducible polynomial over $K$ that has a root in $\bar{K}$ splits completely in $\bar{K}$, the extension $\bar{K}/K$ is normal.
    

### Final Conclusion

Because the algebraic extension $\bar{K}/K$ is both **separable** and **normal**, it is by definition a **Galois extension**.

---

> Let $K$ be a field of characteristic $p > 0$. For an element $a\in K\backslash K^p$ , i.e., $a$ is not a $p$-th power in $K$, find all integers $n$ such that $x^{p^n}−a \in K[x]$ is irreducible.

The polynomial $x^{p^n} - a$ is irreducible in $K[x]$ for **all non-negative integers $n \ge 0$**.

Here is the rigorous proof.

### Proof

For $n = 0$, the polynomial is $x - a$, which is linear and therefore trivially irreducible. We will now prove it holds for any integer $n \ge 1$.

Let $\alpha$ be a root of $f(x) = x^{p^n} - a$ in an algebraic closure $\bar{K}$ of $K$.

Because $K$ has characteristic $p$, the binomial expansion simplifies significantly. We have $\alpha^{p^n} = a$, which allows us to rewrite the polynomial entirely in terms of $\alpha$:

$$f(x) = x^{p^n} - a = x^{p^n} - \alpha^{p^n} = (x - \alpha)^{p^n}$$

Suppose, for the sake of contradiction, that $f(x)$ is reducible over $K$. This means there exists a proper monic polynomial $g(x) \in K[x]$ that divides $f(x)$, with a degree $d$ such that $1 \le d < p^n$.

Since $g(x)$ divides $(x - \alpha)^{p^n}$ in $\bar{K}[x]$, unique factorization dictates that $g(x)$ must be of the form:

$$g(x) = (x - \alpha)^d$$

We can factor the highest power of $p$ out of the degree $d$. Let $d = p^k \cdot m$, where $m$ is an integer coprime to $p$ ($\gcd(m, p) = 1$) and $k$ is a non-negative integer. Because $d < p^n$, it is guaranteed that $0 \le k < n$.

Using the properties of characteristic $p$, we can rewrite $g(x)$:

$$g(x) = (x - \alpha)^{p^k m} = ((x - \alpha)^{p^k})^m = (x^{p^k} - \alpha^{p^k})^m$$

Now, expand this binomial to look at its coefficients:

$$(x^{p^k} - \alpha^{p^k})^m = x^{p^k m} - m \alpha^{p^k} x^{p^k(m-1)} + \dots$$

Since we assumed $g(x) \in K[x]$, every coefficient of $g(x)$ must belong to the field $K$. Therefore, the coefficient of the second-highest degree term must be in $K$:

$$-m \alpha^{p^k} \in K$$

Because $\gcd(m, p) = 1$, the integer $m$ is not zero in the field $K$. We can safely divide by $-m$ within $K$, yielding:

$$\alpha^{p^k} \in K$$

Since $k < n$, we know that $n - k \ge 1$, which means $n - k - 1 \ge 0$. Because fields are closed under multiplication, we can raise $\alpha^{p^k}$ to the power of $p^{n-k-1}$ and the result will still be in $K$. Let's call this element $c$:

$$c = (\alpha^{p^k})^{p^{n-k-1}} = \alpha^{p^{n-1}} \in K$$

Finally, observe what happens when we take the $p$-th power of $c$:

$$c^p = (\alpha^{p^{n-1}})^p = \alpha^{p^n} = a$$

This shows that $a = c^p$ for some $c \in K$, which means $a$ is a $p$-th power in $K$ (i.e., $a \in K^p$).

This directly contradicts the initial premise that $a \in K \setminus K^p$. Thus, our assumption that $f(x)$ is reducible must be false.

**Conclusion:** The polynomial $x^{p^n} - a$ is irreducible over $K[x]$ for all integers $n \ge 0$.
