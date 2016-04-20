# OAuth2 Basics
Specs: https://tools.ietf.org/html/rfc6749


### Basics

- Steps involved in authorizing a Web app:
1. get AUTH code
2. exchange AUTH for ACCESS (use client secret, ask for consent)
3. receive REFRESH and ACCESS
Example: app keeps using access until expiration, then refresh (just redirect to oauth, no consent needed)

- Very interesting post about implementation
http://stackoverflow.com/q/11357176


### Best practices

https://rnd.feide.no/2012/04/19/best-practice-for-dealing-with-oauth-2-0-token-expiration-at-the-consumer/



### Use cases:
http://stackoverflow.com/a/19296349

> Mobile scenario
>
> On the mobile, there are a couple of scenarios that I know of:

>     1. Store clientid/secret on the device and have the device orchestrate obtaining access to the user's resources.

>     2. Use a backend app server to hold the clientid/secret and have it do the orchestration. Use the access_token as a kind of session key and pass it between the client and the app server.

> Comparing 1 and 2

> In 1) Once you have clientid/secret on the device they aren't secret any more. Anyone can decompile and then start acting as though they are you, with the permission of the user of course. The access_token and refresh_token are also in memory and could be accessed on a compromised device which means someone could act as your app without the user giving their credentials. In this scenario the length of the access_token makes no difference to the hackability since refresh_token is in the same place as access_token. In 2) the clientid/secret nor the refresh token are compromised. Here the length of the access_token expiry determines how long a hacker could access the users resources, should they get hold of it.

### Expiration settings

> At the time the consumer receives an access token response, the validity period of the token can be decided using any of these indicators, in the given order:

>    The expires_in property in the access token response. This value is reccomended to be included by the provider.
>    A special scope indicating validity period of the token. In example a scope offline may be defined to mean infinite validity.
>    A default value of validity period, typically per provider.
>    A default value of the consumer implementation, if none of the above is given. It may be infinite, or it may be a limited period.


### Refreshing policies
http://blog.cloud-elements.com/oauth-2-0-access-refresh-token-guide

### Experience with refresh tokens
https://www.box.com/blog/oauth2-update-longer-lived-refresh-tokens/


> Longer lived refresh tokens

> We often get asked about how Box decided to build our OAuth2 implementation. Whenever we build new services, we have to strike the right balance between ensuring enterprise grade security while also making our platform simple and easy to build on for developers. Balancing those two desires can sometimes come into conflict, but we always try to strike the right balance. We wanted to share our thought process on our OAuth2 implementation so far, and share a couple of changes we just took live that should make things a lot better for developers that were hitting some edge cases.

> As you probably know, OAuth2 is a pretty wide specification. It allows for a lot of leeway in the implementation. Some of that leeway causes challenges for application developers.

> oauth2Refresh Tokens (RTs) is the primary area where OAuth2 gets interesting, and potentially controversial. Box has decided to use RTs that are good for a single token refresh. RTs are issued alongside an access token (AT). The two tokens always come in a pair. The AT is good for about an hour, while the RT is good for 60 days. We originally made the RT only good for 14 days, but there were a lot of applications that use Box that are only used about once a month. We took feedback from our customers, our security consultants, and our developer partners, and we changed several things to make rotating RT's easier to work with.

>    We extended the RTs to last 60 days instead of 14
>    If a new RT is lost in transit, you can still use the old one to get a new one. Instead of killing the old RT as soon as we issue the new one, we now wait until you use the new AT. So if for some reason when you try to get a new RT, or if saving the RT to your keystore fails, you can call the refresh grant again, and we will give you the RT/AT pair again. We do recommend storing RT's in a secure keystore, since if you lose them, your users will have to re-authenticate your application with Box.
>    We also now check the AT validity based on the start-time of upload calls instead of when the upload completes. Uploads can take a while for large file or slow connections. So instead of checking the validity of the AT at the end of the upload, we look at the AT you sent, and check its validity at when you started your upload. You might already have used the RT to get a new AT for some other operations, but your upload that you put on a background thread will still succeed.
>    And finally, ATs last a little longer than an hour. We actually randomize the duration a little, so we encourage you to not just set a 60 minute timer in your code.

> For handling RTs, there are 2 options. You can either refresh your tokens a few minutes before they expire, so you hopefully never make an API call with an expired token. Or you can just wait until you get the error response that your token expired and do the refresh then.

