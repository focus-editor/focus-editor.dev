---
layout: post
title:  "Silly coding mistakes"
date:   2018-12-28 08:00:00 -0100
categories: debugging
comments: true
---

Recently I've been following the [Ray Tracing In One Weekend](https://www.amazon.com/Ray-Tracing-Weekend-Minibooks-Book-ebook/dp/B01B5AODD8) book series by P. Shirley. Everything went smoothly until I got to the [BVH](https://en.wikipedia.org/wiki/Bounding_volume_hierarchy) part...

![scene]({{ site.url }}/assets/img/2018/Dec/bvh/scene.png)

<!--more-->

Bounding volume hierarchies are supposed to speed up ray tracing, but in my case it got 1.5x slower than the brute force solution. I put my debugging hat on, added some `printf`s, stepped through the code, and found that the number of intersection tests had indeed increased. I tried to change the way the objects are divided, but to no avail. Finally I had to admit that I didn't really get how the code worked, since I couldn't debug it, and started rewriting the ray tracer from scratch, this time trying to come up with all the algorithms myself and only consult the book if stuck.

After 3 more nights I got to the BVH part again, freshly rewritten. Instead of choosing a random axis for object sorting and division, I tried splitting bounding volumes by the largest dimension, and this time it was actually a little bit better than brute force. Success!

The following morning I decided to copy some parts of the old BVH code to the new ray tracer and try to find out why exactly it was slow after all. To my surpise, when I copied a chunk of the AABB intersection test code over, the BVH solution got 10x faster than my version!

After another hour of debugging I finally found the bug. It was one of those "how the hell did it even work before" beasts. When copying and tweaking a line I forgot to change a `less-than` sign to `greater-than`, and that should have been obvious the next time I would run the program, but I got unlucky and made a similar mistake in a totally different place. Those two typos masked each other, making the program still work but 10x slower.

Key takeaway: be extremely careful when duplicating lines and changing `+` signs to `-` signs and so on.
