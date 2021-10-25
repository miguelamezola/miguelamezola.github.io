---
layout: post
title:  "Checking for Integer Divisibility Using Binary Search"
summary: "Binary search speeds up our check for divisibility without using the division operator or the remainder operator"
excerpt: "Binary search speeds up our check for divisibility without using the division operator or the remainder operator"
date:   2021-10-25 06:45:00 -0700
tags: number theory, integers, divisibility
published: true
mathjax: true
image: /assets/divisibility-proof.png
---

In a [previous post]({% post_url 2021-10-23-integer-divisibility %}){:target="_blank"}, we proved that if $a = qd$ for integers $a, q, d$ such that $d \neq 0$, then $ \lvert a \rvert \geq \lvert q \rvert$.  So, if we can find an integer $q$ in $[-a, a]$ such that $a = qd$, then we can say that $d$ divides $a$.  The approach we used had a linear time complexity &mdash; some function of $2a + 1$ &mdash; because we naïvely checked each possible $q$ within that range.

However, we can find $q$ much faster by using binary search.  The following solution has a time complexity of $\mathcal{O}(\ln a)$.  It passes all the same test cases as our previous approach. Note that we are still not using the division operator here; in order to find `mid`, we use bit shifting.

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
        // carefully choosing our left and right indices cuts our
        // search range in half from [-a, a] to [-a, 0) or (0, a]
        let left;
        let right;

        if (a < 0) {
            if (d < 0) {
                left = 1;
                right = -a;
            } else {
                left = a;
                right = -1;
            }
        } else {
            if (d < 0) {
                left = -a;
                right = -1;
            } else {
                left = 1;
                right = a;
            }
        }

        // binary search
        while (left + 1 < right) {
            const mid = (left + right) >> 1;
            const prod = mid * d;
            if (a == prod) {
                return true;
            }

            if (d < 0) {
                if (a < prod) {
                    left = mid;
                } else {
                    right = mid;
                }
            } else {
                if (a < prod) {
                    right = mid;
                } else {
                    left = mid;
                }
            }
        }
    }
    return false;
}
```