### Description

We need to support two different models for group leadership. `Owners` manage performance management functions, whereas `Admins` have a more traditional administrative function: creating/editing users, assigning points, etc.

#### Examples

1. David is the Trader Department Owner. He sets goals for the department.
2. David is NOT the Trader Department Admin. He cannot, for example, adjust points for members of that Department
3. Betsy is the Sales Department Owner. She sets goals
4. Betsy is also the Sales Department Admin. She creates Behavior Bonuses for her team

### Basic AC

1. For all Groups, Locations and Departments
  - Add Owner, which may be one or many user ids
  - Add Administrator, which may be one or many user ids
2. Write the `admin` back to `yei_rails.groups.group_admin_ids`
3. Write the `owner` back to hgApp Locations and Departments  

### Technical Implementation Plan

1. Create new database entities for
   - department_owners
   - department_admins
   - group_owners
   - group_admins
   - location_owners
   - location_admins
2. Update GQL mutator and resolver
3. Refactor the DTO to include `owner` and `admin
4. ~~Update groups writeback to include Administrator(s)~~
   - After investigation, we do not need to writeback `Group_Admin` to yei because all their functions are tied to User Management pages that are replaced by kwUsers which already knows who this user can administer
   - We do have an issue with GRS Reward Moderation that is unaddressed
   - Notify the Data Insights team of the change, requiring an update to their Group Admin Reporting 
5. Update group CSV import format
   - Add Admin column
   - Add Removed Admins column
   - Add Removed Owners column
   - Accept quote escaped, comma separated lists

### *_owner & *_admin schema

| Field  | Description                         |
| ------ | ----------------------------------- |
| id     | pk                                  |
| owner  | fk: `kw.directory.person.id`        |
| status | enum: `{active, pending, archived}` |
