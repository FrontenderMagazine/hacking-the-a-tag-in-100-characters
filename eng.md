# Hacking the `<a>` tag in 100 characters

A short while ago, I discovered that JavaScript allows you to **change the `<a>`
href *after* you click on it**. It may not seem that serious at first glance,
but rest assured, it can trick customers into giving in their details to
fraudsters.

Let me show you an example. <a href="http://www.paypal.co.uk/"
onclick="this.href='demo.html'">This link should take you to PayPal</a>.

You'll see that you **do not** end up on PayPal (*except on Opera, where it
appears to have been fixed*). That's because when you clicked on the link, I
ran some code that changed the *href* attribute and, surprisingly, the browser
sent me to the *new link*. **That shouldn't happen.** Website visitors (and
perhaps most tech-savvy people) can and will presume where they end up could
just be a genuine redirection from, in this case, PayPal. Last year, PayPal
redirected their UK homepage to paypal-business.co.uk for months. My
assumption is website visitors have grown accustom to redirections, and if
this flaw acts as such, it can pose a real threat <s>to what I call Phishing
2.0</s>.

Let's take a look at the JavaScript:

    // Uncompressed
    var links = document.links;
    for(i in links) {
      links[i].onclick = function(){
          this.href = 'demo.html';
      };
    }

    // Compressed (was 100; now 67 characters exc. the link)
    // Thanks to sgoel from HN
    o=document.links;for(i in o){o[i].onclick=function(){this.href='demo.html'}}

It's also very difficult to detect. Almost *everyone* who uses
JavaScript/jQuery will bind an event to an `<a>` tag, so it's not as simple as
unbinding every `<a>` onclick function. It's very much possible to wrap the code
above to a *setTimeout* to bypass whatever solution can be found. Any half-
decent hacker can make a computer virus or embeddable JavaScript code that can
inject this code alongside another piece of software. As it's incredibly easy
to update JavaScript (particularly embeddable), I would say that tools such as
McAfeeSecure and PhishTank won't be able to keep up with phishing websites up
to the second.

<s>As it shows no real benefit, I'm pledging to World Wide Web Consortium (W3C)
and major browsers to disable the option to change the *href* attribute
**after an onclick event**. It is an incredibly simple interpreter flaw, and
whilst it may seem normal to some, it can be used for ill-fated purposes
rather than good. I'm aware Google and websites as such use this, but if we're
suppose to making the web safer, we can't allow for what can be simple flaws
to exist. There are alternatives (such as using the genuine link rather than
masking it), and for that reason, it should be disabled. It's not worth
internet users being victims of fraud and theft.</s>

*Update (19/3)* — My new pledge is to **warn users if the location of a link
changes to a different domain after they click on it** ([+1 to abididea][1]).
Sites like Google and Facebook can continue operating normally as they use the
same domain, the internet doesn't break, and it eliminates the possibility of
phishing. Everyone wins (except phishers, of course!). I **need your help** to
make major browsers adopt this as quickly as possible. Let's make it one less
easy way for fraudsters to victimize internet users.

*Update (19/3)* — I've suggested the fix to Firefox, and waiting for a
response.

[1]: http://www.reddit.com/user/abadidea