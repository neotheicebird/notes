https://aws.amazon.com/blogs/mobile/customizing-your-user-pool-authentication-flow/

## Introduction

Cognito supports modern auth flows to verify user identity. Along with passwords, we can present one or more challenges to the user, like CAPTCHA. We can customize the authentication flow by incorporating additional auth methods and using server driven authentication (lambda triggers).

1. We generalize authentication with 2 common steps using 2 new APIs `InitiateAuth` and `RespondToAuthChallenge`. In this flow, an user authenticates by answering successive challenges until authentication either fails or suceeds with tokens being issued. These are the new APIs, older ways of authentication still exist, but are legacy.
2. Cognito provides the ability to customize auth flows with AWS Lambda triggers. These triggers issue and verify their own challenges as part of auth flow (say to implement passwordless login)

## `InitiateAuth` and `RespondToAuthChallenge` APIs

1. `InitiateAuth`: Kicks off the auth flow. We can pass initial auth parameters and tell cognito how we are trying to authenticate (as in what `AuthFlow`). This also triggers a pre-authentication lambda.

`AuthFlow:String`  can take values like `USER_SRP_AUTH`, `REFRESH_TOKEN_AUTH`,` CUSTOM_AUTH`, `ADMIN_NO_SRP_AUTH`. 

```
InitiateAuth(
  AuthFlow: string,
  cliendId: string,
  AuthParameter: dict,
  clientMetadata: dict
)
```

`AuthParameter` A set of key-val pairs with all necessary inputs to initiate auth. like `username:neotheicebird`

SRP: Secure Remote Password is a zero-knowledge proof protocol, its a subset of PAGE (password-authenticated key agreement).

ELI5 SRP: During registration, the client uses username, password to create a bunch of values like `verifier` and `salt` which are transmitted to the server. The server doesnt save password or the hash of the password. The server doesn't know of the password. So how will we authenticate? during auth, the client uses username, password, salt sent by server to derive `x`. Server on the other side indepenantly calculates `x` using `verifier`, `username` a public value `A` sent by client (this explanation is not accurate, but the point is we have to trust the magic math here).

**This is not token generation protocol, rather it creates a session of sorts and lets explore this in future**

`CUSTOM_AUTH` is what we are generally interested in. It supports many auth flows using a series of challenge-response cycles.

If no further challenges are to be presented, a successful call will issue tokens. 

2. 
```
RespondToAuthChallenge(
  ChallengeName: string,
  Session: string,
  ChallengeResponses: dict,
  ClientId: string,
)
```

`Session` is an encrypted session state (object encrypted as string) received by client in previous step, sent back as in for the next step. Cannot be replayed and expires in 3 mins.

`ChallengeResponses` a dict of string: string. Contains key value pairs of all params needed to respond to the challenge. like `captchaAnswer:123ABC`

A successful call can lead to issue of tokens or we might have to go to next challenge step.

If last step then we get in response `AuthenticationResult` which contains ID, Access, Refresh tokens.

Else, we get fields like `ChallengName` (name of next challenge), `Session` encrypted session state to be passed in next call to RespondToAuthChallenge, `ChallengeParameters` key-vals containing necessary info to prompt user for the returned challenge e.g `captchaUrl=https://...`


## CUSTOM_AUTH Lambda triggers:

1. `Define Auth Challenge Lambda trigger` - Analyze the challenges a user has answered so far and succeed the auth (generate tokens), fail auth, or prompt new challenge
2. `Create Auth Challenge Lambda trigger` - generate a challenge that consists of a set of params (like CAPTCHA image url and valid ansers that can be used when the challenge is answered) We can store answers server side for added security
3. `Verify Auth Challenge Lambda trigger` - Verify if the answer provided by user is valid


## Cookies vs LocalStorage

Go for cookies, they are secure. Also, only when storing non-essential cookies do we need user consent, since storing tokens is essential, we need not ask for consent (while we just need to let the user know what cookies are being used and why): https://gdpr.eu/cookies/

https://supertokens.io/blog/cookies-vs-localstorage-for-sessions-everything-you-need-to-know#:~:text=Size%20constraints%3A%20Cookies%20have%20a,when%20using%20JWTs%5B1%5D.&text=2.,and%20removed%20by%20the%20browser.

Why not localStorage?

LocalStorage can be prone to XSS attack, as it is accesible through Javascript. This means that a 3rd party code (libraries, plugins, google analytics) can run code on your client side that accesses this info, which is bad.

Why cookies isn't prone to XSS attack?

Cookies can be made secure by setting them as 'httpOnly' which means that they are sent automatically, along with every http request to "your" domain (server's domain). 



## Setting up Cognito manually - Notes

1. To get confirmation emails (now with verification code only. Verification URL doesn't seem to be generated):
	1. Go to MFA and Verification tab
	2. Under `## Which attributes do you want to verify?` choose `Email`. That's it.
2. It is not best practice to let Cognito send emails on its own in production. Instead we need to setup AWS SES to send custom emails. This also makes it easy to customize html based emails. To do this, go to `Cognito panel > Message Customizations` and setup SES information. We can change the verification type from `Code` to `Link` here.
3. SMS sent by Cognito uses Amazon SNS. Check this link for how SNS is used to send SMS by AWS: https://docs.aws.amazon.com/sns/latest/dg/sns-mobile-phone-number-as-subscriber.html

