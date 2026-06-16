Search Engine Optimisation (SEO)
SEO involves taking steps to **get your page** high **up** in search engine **rankings**.

2 categories:
- ”White Hat” SEO: **operating ethically** and following search engine rules and guidelines to get higher rankings.
- “Black Hat” SEO: **violating** search engine **guidelines** and rules to try and get an artificial advantage: **Adversarial IR**.
# Adversarial IR
## Meta Tags
HTML allows for page authors to describe the contents of their pages by using `<meta>` tags

Even for many that did, they initially gave a **high weight** to terms appearing in the **description** and **keywords** of a page.
在meta tag里面加关键词
## Hidden Text
字体颜色和背景一致
![](/assets/12b%20Adversarial%20Information%20Retrieval/file-20260601204403952.png)

1. **Add extra keywords** for a search engine to pick up that aren’t already contained in the content of the page (known as “**keyword stuffing**”). （增加额外的关键字）
2. Increase the term frequency of important search terms. （增加关键词的频率）
## Cloaking
Cloaking is the term **given** to the practice of serving **different content** to web **crawlers** than to **users**.
A well-behaved crawler will identify itself to a web server when it visits, by way of a “**user agent string**”.

The idea is to serve **highly-targeted text** to the **crawler**, so as to gain a high ranking on particular searches. 
- Regular users would be served the normal content.
## Sneaky JavaScript
网络爬虫（和旧浏览器）通常无法执行 JavaScript 代码
there is a  `<noscript>`  tag that allows you to **specify text content for users** who are unable to run JavaScript (this is not so common anymore) 

As a result, this can be exploited in the same way as **cloaking**: web crawlers **index the contents** of the “**noscript**” tag, whereas **real users** will get **redirected** (via JavaScript) to a different page without them noticing.
## Exploiting PageRank
“**link farms**”: sites (or networks of sites) with the sole purpose of **linking to other sites** so as to increase their PageRank.

每个页面对整体PageRank 的贡献很小，因此很容易创建大量仅由链接组成的页面。 
此外，受益于此 PageRank 的页面将链接回农场，从而提高农场的 PageRank。

Another approach is “**comment spam**”, where pages that allow **third-party commenting** are abused to try and boost PageRank.
- 广告链接作为评论发布在数百或数千个地方，以从每个地方获得 PageRank

这就是为链接创建“nofollow”属性的原因。这意味着搜索引擎在计算 PageRank 时不会考虑链接。
## Doorway Pages
**Rather than** attempting to **include all** of the desired **search queries** on a single page (which will affect the term frequencies of the individual terms), sometimes individual “**doorway pages**” are created.
These are pages that are **optimised** for a **particular search query**, each of which then funnels a user to the same destination.