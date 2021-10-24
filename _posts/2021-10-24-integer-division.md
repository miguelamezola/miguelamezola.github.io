---
layout: post
title:  "Custom Integer Division Function"
summary: "How to perform integer division without using the division operator in JavaScript"
excerpt: "How to perform integer division without using the division operator in JavaScript"
date: 2021-10-24 08:10:00 -0700
tags: number theory, integers, division
published: true
mathjax: true
image: /assets/int-div.png
---

If we are not allowed to use the division operator in a programming language, how can we perform integer division?  The following code uses repeated subtraction to achieve this; its time complexity is Big O of the quotient.  One interesting tidbit: `NaN` is the only value in JavaScript that does not equal itself, so the last unit test had to be adjusted to account for this.

**integerDivision.test.js**
```javascript
const expect = require('chai').expect;
const integerDivision = require('./integerDivision');

describe('integerDivision', () => {
    it('returns 0 for 2 divided by 7', () => {
        const expected = 3;
        const actual = integerDivision(7, 2);
        expect(expected).to.equal(actual);
    });

    it('returns 0 for -4 divided by 9', () => {
        const expected = 0;
        const actual = integerDivision(-4, 9);
        expect(expected).to.equal(actual);
    });

    it('returns 0 for 3 divided by -11', () => {
        const expected = 0;
        const actual = integerDivision(3, -11);
        expect(expected).to.equal(actual);
    });

    it('returns 0 for -19 divided by -21', () => {
        const expected = 0;
        const actual = integerDivision(-19, -21);
        expect(expected).to.equal(actual);
    });

    it('returns 4 for 44 divided by 10', () => {
        const expected = 4;
        const actual = integerDivision(44, 10);
        expect(expected).to.equal(actual);
    });

    it('returns -4 for -21 divided by 5', () => {
        const expected = -4;
        const actual = integerDivision(-21, 5);
        expect(expected).to.equal(actual);
    });

    it('returns -7 for 35 divided by -5', () => {
        const expected = -7;
        const actual = integerDivision(35, -5);
        expect(expected).to.equal(actual);
    });

    it('returns 3 for -23 divided by -6', () => {
        const expected = 3;
        const actual = integerDivision(-23, -6);
        expect(expected).to.equal(actual);
    });

    it('returns 1 for -1 divided by -1', () => {
        const expected = 1;
        const actual = integerDivision(-1, -1);
        expect(expected).to.equal(actual);
    });

    it('returns 1 for 1 divided by 1', () => {
        const expected = 1;
        const actual = integerDivision(1, 1);
        expect(expected).to.equal(actual);
    });

    it('returns -1 for 1 divided by -1', () => {
        const expected = -1;
        const actual = integerDivision(1, -1);
        expect(expected).to.equal(actual);
    });

    it('returns 0 for 0 divided by 13', () => {
        const expected = 0;
        const actual = integerDivision(0, 13);
        expect(expected).to.equal(actual);
    });

    it('returns -0 for 0 divided by -23', () => {
        const expected = -0;
        const actual = integerDivision(0, -23);
        expect(expected).to.equal(actual);
    });

    it('returns Infinity for 41 divided by 0', () => {
        const expected = Infinity;
        const actual = integerDivision(41, 0);
        expect(expected).to.equal(actual);
    });

    // NaN is the only value is javascript that does not equal itself
    it('returns NaN for "a" divided by 9', () => {
        const expected = NaN;
        const actual = integerDivision("a", 9);
        expect(isNaN(actual)).to.equal(true);
    });
});
```

**integerDivision.js**
```javascript
module.exports = (a,b) => {
    // Return NaN if 'a' or 'b' are not numbers
    if (isNaN(a) || isNaN(b))
        return NaN;

    // Return Infinity like the JavaScript
    // division operator does when 'b' is 0
    if (b == 0)
        return Infinity

    // Use this bool to return a negative result
    // when 'a' or 'b' and not both are negative
    let negative = false;

    // If negative, replace 'a' with its absolute
    // value and toggle the 'negative' bool, then
    // do the same for 'b'
    if (a < 0) {
        a = a * -1;
        negative = !negative;
    }

    if (b < 0) {
        b = b * -1;
        negative = !negative;
    }

    // Count the number of time that 'b' goes into 'a'
    let count = 0;
    while (a >= b) {
        a = a - b;
        count++
    }
    return negative ? -count : count;
}
```