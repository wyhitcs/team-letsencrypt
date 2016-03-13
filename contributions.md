#Contributions
##March 5th

Fix format problem of Documentation [#2612](https://github.com/letsencrypt/letsencrypt/pull/2612)
We found a display error in Let's encrypt document at website ["Read the Docs"](http://letsencrypt.readthedocs.org/en/latest/using.html?highlight=mail#webroot). This is a command line exceeds the border of the browser window:

``letsencrypt certonly --webroot -w /var/www/example/ -d www.example.com -d example.com -w /var/www/other -d other.example.net -d another.other.example.net``

We change the way of displaying to make it visible and test it with Chrome, Firefox and Safari at Mac OSX and Ubuntu (shown below):

    letsencrypt certonly --webroot -w /var/www/example/ -d www.example.com -d example.com -w /var/www/other -d other.example.net -d another.other.example.net

It has a horizontal scroll to display a long text (both in rst and html) so that the text will not exceed the browser window.

##March 12th
[#2657](https://github.com/letsencrypt/letsencrypt/pull/2657)
The documentation of let's encrypt is not so friendly to new comers, 
Since let's encrypt is a implementation of ACME, I propose to add a link to ACME specification. 
But other contributors say the ACME I link to is not up to date. So, I close this pull request and start a new
Pull Request with correct link.


##March 13th
[#2658](https://github.com/letsencrypt/letsencrypt/pull/2658)
I fix the problem in issue #2657.
But other contributors say the document linked to is still not up to date. 
So I close this pull request and start a new one.i

##March 13th
[#2659](https://github.com/letsencrypt/letsencrypt/pull/2659)
This Pull Request is still pending, but I think this time the link is correct.
That may take a few days to merge this pull request.

#Future Plan
Thought Let's Encrypt is still developing and includes a number of bugs or errors that need a solution, it is difficult to contribute to the project currently since the it is technically complicated. However we do have a plan relating to the problem described in development view. In future weeks, we will work on this and try to make some contributions.
##Problem overview
Currently, when the clients encounter error, it won't show 'trace back' information to users. It simply crashes without any response to inform users what actually happens.  
##Plan
This problem is already an issue [#2114](https://github.com/letsencrypt/letsencrypt/issues/2114) posted in the discussion board. But nobody replies to it by now. We plan to solve this problem. The 'client' package is the potential target that we will work on. After learning its code and layout, we will try to make an improvement. We want to apply an working branches for it and then we can launch our pull request. requests. 
