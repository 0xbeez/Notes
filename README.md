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

1. XSS - reflected
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
