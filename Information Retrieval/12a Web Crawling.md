#exam Describe in detail how you would design a web crawler to find information that could be included in the index of an Information Retrieval system. In your answer, include details about what standards the crawler should follow, and situations where accessing information is difficult.

**Web Crawlers** or **Web Spiders** are programs that automatically **look for documents** on the web that will be included in the index so that users can find them when searching.
They do this by **extracting the hyperlinks** from each page they find, which will in turn point them to new pages to index.

one study claims that using a **Breadth-First Search** strategy will result in finding **higher-quality** pages

Breadth-first spider search: 
- Manually **add a list of ”seed”** URLs to the spider’s queue of pages to visit 
- While there are still URLs in the queue of pages to visit 
	- **Remove** the first URL from the queue of pages to visit and **download the page** 
	- **Extract the links** from the document 
	- **Add these links** to the **queue** of pages to visit if they haven’t already been visited and are not already in the queue.
runs in parallel

# Polite Crawlers
Obeying robots.txt files
Not overloading the website(obey Crawl-Delay)
Not consuming too much bandwidth
Identifying who they are and who they belong to (“Googlebot”, “Baiduspider”).
## The Robots Exclusion Standard
a method of asking web crawlers **not to index certain portions** of a web site.
It is a voluntary standard（无法强制阻止）
### robots.txt
```
Robots Exclusion Rules 
	User-agent: * 
	Disallow: /cgi-bin/ 
	Disallow: /tmp/ 
	Disallow: /~joe/ 

Allow one bot 
	User-agent: Googlebot 
	Disallow: 
	
	User-agent: 
	* Disallow: /
```
## noindex and nofollow
Ignore pages that have `<meta name="robots" content="noindex" />`
# Challenge
In many cases, there may be **several ways** to find the **same** page, using different URLs. This can affect results because of duplication, but also it can affect PageRank results

> Non-static documents

dynamic environment
New pages are **added constantly**, and many existing pages may change **frequently**.
it must still be revisited so the index does not get out-of-date

there are **huge** numbers of pages that are essentially **static**
A policy must be implemented that **decides** which pages should be **reindexed**

> CAPTCHA

A CAPTCHA (Completely Automated Public Turing test to tell Computers and Humans Apart) is typically an image containing text that computers find difficult to read

steam人机验证
