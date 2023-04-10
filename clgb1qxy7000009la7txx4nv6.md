---
title: "Incremental Static Regeneration  in Nextjs"
seoTitle: "Incremental Static Regeneration  in Nextjs"
seoDescription: "Learn what Incremental Static Regeneration means"
datePublished: Mon Apr 03 2023 16:26:00 GMT+0000 (Coordinated Universal Time)
cuid: clgb1qxy7000009la7txx4nv6
slug: incremental-static-regeneration-in-nextjs
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1681143574112/6f263d77-c89f-4282-966e-6311b1178a1b.png
tags: nextjs, nextjs-isr

---

In recent years, the concept of static site generation has become increasingly popular in the world of web development. Static site generators like Gatsby, Hugo, and Jekyll have gained popularity because they offer several advantages over traditional content management systems (CMS) such as speed, security, and ease of deployment. One of the limitations of static site generators has been the lack of dynamic content. However, with the advent of incremental static regeneration, this limitation has been overcome, with ISR we regenerate a static page when its data changes without needing to rebuild the entire site.

### What is Incremental Static Regeneration (ISR)

Revalidation or Incremental Static Regeneration (ISR) is a technique that allows static site generators to update content incrementally, instead of having to rebuild the entire site every time there is new content. This approach enables faster and more efficient updates of the site's content, resulting in quicker load times and a more seamless user experience.

In a traditional static site generation, the entire site is generated from scratch whenever there is new content. This process can take a significant amount of time, especially for larger sites with a lot of pages. With ISR, however, the site is initially built in its entirety, but subsequent updates only regenerate the pages that have been modified.

Nextjs have two types of Revalidation :

* Background: Revalidates the data at a specific time interval.
    
* On-demand: Revalidates the data based on an event such as an update.
    

### Examples

E-commerce Website: An e-commerce website with a large catalog of products can use ISR to generate product pages statically and incrementally regenerate them when product data changes. This can help improve page load times and ensure that customers always see up-to-date product information.

News Website: A news website can also use ISR to generate and serve static pages for articles that don't change frequently, while incrementally regenerating pages for breaking news stories or articles that are updated frequently. This can help improve website performance and ensure that readers always have access to the latest news.

Blog: A blog can use ISR to generate and serve static pages for blog posts, while incrementally regenerating pages when new comments are added. This can help improve page load times and ensure that readers always see up-to-date comments.

## Conclusion

Incremental Static Regeneration is a powerful technique that has made static site generators even more appealing to developers. With ISR, static sites can now offer dynamic content that can be updated quickly and efficiently, resulting in faster load times and a more seamless user experience. Next.js provides a simple and intuitive way to implement ISR, making it easy for developers to adopt this technique in their projects. If you're considering building a static site, be sure to check out Next.js and the ISR feature to take advantage of this technique.

## Attributions

[https://beta.nextjs.org/docs/data-fetching/revalidating](https://beta.nextjs.org/docs/data-fetching/revalidating)

[https://nextjs.org/docs/basic-features/data-fetching/incremental-static-regeneration](https://nextjs.org/docs/basic-features/data-fetching/incremental-static-regeneration)