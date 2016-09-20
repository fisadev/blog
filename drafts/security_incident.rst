I'm writting this down because as far as I can tell, someone might be targetting me, and gaining access to my online accounts and other private stuff (there is one other possible explanation so far).

I have my own theories (detailed below), but I would definitely benefit from other people giving it a thought, maybe identifying some other possible causes or methods.

I'll post everything I know that happened as a timeline.

And yes, you will find some not-optimal security practices. I know I didn't do everything perfectly, but it's still hard to know how I'm being attacked (if so). If you don't use a helmet while riding a bike, it's perfectly normal to hit your head with a low hanging branch, but it's not normal to have the bike explode on your face.

We are speaking of personal accounts. I handle work stuff quite differently. And I'm sure most people do the same: you don't use top-level security practices to store the password of that random forum that asked for a login to download a game mod.

After this incident, most of my work credentials have been revoked and re-generated, just in case. And if you ever gave me access to anything that you think I could have stored the credentials of, then I suggest you to revoke them too.

Lots of years ago
=================

I started using ``fisadev@gmail.com`` for email and as my identity on the internet.

2011
====

I created a secondary gmail account: ``franciswpritchard@gmail.com``, to test some random stuff with the facebook api.

My main account (``fisadev@gmail.com``) was added to that account as a recovery address. I also configured my gmail (on my main account) to check for emails to this second account via pop3.

I used this account for just 1 thing: to create the facebook account. Besides that, I didn't use that account for anything else. Not a single email sent, not a single app added. In fact, I almost forgot I had it. 

And I didn't do anything with the facebook account either. No friends added, no posts, etc. Just a few queries through the api.


2012
====

I created another gmail account: ``jfisanotti@gmail.com``, to "reserve" my first-letter-and-lastname as an account. Just in case someday I needed a more serious account, as my main account is just a nickname.

My main account (``fisadev@gmail.com``) was added to that account as a recovery address too. I also configured my gmail (on my main account) to check for emails to this second account via pop3.

I used this account for just one thing: to create a Microsoft Windows account, as for some reason it didn't let me use my main email. Besides that, I didn't use that account for anything else. Not a single email sent, not a single app added.

2013
====

Decided to start writting down my logins somewhere. Went for a simple option, "secure enough" for my personal stuff:

* an unencrypted txt file
* each line was a service 
* the service account name was stored as plain text
* the password itself wasn't stored, instead, a small hint (usually 2 letters) was included
* the file was stored in a private git repository at bitbucket.org.

Example:

``google.com: jfisanotti / bk``

That would mean: the ``jfisanotti`` account from ``google.com`` service, has the ``bigkimonoman`` password (fake, example).

If someone had access to my txt file, then the person would still need to be inside my head to get the passwords... or not.

Some services shared passwords, and thus hints. If someone had my password for service X and had my txt, then that person would be able to know the password of any other service that shared the hint. **But** I only shared passwords between "low importance" services (random forums, etc).

Did the google accounts share passwords with other sites? Only the ``jfisanotti`` one, not the others. Mostly because I didn't care a lot about it.

Also, when writting down the ``franciswpritchard`` account, I made an important mistake: I mixed up the account vs password parts, and end up writting this:

``google.com: windmill2 / pr``

What is that? ``windmill2`` is the **password**, and ``pr`` as a hint for what I wrongly thought was the password (remember, I hadn't used that account in 2 years. I even thought of abandoning it). So yes, the txt file had the password in clear text, but **not the account name**.


2016, Sep 1
===========

I decided to start using a real password manager. No specific reason for it, just felt it was about time.

I switched to:

* Pass (command line open source program) to store passwords in my laptop, encrypting them with a **brand new password protected gpg key** (old one marked as deprecated).
* Encrypted data files stored in a new private git repo at bitbucket.org.
* Password Storage (open source) app in Android to clone the repo and be able to access the passwords on my phone.
* OpenKeychain (open source) app in Android to store the private gpg key, to be able to decrypt the passwords on the phone.

Worth mentioning: 

* My phone is a nexus 5x, running the official rom from google.
* The password I used for the new gpg key was new, unseen and not written down anywhere.

The problem with how I wrote down the ``franciswpritchard`` account was migrated to this new method too: ``windmill2`` was in clear text, while the contents of the encrypted file had ``pritchard`` (the wrongly remembered "password"). Still no ``franciswpritchard`` stored anywhere.


2016, Sep 2
===========

Email from google regarding the ``jfisanotti`` account: "Someone has your password".

.. code::

    ...
    Hi Juan Pedro,
    Someone just used your password to try to sign in to your Google Account jfisanotti@gmail.com, using an application such as an email client or mobile device.

    Details:
        Friday, September 2, 2016 11:50 AM (Eastern Daylight Time)
        United States*

    Google stopped this sign-in attempt, but you should review your recently used devices:
    ...


Mild paranoia. How could someone get that password? From the US? (I'm from Argentina). Some simple theories arised, and I changed passwords on every service that I cared of, just in case.

2016, Sep 19
============

Email from google regarding the ``franciswpritchard`` account: "Someone has your password".

.. code::
		
    ...
    Hi Francis Wendell,
    Someone just used your password to try to sign in to your Google Account franciswpritchard@gmail.com, using an application such as an email client or mobile device.

    Details:
        Monday, September 19, 2016 12:13 PM (Eastern Daylight Time)
        United States*

    Google stopped this sign-in attempt, but you should review your recently used devices:
    ...

Paranoia * 100. How could someone get **that** password? I can understand the previous one, but that one??? 

Theories
========

The access to ``jfisanotti`` can be explained in several different ways, all of them not that complex. But the real problem is the access to the second account, ``franciswpritchard``. That's a different story, and if the person behind both attacks is the same (assuming it's an attack), then it's using different methods for each account, and exploiting access to multiple sources of private data.

Theory 1: single password leak + shared passwords + leaked passwords data
-------------------------------------------------------------------------

I logged in to service X with ``fisadev@gmail.com``, and used the same password than the one for ``jfisanotti@gmail.com`` at google.

Someone has my password from service X (leaked, employee from X, whatever). Then the same person gains access to my old txt or my new encrypted pass data (either from local machine, bitbucket repo, phone apps), and is able to compare for other services with equal hints or equal encrypted data. And so, he discovers that ``jfisanotti`` at google has the same password than ``fisadev`` at service X (which he knows in clear text from service X).

This doesn't require the ability to decrypt the pass data, or know the passwords associated to the hints from the txt. Just by looking for repeated stuff, you could deduce the password is the same.

That would explain the access to the ``jfisanotti`` account, but not the ``franciswpritchard`` account. The name of the second one wasn't stored in the txt or pass data, and there is no way to "guess" it or relate it to me.

Theory 2: single password leak + shared passwords + real name
---------------------------------------------------------------

I logged in to service X with ``fisadev@gmail.com``, and used the same password than ``jfisanotti@gmail.com`` at google.

Someone has my password from service X, and is able to know my real name (many services have it). After trying to use the password to login at google using ``fisadev`` with no success, he could have guessed the ``jfisanotti`` account (by knowing my real name on service X) and try the password with it.

Again, this explains the access to ``jfisanotti``, but it's impossible to do this to gain access to ``franciswpritchard``.

Theory 3: apps leaking passwords
--------------------------------

Somewhat some of the apps (local of phone), leaked the password while displaying it (after being decrypted).

Once more, that would explain access to ``jfisanotti``, but not ``franciswpritchard``, as the account name it's not in the data. The data has a random word which happens to be the password for an account which isn't mentioned anywhere.

Theory 4: keylogger
-------------------

Someone has a keylogger in my local pc? or something taking screenshots on my phone?

That could explain the access to ``jfisanotti`` if it was recent, but to explain the access to ``franciswpritchard`` it must be a keylogger on my laptop 5 years ago. 5 years ago the data was gathered, and I'm only seeing access now? so close to the migration to the password managers, etc?

Theory 5: leaked passwords data + access to my inbox
----------------------------------------------------

If someone had the passwords data (txt or encrypted pass data) and access to my email inbox, then they could have seen the ``franciswpritchard`` account in the old emails, and they could have tried using the ``windmill2`` password with it.

Bear in mind that they would have to be able to guess that the ``windmill2`` "account name" in the passwords data was in fact a mistake, and it was a password. And then they would have to guess that the ``franciswpritchard`` account they saw in the old mails, was the account for that password. This still looks quite hard to achieve, unless you **really** want to have access to everything mine.

This method implies they have access to my inbox, which is scary. And at the same time, it doesn't explain the access to ``jfisanotti``, for that, they would need to use one of the other simpler methods.

Theory 6: google bug regarding their own pop client
---------------------------------------------------

Maybe google is wrongly flagging the access of it's own gmail internal pop3 client as suspicious activity? Remember, I added both accounts to the gmail web client of the main account, so internally, google is using pop3 to connect to both accounts. 

This would be rather strange.

Conclusions so far
==================

I'm either witnessing some weird google bug in its internal infrastructure, or someone really skilled is targetting me to get access to my accounts.

Motives for someone to target me? I wish I knew. The only thing that comes to my mind: I worked some years for a company that developed an electronic voting system used in Argentina (yes, that one on the news).

Not fun, not fun at all.
