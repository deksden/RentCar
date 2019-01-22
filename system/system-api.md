# System API

### ACL routes:

> GET /acl/user/:userId

Returns list of ACL objects defined in system for specified user

> POST /acl/user/:userId

Define ACL object with permissions for specified users

> GET /acl/user-group/:groupId

Returns list of ACLs for specified user group

> POST /acl/user-group/:groupId

Define ACLs for user group

### Auth routes:

> POST /auth/signup

Create new user profile in system

> POST /auth/login
 
Login via email/password

> GET /auth/signup (html) ?invite=code

Return signup page with hidden field "invite"
 
### Invite routes:
 
> POST /auth/invite

Create invite for new user
  
> GET /auth/invite

Return list of invites in system

> GET /auth/invite/:id

Get specified invite

> DELETE /auth/invite:id
  
Delete specified invite

> POST /auth/invite/:id/send

Send invite for specified user

### Login routes

> GET /login

Return list of active login (sessions) for current system

> GET /login/:id 

Return data about active login

> DELETE /login/:id

Remove active login session (force specified user session to logout)

### UserGroup router

> GET /user-group

Return list of user groups 

> POST /user-group

Create new user group

> GET /user-group/:id

Return info about specified user group

> PUT /user-group/:id

Update info about user group

> DELETE /user-group/:id

Delete user group from system

> GET /user-group/:id/users

List users in this usergroup

> POST /user-group/:id/users

Add user to usergroup

> DELETE /user-group/:id/users/:userId

Delete single user from group

> DELETE /user-group/:id/users

Delete specified list of users from group

### User router:

> GET /user

Return list of users in system 

> POST /user 

Add user tp system

> GET /user/:userId

Get user profile

> PUT /user/:userId

Update user profile

### User permissions routes:

> GET /user/:userId/permissions

List all defined permission for this user

> POST /user/:userId/permissions

Create new permission for specified user

### User login routes:

> GET /user/:userId/logins

List logins of this user

> GET /user/:userId/logins/:loginId

Get details of logins for specified user

> DELETE /user/:userId/logins/:loginId

Manually log-off specified user

### Me routes:

> GET /me

Return profile of currently logged-in user

> GET /me/permissions

List ACL permissions for currently logged-in user
