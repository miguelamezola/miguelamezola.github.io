---
layout: post
title:  "Checking for Integer Divisibility"
summary: "How to check for divisibility without using the division operator"
excerpt: "How to check for divisibility without using the division operator"
date:   2021-10-23 16:30:00 -0700
tags: number theory, integers, divisibility
published: true
mathjax: true
image: /assets/divisibility-proof.png
---

Suppose that we are not allowed to use the division operator in a programming language such as JavaScript.  With this constraint, how can we check whether or not an integer evenly divides another?  Consider the following approach.

**Definition.**  Let $a$ and $d$ be integers.  We say that $d$ divides $a$ if there exists an integer $q$ such that $a=qd$.

**Proposition.** Let $a, q, d$ be integers such that $d \neq 0$.  If $a = qd$, then $ \lvert a \rvert \geq \lvert q \rvert$.

**Proof.**  Since $ a=qd $ implies that $\lvert a \rvert = \lvert qd \rvert = \lvert q \rvert \cdot \lvert d \rvert \geq \lvert q \rvert $, then $\lvert a \rvert \geq \lvert q \rvert$.  Thus, if $a = qd$, then $\lvert a \rvert \geq \lvert q \rvert$. $\blacksquare$

So, in order to check whether $d$ divides $a$, a computer program would just have to search for an integer $q$ between $-a$ and $a$ (inclusive) such that $a = qd$.  The following approach has linear time complexity; I would describe it as being naïve or brute force.

**integerDivisibility.test.js**
```javascript
const expect = require('chai').expect;
const integerDivisibility = require('./integerDivisibility');

describe('integerDivisibility', () => {
    it('returns true for -14 divides 28', () => {
	    const expected = true;
        const actual = integerDivisibility(-14, 28);
        expect(expected).to.equal(actual);
    });
});

describe('integerDivisibility', () => {
    it('returns false for 15 divides 25', () => {
        const expected = false;
        const actual = integerDivisibility(15, 25);
        expect(expected).to.equal(actual);
    });
});

describe('integerDivisibility', () => {
    it('returns false for 27 divides 9', () => {
        const expected = false;
        const actual = integerDivisibility(27, 9);
        expect(expected).to.equal(actual);
    });
});

describe('integerDivisibility', () => {
    it('returns true for 13 divides 0', () => {
        const expected = true;
        const actual = integerDivisibility(13, 0);
        expect(expected).to.equal(actual);
    });
});

describe('integerDivisibility', () => {
    it('returns false for 0 divides 42', () => {
        const expected = false;
        const actual = integerDivisibility(0, 42);
        expect(expected).to.equal(actual);
    });
});

describe('integerDivisibility', () => {
    it('returns true for 1 divides 8', () => {
        const expected = true;
        const actual = integerDivisibility(1, 8);
        expect(expected).to.equal(actual);
    });
});
```

**integerDivisibility.js**
```javascript
module.exports = (d, a) => {
    if (d != 0)
        for (let q = -a; q <= a; q++)
            if (a === q * d)
                return true;
    return false;
}
```