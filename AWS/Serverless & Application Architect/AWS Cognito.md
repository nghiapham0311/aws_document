![[AWS Cognito.png]]
Cognito provides 2 main pieces of functionality both are very different, but both are essential to understand. The service as a whole provides authentication, authorization and user management for web and mobile application.

Authentication means to log in to verify credentials, authorization means to manage access to services, and user management means to allow the creation and management of a serverless user database.

There are 2 parts of Cognito, *user pools* and *identity pools*. The end goal of a user pool is to allow you to sign in, and if successful you get a JSON Web Token known as a JWT. This JWT can be used for authentication with applications, certain AWS products such as API Gateway can even accept it directly. But, this is crucial to understand, most AWS services cannot use JWT’s. To access most AWS services, you need actual AWS credentials.

*User pools* **do not grant access to AWS services**, their job is to *control sign in and deliver a JWT*. So they do things like sign up and sign in services, they also provide a builtin customizable web user interface to sign in users.

They provide certain security features, such as multifactor authentication. They check for compromised credentials and they offer account takeover protection, as well as phone and email verification.

You can also implement customized workflows, and user migration by using Lambda triggers.

Where it gets confusing is that user pools, as well as allowing sign in from builtin users, they also allow social sign in using identities provided by Facebook, Google, Amazon, Apple, as well as offering sign in services using other identity types, such as SAML identity providers.

> **The important thing to understand is this is about offering a joined up user management experience. At no point can user pool be used to directly access most AWS resources. When you think of user pools, imagine a db of users which can include external identities. They sign in and they get a JWT, that’s it**

The aim of an *identity pool* is to **exchange a type of external identity for a set of temporary AWS credentials which can then be used to access AWS resources**.

One option is authenticated identities which can be used to offer guest access to AWS resources. Imagine you have a mobile application and want to allow high scores to be stored in a leaderboard which is hosted using DynamoDB. You want to offer this without a user having to sign up, and this is one way to do that.

*Identity pools* can also be **used to swap an external identity for temporary AWS credentials**, and this means things like *Google Identity, Facebook, Twitter, SAML 2.0* for corporate logins and even user pool identities. *So from an identity pool perspective, user pools are just treated as another form of identity.*

Now all of these are examples of authenticated identities. If another identity provider which we trust say that they have authenticated successfully, then identity pools will exchange that identity for temporary AWS credentials.

| User pools                                                                                                                                                                            | Identity pools                                                                                                                                                                                                                                                                                                    |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| User pools are about offering a joined up sign up, or sign in experience with user directory and profile management services, so it’s about login and about managing user identities. | Identity pools are about swapping either an unauthenticated, or authenticated identity for AWS credentials. And one possible type of identity is actually a user pool identity, and this is reason why these 2 different components of Cognito are often difficult to separate because they can operate together. |
|                                                                                                                                                                                       | Identity pools work by assuming an IAM role on behalf of the identity. That assumption generates temporary credentials, and they’re provided back in return in most cases to a mobile or web application. These IAM roles are configured within identity pools.                                                   |
#### User pools
![[AWS Cognito-1.png]]
Remember user pools are about user managements, sign in, sign up and anything associated with that process.

> These are known as JWT’s and API gateway is capable of accepting JWT’s for authentication.

A user pool is a collection of identities of users, it’s used to allow sign up and sign in both for internal users and social sign in. This tokens, which are generated as a result can be used for self-managed systems, and the tokens can be used to authenticate for API gateway.

> **This is the single biggest thing to remember about Cognito, these tokens cannot be used to access AWS resources**

#### Identity Pools
![[AWS Cognito-2.png]]
So we have the same web and mobile application, this time though, we aren’t using user pools, we’re allowing customers to log in directly using external identities.

How this works is as follows, we start with a collection of supported external identities and these include the same social identities in the user pools.

Our application allows users to sign in with any of those external identities. So when they click on a sign in button within our application, they’re directed at an external ID provider sign in page. You might have experienced one of these before, this is an example of the sign in with a Google page.

After a customer authenticates with their Google credentials which it’s worth pointing out that we never have access to because this sign in takes place on the external identity provider. In any case, we receive a Google token as a result, but it could be a Facebook token, an Amazon token or whatever external ID provider is used. Crucially, it can be one of many different types. If we want to support many different external identity providers, then we need to configure that support. But now that we have this external ID provider token, this proves that a user has logged in with an external ID provider. This token can’t be used to access AWS resources, and that’s where identity pools come in handy.

Our application takes this token, and passes it to an identity pool that we’ve configured. We’ve configured this to support every external identity that we want to allow logins from. **This is a key thing to keep in mid if we want to support 5 external ID providers, we need 5 different configurations, 5 different types of token to be supported.**

What happens next is that Cognito is configured with roles at least one for authenticated identities, and one for unauthenticated or guest identities. In this case, we have an authenticated identity, the Google token and so on our behalf, Cognito assumes a role and generates temporary AWS credentials which are then passed back to the application.

The application can then use these credentials to access AWS resources. Once they expire, the application renews them again using Cognito and the process continues.

The permissions the application has are based on the roles permissions, at no point does the application store any credentials within code, or any credentials permanently. So the process is that an external identity provider authenticates a user. Cognito identity pools swap the external ID token for temporary credentials and these are used to authorize access to AWS resources.

You can use user pools and identity pools together to fix one small lingering problem. With this configuration, your application has to be able to deal with many different ID tokens from many different external providers.
#### User & Identity Pools
![[AWS Cognito-3.png]]
One option is that we could use user pools to handle the many different types of identity, and then we can use identity pools to swap the Cognito user pool token for AWS credentials. The swapping of any external ID provider token for AWS credentials is known as web identity federation.

We start with a user pool, and this is configured to support external identities and its internal store of users. Whatever is used, whichever identity type is used to log in, the identity that’s authenticated is now a Cognito user pool user. So there’s only one type of token which is generated where the sign in is using internal users or social sign in. This is the user pool token or JWT.

By using a user pool, we’ve abstracted away from all of the configuration of many different external ID providers. We have conceptually one user store to manage one set of user profiles all provided via a Cognito user pool. So if you log in with a user pool user, or if you log in via the user pool, but using Google credentials, the outcome is the same.

A user pool token is returned to the application, so the user pool simplifies the management of identity tokens.

Next the application can pass this user pool token into an identity pool. This assumes an IAM role defined in the identity pool which generates temporary AWS credentials. These temporary credentials are returned to the application.

The benefit to this approach is that the identity pool need only be configured with a single external identity provider, the user pool, but otherwise the process is the same as using an identity pool directly just with less admin overhead. The application can then use those AWS credentials to access AWS resources.

In summary, user pools manage user signup and user sign in either internal or using social logins. What you get as a result is a user pool token also known as JWT and that is the output of any form of sign in using user pools.

Identity pools swap external identity tokens for AWS credentials, this process is called federation. External identity tokens can be direct external identity token such as Google, Amazon, Facebook and many others, or they can be user pool tokens which can themselves represent external ID logins. Once an application uses an identity pool to gain access to temporary AWS credentials, it can access AWS resources. Now this process allows for a near unlimited number of users. An unlimited is much more than the 5,000 IAM user limit which means this is great for web scale applications.

[[AWS Glue]]