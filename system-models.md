## System models

### ACL:

ACL - Access control list: таблица для определения прав доступа к различным моделям системным / прикладным
 
* id: ACL identifier
* permissions: [] array of permissions:
* permission: permission name, like "read", "write"
    * users: list of users that have this permission of this kind:
        * id: userId
        * kind: permission, one of ALLOW/DENY
    * userGroups: list of groups that have this permission of this kind

### Invite:

Приглашения для новых пользователей системы

 * id : uuid
 * expireAt : date
 * registeredUser -> User.id
 * createdBy -> User.id
 * disabled : boolean
 * email: invited to this email
 * assignUserGroups [ UserGroup ]: array of user group Ids to assign for user created via this invite

### Login:

Сеанс пользователя системы

* id: login identifier, UUID
* userId -> User.id: user, associated with this login
* createdAt: date of logging-in
* ip: IP address of user's client
* disabled: if this login is disabled
  
### User:

Пользователь системы

* id: user identifier, UUID
* name: user's full name
* email: email, that user choose for registering
* password: hashed password
* invitedBy: -> User.id: user that created invite
* inviteDate: date of invite
* inviteId -> Invite.id: link to invite
* disabled: if user account is disabled

### UserGroup:

Группа пользователей системы

* id: identifier, UUID
* name: group caption
* systemType: null, Admin, Guest, LoggedIn
* users [User]: members of this group

System types:

* Null: all members are defined by users, not system
* Admin: system type for admin user (first user in system at least) - manually can be added other user accounts
* Guest: not authenticated user
* LoggedIn: Authenticated users (any - admin, other users)

### ACL priorities

If no ACL is defined, then user access will be DENY.

Lowerst priority is for group permissions

User-specific permissions have priority over group permissions.
