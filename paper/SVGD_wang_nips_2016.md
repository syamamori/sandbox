# Stein variational gradient descent: A general purpose bayesian inference algorithm
Authors: Qiang Liu and Dilin Wang 
Stein's identity, variational inference 

paper:[https://arxiv.org/abs/1608.04471](https://arxiv.org/abs/1608.04471)  
code:[https://github.com/DartML/Stein-Variational-Gradient-Descent](https://github.com/DartML/Stein-Variational-Gradient-Descent)  
blog:[http://www.cs.dartmouth.edu/~dartml/project.html?p=vgd](http://www.cs.dartmouth.edu/~dartml/project.html?p=vgd)  
slide:[http://www.cs.dartmouth.edu/~qliu/PDF/steinslides16.pdf](http://www.cs.dartmouth.edu/~qliu/PDF/steinslides16.pdf)  

## 概要
- 一言  
    与えられた確率モデル$p(x)$とKLダイバージェンスが最小になる確率密度$q(x)$を求める問題（変分問題）で
    ノンパラメトリック的で高速な方法を与えた．
- 問題点  
    KLダイバージェンスが計算できない（積分できない）複雑な$p(x)$ではこれまで，
    変分ベイズがMCMCが主に用いられてきた．これらは近似による誤差や収束速度の課題があった．
- 技術的キモ  
    Kernelized Stein Discrepancy(KSD)を用いて，サンプル系列$\\{x_i\\}^n_{i=0}$の
    摂動に対するKLダイバージェンスの勾配を計算した．これにより勾配を使ってサンプルを
    更新する．

- 関連研究  
    MCMCも高速で，変分ベイズよりも精度がよい．
- 検証  
    主に一次元のp(x)への収束性の評価とベイジアンニューラルネットワークの推定精度の評価

## 定式化：

$$ q^{\*} = \mathrm\{arg\min\}\_\{q\in Q\} \{\mathrm\{KL\}(q(x)\|p(x))
\equiv \mathbb{E}\_q \[\log q(x)\] - \mathbb{E}\_q \[\log \bar{p}(x)\] + \log Z\} $$  

$p(x)$:target distribution $p(x) = \bar\{p\}(x) / Z$

Z: normalized coefficient

$Q$: a predefined set. In this article, Q is the set of smooth distributions
初期サンプル$\\{x\_i\^{0}\\}^n\_{i=0}$から以下のように更新する．

$x\_i\^{l+1} \gets x\_i^{l} + \epsilon\_l \hat{\phi}^\* (x\_i\^l)\quad $  
$\mathrm{where} \quad \hat{\phi}^\*(x) = \frac{1}{n} \sum\_{j=1}^n\[ k(x^l\_j, x) \
\nabla\_{x\_j^l} \log{p({x\_j^l})} + \nabla\_{x\_j^l}k(x\_{j}\^{l}, x)\]$

証明：
$q\_{\[T\]}(z) = q(\mathbf{T}^{-1}(z)) \cdot | \det(\nabla\_z\mathbf{T}^{-1}(z))|$
- Th.3-1
- Lemma 3-2
- Th.3-3
