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

Once you've established the framework of a website, either from the above favicon example or by looking for clues in the page source such as comments, 
copyright notices or credits, you can then locate the framework's website. From there, we can learn more about the software and other information, possibly leading to more content we can discover.

First we access the link to get the account and the path of `admin`.
>  https://static-labs.tryhackme.cloud/sites/thm-web-framework

![image](https://user-images.githubusercontent.com/63194321/149957585-f314b950-94c9-4170-9bf1-edef78a8a1a7.png)


Next visit the link, log in to the `admin` account and retrieve the flag.
> http://10.10.219.61//thm-framework-login

![image](https://user-images.githubusercontent.com/63194321/149958549-5c38ddd2-406e-442e-9c6e-66f55a51dc7b.png)

### II. OSINT (Open-Source Intelligence)

#### 1. Google Hacking / Dorking

Google hacking / Dorking utilizes Google's advanced search engine features, which allow you to pick out custom content.

Here is an example of more filters you can use:

![image](https://user-images.githubusercontent.com/63194321/149959165-a3254685-2d6e-4df1-92bd-377f54b26ce9.png)

More information about google hacking can be found [(here)](https://en.wikipedia.org/wiki/Google_hacking)

#### 2. Wappalyzer, Wayback Machine

* [Wappalyzer](https://www.wappalyzer.com/) is an online tool and browser extension that helps identify what technologies a website uses, such as frameworks, Content Management Systems (CMS), payment processors and much more, and it can even find version numbers as well.
* The [Wayback Machine](https://archive.org/web/) is a historical archive of websites that dates back to the late 90s. You can search a domain name, and it will show you all the times the service scraped the web page and saved the contents. This service can help uncover old pages that may still be active on the current website.

#### 3. GitHub

To understand GitHub, you first need to understand Git. Git is a version control system that tracks changes to files in a project.
Working in a team is easier because you can see what each team member is editing and what changes they made to files. When users have finished making their changes, 
they commit them with a message and then push them back to a central location (repository) for the other users to then pull those changes to their local machines.
GitHub is a hosted version of Git on the internet. Repositories can either be set to public or private and have various access controls. 
You can use GitHub's search feature to look for company names or website names to try and locate repositories belonging to your target. 
Once discovered, you may have access to source code, passwords or other content that you hadn't yet found.

#### 4. S3 Buckets

S3 Buckets are a storage service provided by Amazon AWS, allowing people to save files and even static website content in the cloud accessible over HTTP and HTTPS. The owner of the files can set access permissions to either make files public, private and even writable. Sometimes these access permissions are incorrectly set and inadvertently allow access to files that shouldn't be available to the public. The format of the S3 buckets is `http(s)://{name}.s3.amazonaws.com` where {name} is decided by the owner. S3 buckets can be discovered in many ways, such as finding the URLs in the website's page source, GitHub repositories, or even automating the process. One common automation method is by using the company name followed by common terms such as {name}-assets, {name}-www, {name}-public, {name}-private, etc.

### III. Automated Discovery

* What is Automated Discovery: Automated discovery is the process of using tools to discover content rather than doing it manually. This process is automated as it usually contains hundreds, thousands or even millions of requests to a web server. These requests check whether a file or directory exists on a website, giving us access to resources we didn't previously know existed. This process is made possible by using a resource called wordlists.
* Automation Tools: Although there are many different content discovery tools available, all with their features and flaws, we're going to cover three which are preinstalled on our attack box, ffuf, dirb and gobuster.

Use `ffuf` to find hidden paths.

![image](https://user-images.githubusercontent.com/63194321/149961701-30c368dd-faaa-4834-9a00-45e53294862d.png)

## End! 
  






