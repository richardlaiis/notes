
(deg3 example)

let $\sigma(x)=\frac{1}{1-x}$
and consider the fixed field $\mathbb{C}(x)^{<\sigma>}$

$\sigma^2(x)=\frac{x-1}{x}$
$\sigma^3(x)=x$

so we have $\sigma^3(\varphi(x))=\varphi(x)$ for $\varphi(x)\in \mathbb{C}(x)$, $\sigma^3=\mathrm{id}_{\mathbb{C(x)}}$
and $<\sigma>\cong \mathbb{Z}/3\mathbb{Z}$
$[\mathbb{C}(x):\mathbb{C}(x)^{<\sigma>}]=3$

- [ ] Is it example above Galois?  seems yes?
- [ ] Is $\mathbb{C}(x)/\mathbb{C}$ a finite extension?


---

In $S_3$ example we see:
+ Fix field $\mathbb{C}(x)^G$
	+ $[\mathbb{C}(x):\mathbb{C}(x)^G]=|G|=6$
	+ $\sum_{\sigma\in G}\sigma(x)\in \mathbb{C}$ so this fails to works
+ Find $\phi(x)$ such that $\sum_{\sigma\in G}\sigma(\phi(x))\notin \mathbb{C}$, how about $\phi(x)=x^2$
+ here we try another way
+ $$\mathbb{C}(x)^G\subset \mathbb{C}(x)^{<\sigma>}=\mathbb{C}(\mu)\subset \mathbb{C}(x)$$

The definition of [Symmetric function](https://en.wikipedia.org/wiki/Symmetric_function) and [Symmetric polynomials](https://en.wikipedia.org/wiki/Symmetric_polynomial)


Our clean choices comes from "fixed parts of the transformations by $g\in G$ 
(One theorem comes to my mind is [Fixed points theorem](https://en.wikipedia.org/wiki/Fixed-point_theorem), but currently I don't know what it is and how it relate to the class, so maybe I can see it later.)

Now, I'm lost in the class, lost in the lattice of Galois groups, just recording something I may use in the internet: [Field trace](https://en.wikipedia.org/wiki/Field_trace), it seems like relating to the averaging map (I couldn't find the term qwq)


## further questions
1. what is the minimal polynomials of $x/\mathbb{C}(y),\mathbb{C}(z), \mathbb{C}(w), \mathbb{C}(v), \mathbb{C}(J)$
2. $\mathbb{C}(y,z)=\mathbb{C}(x)$ so $x=\varphi(y,z)$ where $\varphi$ is a rational function. What is $\varphi$?