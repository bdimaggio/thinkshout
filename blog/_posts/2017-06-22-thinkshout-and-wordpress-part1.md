---
layout: blog
body-class: blog-post
topic: technology
title: ThinkShout and WordPress, Part One
homepage: false
author: joe
published: true
featured: false
short: A solid base for theming world-class websites, regardless of CMS.
tags:
  - WordPress
  - PHP
  - Front-end
  - Full Stack
date: 2017-06-22 13:00:00
image: https://thinkshout.com/assets/images/ts_icon.jpg
---

The message to the tech channel was simple enough:

>"Anyone have experience with WordPress?"

[WordPress!](https://wordpress.com/) I'd started using it for consulting work ten years earlier, and had been working with it on and off until a fairly recently. During my tenure here at ThinkShout, the only platforms we'd used were [Drupal](https://www.drupal.org/) and [Jekyll](https://jekyllrb.com/), so the question was both surprising and delightful. I jumped at the chance to reacquaint myself with my first CMS.

Most of my previous work had been small, virtual-hosted sites that functioned primarily as a company brochure with some blog, news, or event functionality - in other words, ideal WordPress sites. ThinkShout generally uses [Jekyll](https://jekyllrb.com/) for that sort of client work, so I was curious what sort of site we'd be developing.

In fact, we had an opportunity to take over maintenance of two websites: [Travel Portland](https://www.travelportland.com/) and the related but separate Meeting Planner site. It turns out I'd moved to Portland at the perfect time!

Both sites had a fair amount of ad-hoc code, but there was nothing too terrible. Just a lot of the head-scratching that comes from trying to figure out why someone did a thing a certain way. The Meeting Planner site was the one that needed the most attention - it wasn't [Responsive](//alistapart.com/article/responsive-web-design), and was achingly slow due to a reliance on cross-site queries. We were assured that after we'd stabilized things on the main site and fixed the most pressing issues on the Meetings site, we'd be looking at a rebuild from the ground up. This was in 2015.

After a good year+ of maintenance and modest feature enhancements, including a non-trivial hosting upgrade to [Pantheon](https://pantheon.io/), we began discovery and wireframes in partnership with both the client and their design firm, [Grady Britton](https://gradybritton.com/). There were a number of exciting aspects to the design - perhaps the most eye-catching was the 'Speech Bubble' on the home page.

One of the first questions that came my way was whether or not this was feasible in a responsive design. I could tell right away that the nicely hand-drawn quality of the speech bubble would be something Grady Britton would want to keep.

I suspected that it would be possible using a background SVG and careful use of margins and padding. After a little research, I found what I was looking for - an SVG property called `preserveAspectRatio`. If this is set to `none`, the SVG will deform to fit the container. During implementation the margin/padding was slightly more complicated than anticipated, but by using some custom media queries, we were able to tweak the margins, padding, and font size as the SVG shrank.

As for actually _theming_ in WordPress, this was quite an interesting change from Drupal. These days, Drupal uses the Twig templating language to ensure that there's no PHP in templates. Additionally, Drupal uses template and module functions (called hooks) to alter the rendered code. WordPress is still very simple in this regard. Set up the right kind of template based on the hierarchy, use a couple of custom functions to call in content (or Advanced Custom Fields), and after that it's all on you!

From a theming perspective, this is ideal - the ability to use precise markup in the template and only bring in the fields you need makes theming very efficient. Having everything in a single template (or partial) means you don't have to hunt through the site to figure out where to alter the code.

Many features of the site could be developed by one person, a rarity on most Drupal builds. Even in scenarios where a back-end developer handed a template to a front-end developer, it was easy for the front-end developer to look at the template and tweak the markup (adding a class or an occasional wrapper).














