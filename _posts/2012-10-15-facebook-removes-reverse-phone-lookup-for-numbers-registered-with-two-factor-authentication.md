---
layout: post
title: Facebook Removes Reverse Phone Lookup for Numbers Registered With Two-Factor
  Authentication
created: 1350323788
---
Ars Technica has a great <a href="http://arstechnica.com/security/2012/10/facebook-moves-to-keep-phone-numbers-for-two-factor-protection-private" target="_blank">post</a> about Facebook removing reverse lookup of phone numbers used in conjunction with two-factor authentication. This presents an interesting issue. If you know a person's phone number and you suspect that the person is using two-factor authentication, you can verify that by attempting a lookup on that phone number. If the attempt fails (and you know that the person registered his/her phone number with Facebook), then you know that the user has two-factor authentication set up. If the attempt succeeds, then the user does not have two-factor authentication enabled.

There are two possible solutions I can think of right off-hand:
<ol>
<li>Server-side: Remove reverse lookup completely.</li>
<li>Client-side: Use Google Voice as your publicly-listed (and publicly-used) phone number. Use your direct number for two-factor authentication.</li>
</ol>

I understand why Facebook would have a reverse lookup feature. Among other things, it allows a person to see if a number of a call received matches any of their Facebook friends. So in that respect, I don't support completely removing reverse lookup. It might come in handy. The second option is more appealing. Setting up Google Voice is easy. The only drawback is that making the transition to Google Voice can be time-consuming. You'll need to call (or text) all your contacts and tell them your new number. You'll have to contact all your accounts and change your registered number. I've already made that transition, but the first time I did it it took around a week. And Google's Android app still has a long ways to go for the impatient user.

In short, by removing phone numbers registered with two-factor authentication, Facebook opened an information disclosure vulnerability. This vulnerability allows attackers to verify whether a phone number is registered with two-factor authentication.
