# Portfolio selection, Laplace equation and Random Walk

Suppose you would like to invest 1000$ in two stocks, and you have to decide how much money you should put into these two stocks. You could use some <a href="http://www.investopedia.com/terms/m/modernportfoliotheory.asp">Modern Portfolio Theory</a> for that, for example. However, I would like to talk about a second approach, which is quite curious in my opinion.

This post is based on <a href="http://mate.dm.uba.ar/~jrossi/ToWandPDEs(2).pdf">this text</a> by Julio Rossi, from the University of Buenos Aires. In this text, the following problem is treated:
<blockquote>Suppose we have a bounded and smooth domain $latex \Omega\subset \mathbb{R}^N$, and suppose that we divide the boundary $latex \Omega$ into two disjoint parts, $latex \Gamma_1$ and $latex \Gamma_2$. Suppose that we begin at a point $latex x\in \Omega$, and suppose that we start moving randomly. That is, we start moving in any random direction, and past positions do not influence the movement. Let $latex u(x)$ be the probability of hitting $latex \Gamma_1$ first, the first time we hit the boundary $latex \partial \Omega$. What can we say about $latex u(x)$?</blockquote>
I do not want to go into details of how to calculate the value of $latex u(x)$ (if you are interested, in the <a href="http://mate.dm.uba.ar/~jrossi/ToWandPDEs(2).pdf">text by Julio Rossi</a> that I mentioned all the procedure is explained wonderfully). The thing is, the function $latex u(x)$ turns out to be the solution of the following Laplace equation:
<p style="text-align: center;">$latex \begin{cases}-\Delta u=0&amp;\mbox{in }\Omega,\\u|_{\Gamma_1}\equiv 1,\\ u|_{\Gamma_2}\equiv 0.\end{cases}$</p>
So, how could this be used in our case? Suppose that the current prices of the two stocks we want to invest in are $latex S_0^1, S_0^2$. Assume that we have $latex P_0$ dollars to invest (in our case, $latex P_0=1000$. Assume that we will sell the portfolio, if the price of either stock rises more than $latex \beta\%$, or if it decreases more than $latex \alpha\%$. We define
<p style="text-align: center;">$latex \Omega:=((1-\alpha)S_0^1, (1+\beta)S_0^1)\times ((1-\alpha)S_0^2, (1+\beta)S_0^2)$</p>
Geometrically, $latex \Omega$ would be a rectangle whose sides are parallel to the coordinate axes. Suppose that we buy $latex C^1$ shares of the first stock, and $latex C^2$ shares of the second (so that $latex C^1 S_0^1 + C^2 S_0^2=P_0$). Then, we define
<p style="text-align: center;">$latex \Gamma_1=\{(x,y)\in \partial \Omega \big| C^1x+C^2y&gt;P_0\}$</p>
<p style="text-align: center;">$latex \Gamma_2=\partial \Omega\setminus \Gamma_1$</p>
Ideally, we would like to sell the portfolio at a price $latex x$ for the first stock and $latex y$ for the second one, so that $latex (x,y)\in \Gamma_1$, since this way we would earn money.

Let $latex u_{C_1, C_2}(x,y)$ be the probability of hitting $latex \Gamma_1$ first when hitting the boundary for the first time. Then, our objective is find $latex C_1,C_2$ so that $latex u_{C_1,C_2}(S_0^1,S_0^2)$ is maximum. But we know that $latex u_{C_1,C_2}$ satisfies
<p style="text-align: center;">$latex \begin{cases}-\Delta u_{C_1,C_2}=0&amp;\mbox{in }\Omega,\\u_{C_1,C_2}|_{\Gamma_1}\equiv 1,\\ u_{C_1,C_2}|_{\Gamma_2}\equiv 0.\end{cases}$</p>
So using this fact, we may find the optimal $latex C_1$ and $latex C_2$. Then, we buy $latex C_1$ shares of the first stock and $latex C_2$ shares of the second one.
<h1>Practical example</h1>
Suppose that I would like to invest in to companies, AK Steel Holding Corporation (<a href="https://finance.yahoo.com/quote/AKS/">AKS</a>) and Arbor Realty Trust Inc. (<a href="https://finance.yahoo.com/quote/ABR/">ABR</a>), since after my own analysis, I think this companies will go up in the future. At the time this post was written, AKS was trading at 5.51$, and ABR at 7.14$. Suppose I have 1000$ to invest into these two stocks, and I would like to sell the portfolio if AKS or ABR goes up more than 10%, or if either of these two stocks goes down more than 5%. In our case, $latex P_0=1000$, $latex \alpha=0.05$ and $latex \beta=0.1$. If we take a look to both stocks, we can check that after using Phillips-Perron's test, the behaviour of both stocks could be just a random walk. This can be checked in R as folows:

<pre lang="PHP">
library(quantmod)
getSymbols("AKS", from="2016-03-01")
PP.test(Cl(AKS))$p.value
</pre>

Which returns a p-value of 0.3. So, solving

<p style="text-align: center;">$latex \begin{cases}-\Delta u_{C_1,C_2}=0&amp;\mbox{in }\Omega,\\u_{C_1,C_2}|_{\Gamma_1}\equiv 1,\\ u_{C_1,C_2}|_{\Gamma_2}\equiv 0.\end{cases}$</p>


for different values of $latex C_1$ and $latex C_2$, we obtain that $latex u_{C_1, C_2}(5.51, 7.14)$ is maximum when $latex C_1=18$, $latex C_2=126$, and in this case $latex u_{18, 126}(5.51, 7.14)=0.53$. That is, if the two stocks behave like a random walk, there would be a probability of 53% to obtain a positive return. So, if analysis was mistaken and AKS or ABR (or both) did not behave as expected, and they just continued as a random walk, at least I have a chance of 53% of still obtaining positive returns.
