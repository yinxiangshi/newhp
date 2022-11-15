+++
author = "Allonsy"
title = "An introduction of Bloom Filter"
date = "2022-11-11"
description = "Guide to Bloom Filter"
tags = [
    "Tutorial",
]
+++

# Bloom Filter


<div  align="center">    
 <img src="/img/filterPic.png" width = "600" height = "216" alt="filterPic" align=center/>
</div>

## 1. Introduction

Bloom Filter, a great tool used to store immutable data. Bloom Filter doesn't support deleting files but support adding files.

### 1.1 A Math Definition

Suppose we have a set of items $D=\{d_1,d_2,d_3,\dots,d_n\}\in R^n$, and a hash functions family $\mathcal{H}\in R^K$, the filter $T$ is an array which can only store $0/1$. To store elements, we set $T[\mathcal{h}(d_i)]=1,d_i\in D, \mathcal{h}\in\mathcal{H}$.

### 1.2 False Positive
From the above, we can tell that if two elements have the same hash code, the collision will happen. For example, if $a,b$ have the same hash code, and first we store $a$, then we query if $b$ exists in the filter, we will get a positive result. This is called **false positive**.

To avoid the false positive happens, a naive solution is using different hash functions to compute different $\mathcal{h}(x)$ and set all of $T[\mathcal{h}(x)]$ to $1$. For this approach, how many hash functions we will need to balance time complexity and false positive rate?

Let's fix the size of the $T\in R^M$, we randomly choose $k$ hash functions, suppose we added all elements of $D$ to $T$, what's the probability of $T[t]=1$?

$Pr[T[t]=0]=(1-\frac{1}{M})^{kn}=e^{-kn/M}$

Hence, $Pr[T[t]=1]=1-e^{-kn/M}$, and we have $Pr[T[h_1(x)]=1\cup T[h_2(x)]=1\cup\dots\cup T[h_k(x)]=1]=(1-e^{-kn/M})^k$

To minimize this probability, let $f(k)=ln(1-e^{-kn/M})^k=kln(1-e^{-kn/M})$. Since we have $p=e^{-kn/M},k=-\frac{M}{n}lnp$, replace $k$ with this, we get $f(k)=-\frac{M}{n}lnpln(p-1)$