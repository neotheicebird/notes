ELI5:

Oauth2.0 - A security standard in which the user gives an application permission to get data from another application. For example, I can signup into Dev.to using my github account, during which Dev.to redirects to Github, asks user to input credentials if not logged in, shows a screen with what information would be accessed and upon accept, shared specified information

https://www.youtube.com/watch?v=t18YB3xDfXI

Example use cases of OAuth:

1. Share email contacts: You signup for a service called "TerriblePunOfTheDay" and you like the puns shown each day, you want to share it will all your email contacts, but you dont want to do it manually. Lets say TPOTD provides a OAuth shortcut, we can click on "share with contacts > select email provider (Say GMail) > redirect to domain of email provider (in a tab or a pop up) > enter credentials if logged out > show a screen listing information to be shared and Allow/Deny buttons > On click redirect to TPOTD and show Success/Failure message"




Terminology:

1. Resource Owner - You, the user, who owns the information in a service
2. Client - The application that requests for information, in above example TPOTD
3. Authorization Server - Application that knows the Resource Owner (GMail), where the resource owner has an account
4. Resource Server - Is the API the client wants to use on behalf of the Resource Owner. This in some cases are same as the Authorization Server. For a case in which our app uses Auth0 for authorization, if the app exposes an OAuth service, then Auth0 is the Authorization server, and the apps' API is the Resource Server. In this 2nd case, the Auth server is a 3rd party service the Resource server trusts.
5. Redirect URI - The URL which the Authorization server redirects the resource owner back to, after granting permissions to the client. Also refered to as callback URL
6. Response type - The type of information the client expects to receive, the common type is called code. The client expects to receive an Authorization code
7. Scope - Granular permissions the client wants
8. Consent - The authorization server takes the scopes the client is requesting and verifies with the resource owner whether they want to grant those permissions
9. Client ID - unique ID used to identify the client with the authorization server
10. Client Secret - A secret password that only the client and auth server know, which allows them to securely share information between them
11. Authorization Code - A short lived temporary code that Auth server sends back to the client. The client then sends the auth code along with the client secret in exchange for access token
12. Access Token - A key used to perform actions on the resource server on the resource owner's behalf, typically a JWT token

Flow - Based on above example

1. You, the `Resource Owner` wants to allow TPOTD, the `Client` to send messages to all your GMail contacts
2. The client redirects the browser to the `Auth Server` which in this case is GMail's Authorization server with the following information
	1. Client ID
	2. Redirect URI
	3. Response Type
	4. Scope
3. The authorization server verifies the resource owner and if necessary prompts for a login
4. The auth server then presents with a `Consent` form where the resource owner has an opportunity to Allow or Deny the permissions
5. The Authorization server redirects using the `Redirect URI` along with a temporary `Authorization Code`
6. The client then contacts the Auth server directly with the Auth code, client id and client secret
7. The Auth server verifies the data and provides the client with an access token
8. The access token is a value the client doesn't understand, its just jibberish, however it can send the access token and make the resource server perform certain actions. The client says "Here is an access token, give me all contacts of the resource owner whom this access token identifies"
9. The resource server verifies the access token and if valid responds with the contacts requested
10. For all this to work, while development of TPOTD, they and the Auth Server established a relationship. The auth server should have generated a Client ID and Client secret for TPOTD.

OpenID Connect (OIDC):

OAuth2.0 is designed only for authorization, OAuth is designed to give client a key, the key is useful but doesn't tell the client anything about you.

OIDC is a thin layer that sits on top of OAuth that adds functionality about login and profile information about the person who is logged in. Instead of a key its like providing a Badge, in which basic information about who you are is present and the badge can be used to perform other actions on the resource server. 

OAuth enables authorization from one app to another

OIDC allows a client to establish a login session, often refered to as Authentication. When an Auth server supports OIDC it is sometimes called Identity Provider, since it provides information about the resource owner back to the client.

OIDC enables scenario when one login can be used across multiple applications, called SSO (Single Sign On). OIDC is like using an ATM, its job is to access data such as bank balance, deposit/withdraw money. Your debit card is the access token issued by the bank, it not only gives the ATM access to your account (the authorization), it also has basic information about who you are (name), and authentication information like when the card expires, who issued the card, banks phone number. The ATM cannot work without bank infrastructure, similarly OIDC cannot work without OAuth Framework.

## Summary

A OIDC looks the same as OAuth, the only differences are:

1. In the initial request a scope of `openid` is used
2. The auth server goes through the same set of steps and issues an Authorization Code using the resource owner's browser back to the client
3. The Key difference here is that when the client exchanges Authorization code, client id and client secret with the Auth server, the client recieves `Access Token` along with a new `ID Token`. The access token is a value the client doesn't understand, the ID token is very different. Its a specific string known as JWTs. It may look like Jibberish to us, but the client can extract information such as `issued by, issued_at, expiration, user_id, email` etc. The data inside ID Token are called claims. The client can also tell if anyone had tried to tamper with the JWT. The client can also ask for more claims like the email id using the access token.
