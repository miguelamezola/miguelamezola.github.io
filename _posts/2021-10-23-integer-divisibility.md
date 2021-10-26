---
layout: post
title:  "Checking for Integer Divisibility"
summary: "How to check for divisibility without using the division operator or the remainder operator"
excerpt: "How to check for divisibility without using the division operator or the remainder operator"
date:   2021-10-23 16:30:00 -0700
tags: number theory, integers, divisibility
published: true
mathjax: true
image: /assets/divisibility-proof.png
---

Suppose that we are not allowed to use the division operator or remainder operator in a programming language such as JavaScript.  With this constraint, how can we check whether or not an integer evenly divides another?  Consider the following approach.

**Definition.**  Let $a$ and $d$ be integers.  We say that $d$ divides $a$ if there exists an integer $q$ such that $a=qd$.

**Proposition.** Let $a, q, d$ be integers such that $d \neq 0$.  If $a = qd$, then $ \lvert a \rvert \geq \lvert q \rvert$.

**Proof.**  Since $ a=qd $ implies that $\lvert a \rvert = \lvert qd \rvert = \lvert q \rvert \cdot \lvert d \rvert \geq \lvert q \rvert $, then $\lvert a \rvert \geq \lvert q \rvert$.  Thus, if $a = qd$, then $\lvert a \rvert \geq \lvert q \rvert$. $\blacksquare$

So, in order to check whether $d$ divides $a$, a computer program would just have to search for an integer $q$ between $-a$ and $a$ (inclusive) such that $a = qd$.  The following approach has linear time complexity, a function of $a$; I would describe it as being naïve or brute force.

If we are concerned with numeric overflow, then we should constrain $a$ and $d$ to have an absolute value no greater than $46340$, which is between $2^{15}$ and $2^{16}$.

**divides.test.js**
```javascript
const expect = require('chai').expect;
const divides = require('./divides');

describe('divides', () => {
    // negatives
    it('returns true for -14 divides 28', () => {
        const expected = true;
        const actual = divides(-14, 28);
        expect(expected).to.equal(actual);
    });

    it('returns false for -14 divides 13', () => {
        const expected = false;
        const actual = divides(-14, 13);
        expect(expected).to.equal(actual);
    });

    it('returns true for 9 divides -18', () => {
        const expected = true;
        const actual = divides(9, -18);
        expect(expected).to.equal(actual);
    });

    it('returns false for 32 divides -2', () => {
        const expected = false;
        const actual = divides(32, -2);
        expect(expected).to.equal(actual);
    });

    it('returns true for -4 divides -56', () => {
        const expected = true;
        const actual = divides(-4, -56);
        expect(expected).to.equal(actual);
    });

    it('returns false for -5 divides -99', () => {
        const expected = false;
        const actual = divides(-5, -99);
        expect(expected).to.equal(actual);
    });

    // positives
    it('returns false for 15 divides 25', () => {
        const expected = false;
        const actual = divides(15, 25);
        expect(expected).to.equal(actual);
    });

    it('returns true for 9 divides 72', () => {
        const expected = true;
        const actual = divides(9, 72);
        expect(expected).to.equal(actual);
    });

    it('returns false for 27 divides 9', () => {
        const expected = false;
        const actual = divides(27, 9);
        expect(expected).to.equal(actual);
    });

    // zeroes
    it('returns true for 13 divides 0', () => {
        const expected = true;
        const actual = divides(13, 0);
        expect(expected).to.equal(actual);
    });

    it('returns false for 0 divides 42', () => {
        const expected = false;
        const actual = divides(0, 42);
        expect(expected).to.equal(actual);
    });

    it('returns false for 0 divides 0', () => {
        const expected = false;
        const actual = divides(0, 0);
        expect(expected).to.equal(actual);
    });

    // ones
    it('returns true for 1 divides 8', () => {
        const expected = true;
        const actual = divides(1, 8);
        expect(expected).to.equal(actual);
    });

    it('returns false for 13 divides 1', () => {
        const expected = false;
        const actual = divides(13, 1);
        expect(expected).to.equal(actual);
    });

    it('returns true for 1 divides 1', () => {
        const expected = true;
        const actual = divides(1, 1);
        expect(expected).to.equal(actual);
    });
});
```

**divides.js**
```javascript
module.exports = (d, a) => {
    // some edge cases
    if (d == 0) {
        return false;
    }

    if (a == 0 || d == 1) {
        return true;
    }

    if (Math.abs(d) <= Math.abs(a)) {
        let start = -a;
        let end = a;
        if (a < 0) {
            start = a;
            end = -a;
        }
        for (let q = start; q <= end; q++)
            if (a === q * d)
                return true;
    }
    return false;
}
```