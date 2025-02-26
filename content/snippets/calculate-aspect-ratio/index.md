+++
authors = ["Caleb Trevatt"]
title = "Aspect Ratio Calculator"
description = "Cheeky Lo-Fi Pixel Art Aspect Ratio Calculator"
date = 2025-02-25

[extra]

[taxonomies]
tags = ["Snippet", "Javascript", "Calculator", "CodePen"]
+++

## Why?
I threw this together for two reasons:
1. Aspect ratio is tedious to calculate by hand.
2. I wanted to try using CodePen.

### Tedium

To calculate aspect ratio, you need the Greatest Common Divisor (GCD). You can get this with the Euclidean algorithm, but doing this by hand or in a phone calculator is still slow and manual.

You can side-step GCD by using `N:1` aspect ratio instead, e.g. "1.85:1"

Just divide. `1920/1080 == 1.85`

But unless you're a cinephile, that's not as common and probably not what you're looking for.

### CodePen

Anyway. Code to the rescue!

<p class="codepen" data-height="600" data-default-tab="result" data-slug-hash="wBvzWNW" data-pen-title="Aspect Ratio Calculator" data-user="in03" style="height: 600px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">
  <span>See the Pen <a href="https://codepen.io/in03/pen/wBvzWNW">
  Aspect Ratio Calculator</a> by Caleb Trevatt (<a href="https://codepen.io/in03">@in03</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://public.codepenassets.com/embed/index.js"></script>

## How?

Check out the [source code](https://codepen.io/in03/pen/wBvzWNW).

I initially did this in Python which was very easy with the function built into the standard library:

```python
from math import gcd
```

Then I wanted to publish it to my site, so I moved it to Javascript. Unfortunately JS doesn't have a GCD function built in...

```javascript
function gcd(a, b) {
    return b === 0 ? a : gcd(b, a % b);
}
```

This is a recursive function implementing the [Euclidean algorithm](https://en.wikipedia.org/wiki/Euclidean_algorithm) to find the greatest common divisor of two numbers.

{% alert(note=true) %}
If you're unfamiliar with that questionmark / colon syntax, that's a [`ternary operator`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Conditional_operator). 
It's essentially a one liner `if/else` statement. 
{% end %}

1. If `b` is `0`, it returns `a` (base case).
2. Otherwise, it calls itself recursively with `b` and `a % b` until `b`becomes `0`, at which point `a` is the`GCD`.

```javascript
let divisor = gcd(width, height);
let aspectWidth = width / divisor;
let aspectHeight = height / divisor;
```

This finds the largest number that evenly divides both `width` and `height`.

## Notes on CodePen
I heard an [episode of the Changelog recently](https://changelog.com/friends/72) interviewing Chris Coyier and Dave Rupert. 
Chris Coyier is the founder of Codepen and talked a little bit about it. Some of the features sounded really interesting. 
I threw together this junky calculator and wondered briefly about embedding it straight into my site, but I thought I'd take the opportunity to give CodePen a whirl.

I'd never actually written of my own in CodePen before, but I've messed around with it online when I've stumbled into the world of frontend demos and visual javascript experiments.

### Some Key Points No One Asked For
- **Layout**: Not a fan of the default view. I like the results on the right and the HTML, CSS, JS stacked on the left.

- **Ducks in a row**: Indentation seems quirky. Alignment always seems to overshoot on the first tab.

- **Brainrot**: I was hankering for my IDE and AI code completion... Maybe more of a comment on me than CodePen.

- **Activation Energy**: I wouldn't have bothered with this if I had to start a git repo, create a project, spin up a web server, etc. 
  How much learning have I missed because I haven't throw together junky tools with rapid prototyping?
