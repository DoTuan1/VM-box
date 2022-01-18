## TryHackMe_Content Discovery

### Overview 

Content can be many things, a file, video, picture, backup, a website feature.  When we talk about content discovery, we're not talking about the obvious things we can see on a website; it's the things that aren't immediately presented to us and that weren't always intended for public access.

There are three main ways of discovering content on a website

*  Manually
*  Automated
*  OSINT (Open-Source Intelligence)

### I. Manual Discovery 

#### 1. Robots.txt

The robots.txt file is a document that tells search engines which pages they are and aren't allowed to show on their search engine results or ban specific search engines from crawling the website altogether. 
It can be common practice to restrict certain website areas so they aren't displayed in search engine results. These pages may be areas such as administration portals or files meant for the website's customers.
This file gives us a great list of locations on the website that the owners don't want us to discover as penetration testers.

Perform a read of the robots.txt file with the `curl` command.
> curl http://10.10.101.50/robots.txt

![image](https://user-images.githubusercontent.com/63194321/149931621-df5a7855-6c91-482c-a974-cd6bbd161e1b.png)

#### 2. Favicon

The favicon is a small icon displayed in the browser's address bar or tab used for branding a website.
Sometimes when frameworks are used to build a website, a favicon that is part of the installation gets leftover, 
and if the website developer doesn't replace this with a custom one, this can give us a clue on what framework is in use. 
OWASP host a database of common framework icons that you can use to check against the targets favicon [(check here)](https://wiki.owasp.org/index.php/OWASP_favicon_database). 
Once we know the framework stack, we can use external resources to discover more about it (see next section).

Use command to load favicon and convert to `md5` value.
> curl https://static-labs.tryhackme.cloud/sites/favicon/images/favicon.ico | md5sum

![image](https://user-images.githubusercontent.com/63194321/149932182-c52440c9-4f10-4060-8464-dfade92e165a.png)

Use the `md5` value to search the OWASP database to display the icon's code.

![image](https://user-images.githubusercontent.com/63194321/149932376-8231a522-55ea-4e5a-a060-2ca053b7d48f.png)

#### 3.  Sitemap.xml

Unlike the robots.txt file, which restricts what search engine crawlers can look at, the sitemap.xml file gives a list of every file the website owner wishes to be listed on a search engine. 
These can sometimes contain areas of the website that are a bit more difficult to navigate to or even list some old webpages that the current site no longer uses but are still working behind the scenes.


Use `curl` to read the `sitemap.xml` of the web page.
> curl http://10.10.101.50/sitemap.xml  

![image](https://user-images.githubusercontent.com/63194321/149932775-8523d281-5ed2-4e10-975d-65f9117ff4aa.png)

#### 4. HTTP Headers

When we make requests to the web server, the server returns various HTTP headers. 
These headers can sometimes contain useful information such as the webserver software and possibly the programming/scripting language in use.

Use `curl` to read the `header` of the web page.
> curl http://10.10.101.50 -I 

![image](https://user-images.githubusercontent.com/63194321/149933076-e19653f9-6e85-402e-bd24-edb600769150.png)


#### 5. Framework Stack
