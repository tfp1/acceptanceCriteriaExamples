## User Scenarios

* As a user, I cannot navigate to `/admin.`
* As a user, I cannot `create or update users`.
* As a user, I cannot `create or update iGroups`.
* As a user, I have delegated permissions to edit my own user record based on Company Settings Flags.

## Admin Scenarios

* As an admin, I can `create or update all users`.
* As an admin, I can `create or update all iGroups`.
* As an admin, I can move `users` between `iGroups`.
* As an admin, I can upload a CSV of `HRIS` or `iGroup` data.

Group Admin Scenarios

* As a group admin, I can read or update users that are members of an iGroup that I am a group admin of or its children
  * The users datatable should only display users I can administer
  * The people search on the datatable should only return users I can administer
    * What happens if a group admin is the admin of two iGroups within the same category (e.g. two locations)?
  * The group admin can create a user, but that user must be in at least one of the locations
  * What happens if a group admin is the admin of two iGroups of differing types (e.g. one location and one department)?
    * The group admin can create a user, but that user must be in at least one of the groups
* As a group admin, I cannot move a user out of all group I administer.
  * The UI should display a error modal (a hard stop on the action) that indicates the group admin will not be allowed to administer the user after the transaction.
* As a group admin, I cannot move a user into my group
  * API should block this action
  * The UI already covers scenario this because it does not allow the group admin to see users not in their groups
* As a group admin, I cannot create or update an iGroup.
  * Group admin should not see the groups, locations and departments sub-menu items
* As a group admin, I cannot import a CSV of HRIS or iGroup data.
  * Hide or disable the Import/Update users button in the header
* As a group admin, I can export users to CSV

## Decisions

* Can a group admin run a CSV?
  * What if there’s a user not from that group admin's groups?
  * Group admin can import a CSV without restrictions in legacy apps
  * Is this something we want to permit in KW?
* `CTO` decided we will not allow a Group Admin to run a CSV

