# Взлом тега <a> в 100 символов

Совсем недавно я обнаружил, что JavaScript позволяет **изменять атрибут href у 
<a> *после* того, как вы кликнете по нему**. Это может показаться не таким уж 
серьезным на первый взгляд, но будьте уверены, так покупатели могут попасться 
на крючок и передать свои личные данные мошенникам.

Давайте я покажу вам пример.
<a href="http://www.paypal.co.uk/" onclick="this.href='http://bit.ly/141nisR'">
  Эта ссылка должна привести вас на PayPal
</a>.

Вы увидите, что она **не ведет** на PayPal (*кроме Opera, где это, похоже, было 
исправлено*). Это происходит потому, что когда вы кликнули по ссылке, я 
запустил код, который изменил атрибут *href*, и браузер неожиданно повел меня 
по *новой ссылке*. **такого быть не должно.** Посетители сайта (и, возможно, 
наиболее технически подкованные люди) могут и будут считать, что то место, где 
они оказались, может быть обычной переадресацией, в данном случае, с PayPal. В 
прошлом году PayPal на протяжении месяцев перенаправлял пользователей с главной 
страницы их британского сайта на paypal-business.co.uk. Мне кажется, что 
посетители сайтов были приучены к перенаправлениям, и если этот изъян будет 
использоваться именно так, то это может представлять собой реальную угрозу 
<s>того, что я называю Фишинг 2.0</s>.

Давайте взглянем на JavaScript:

    // Uncompressed
    var links = document.links;
    for(i in links) {
      links[i].onclick = function(){
          this.href = 'http://bit.ly/141nisR';
      };
    }

    // Compressed (was 100; now 67 characters exc. the link)
    // Thanks to sgoel from HN
    o=document.links;for(i in o){o[i].onclick=function(){this.href='//bit.ly/141nisR'}}

It's also very difficult to detect. Almost *everyone* who uses 
JavaScript/jQuery will bind an event to an <a> tag, so it's not as simple 
as unbinding every <a> onclick function. It's very much possible to wrap the 
code above to a *setTimeout* to bypass whatever solution can be found. Any 
half-decent hacker can make a computer virus or embeddable JavaScript code that 
can inject this code alongside another piece of software. As it's incredibly 
easy to update JavaScript (particularly embeddable), I would say that tools 
such as McAfeeSecure and PhishTank won't be able to keep up with phishing 
websites up to the second.

<s>As it shows no real benefit, I'm pledging to World Wide Web Consortium (W3C) 
  and major browsers to disable the option to change the *href* attribute 
  **after an onclick event**. It is an incredibly simple interpreter flaw, and 
  whilst it may seem normal to some, it can be used for ill-fated purposes 
  rather than good. I'm aware Google and websites as such use this, but if 
  we're suppose to making the web safer, we can't allow for what can be simple 
  flaws to exist. There are alternatives (such as using the genuine link rather 
  than masking it), and for that reason, it should be disabled. It's not worth 
  internet users being victims of fraud and theft.</s>

*Update (19/3)* — My new pledge is to **warn users if the location of a link 
changes to a different domain after they click on it** ([+1 to abididea][1]). 
Sites like Google and Facebook can continue operating normally as they use the 
same domain, the internet doesn't break, and it eliminates the possibility of 
phishing. Everyone wins (except phishers, of course!). I **need your help** to 
make major browsers adopt this as quickly as possible. Let's make it one less 
easy way for fraudsters to victimize internet users.

*Update (19/3)* — I've suggested the fix to Firefox, and waiting for a response.

[1]: http://www.reddit.com/user/abadidea