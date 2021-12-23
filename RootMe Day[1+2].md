## Web - Client

### Lab1: HTML - disabled buttons

**Step1:** When entering the lab we see there is 1 textfield and 1 button but we cannot use it. 

![image](https://user-images.githubusercontent.com/63194321/147231855-675b4983-ad51-410e-b55d-0dc044c0993a.png)

**Step2:** We check the website's inspect and see that these 2 fields are `disabled` 

![image](https://user-images.githubusercontent.com/63194321/147233012-891cdf61-2591-48b8-a2d5-f8738a32a05e.png)

**Step3:** Proceed to remove the `disable` method of the two fields above. The form is already usable. 

![image](https://user-images.githubusercontent.com/63194321/147233195-d85ce01c-b78a-4b6c-9611-7578b4a6ab10.png)

**Step4:** Enter any data in the form and submit the data. The site will return the `password (flag)` we are looking for 

![image](https://user-images.githubusercontent.com/63194321/147233403-083921f5-f7f7-4d51-8f17-8f813643801b.png)

### Lab2: Javascript - Authentication

**Step1:** At the interface we enter any `username` or `password`. Then there will be 1 javascript popup notification. 

![image](https://user-images.githubusercontent.com/63194321/147233923-5a185caa-b031-45e1-8a6d-e0bfa6351496.png)

**Step2:** Checking the website's inspect shows that the site is using a file `login.js`.

![image](https://user-images.githubusercontent.com/63194321/147234451-1be0d139-d7b3-4355-b73d-993ee257ad8d.png)

**Step3:** Access the javascript file by going to inspect/sources. 

![image](https://user-images.githubusercontent.com/63194321/147234594-14aed3fa-0705-4a48-a901-260021cf36b4.png)

**Step4:** In this file we can see the disclosure of login information.

![image](https://user-images.githubusercontent.com/63194321/147234817-3a221d72-bad3-4f30-887e-7d077b54b14b.png)

**Step5:** Use the information entered above to access the website. And the login password is also the flag of the lab. 

![image](https://user-images.githubusercontent.com/63194321/147234979-ce1782a4-f148-413e-9c6b-371d490d14f4.png)

### Lab3: Javascript - Source

**Step1:** Accessing the post has a popup asking for a password. I enter any. 

![image](https://user-images.githubusercontent.com/63194321/147235254-8b8b819e-ea48-4c62-b482-4242659008d9.png)

**Step2:** Inspecting the site, we see that in the `header` section there is a function `login` 

![image](https://user-images.githubusercontent.com/63194321/147235440-f2ce19d8-eb44-484c-928e-cef87b71765f.png)

**Step3:** We can determine the password to log in through the `login` function. 

![image](https://user-images.githubusercontent.com/63194321/147235612-2e31a459-4366-4557-8c03-498bd7318daa.png)

**Step4:** Use the password to login to the website. And the password is also the flag of the lab. 

### Lab4: Javascript - Authentication 2

**Step1:** Access the lab and log in to the system.

![image](https://user-images.githubusercontent.com/63194321/147236591-52afa5aa-9e72-455c-bd6b-d55d9650f16d.png)

**Step2:** Enter optional login information. 

![image](https://user-images.githubusercontent.com/63194321/147236827-2f09c41a-cc57-479d-b206-86a0ab6bcbd7.png)

**Step3:** Check the site's inspect. We see the website uses `login.js` to verify the login. 

![image](https://user-images.githubusercontent.com/63194321/147236968-c6ea08c5-d366-4b72-b135-0b9d4d2f9918.png)

**Step4:** Checking the file `login.js` we find the login information. 

![image](https://user-images.githubusercontent.com/63194321/147237113-45928f0a-2e04-4acf-ad9b-a0aafabd8ed9.png)

**Step5:** Use the login information you just found to log into the website. The login password is also the flag of the lab. 

