
Authentication
==============

Authentication component provides a service to manage authentication mechanism on your site. It's meant for controller actions only, so in order to activate it, you have to inherit `\Krystal\Application\Controller\AbstractAuthAwareController` rather than default `\Krystal\Application\Controller\AbstractController`. Usually you would want to make all controllers protected that belong to administration area of your site.

Once you inherit `\Krystal\Application\Controller\AbstractAuthAwareController` controller, you have to implement 4 protected methods in a descendant:

## getAuthService()

Must return a service (we'll discuss it below)

## onSuccess()

This method will be invoked automatically, once user enters valid credentials.

## onFailure()

This method will be invoked automatically if not-logged-in user tried to access protected area (i.e protected controller actions).

## onNoRights()

This method will be invoked if when user tried to access an area he has no rights to.

# Writing the service

The service must implement `\Krystal\Authentication\UserAuthServiceInterface`:

## getId()

Returns an id from storage.

## getRole()

Returns a role from storage.

## authenticate($login, $password, $remember, $hash = true)

Performs an authentication. When implementing this method you would query a database against provided data.

## logout()

Logouts a user.

## isLoggedIn()

Determines whether a user is already logged in.

# RBAC

@TODO
