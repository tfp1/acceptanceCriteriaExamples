# Description

As a platform admin in KW, I can supply a subdomain for a company and yei_rails returns:
- An array of group_type.id
- An array of groups.id
- An array of users.id

As kw, I can process YEI entity IDs and create the corollary kw entities
- Parent Groups
- Child Groups
- Users (including country)
- Departments
- Locations
- Group assignments
- Manager assignments & hierarchy

# User Journey
Platform Admin navigates to a superadmin page
UI provides a form for:
Company subdomain
- String name for the `group_type` object(s) that should be mapped to departments. Can be `null`
- String name for the `group_type` object(s) that should be mapped to locations. Can be `null`
- String name for the `group_type` object(s) that contain countries. Can be `null`
- Platform Admin submits the form

# Implementation Notes
- Ingestion process needs to be run parallelized. 
- May need to be able to write lock KW user table during this process. pending Lead Engineer's strategy decision

# Application Processing

## Scenario 1: No departments, no locations, no countries

**Given** the Platform Admin has navigated to the **Migrate YEI Company to KW** Page  
_and_ the Platform Admin properly supplied a `subdomain`  
_and_ the Platform admin did not provide values for `departments`, `locations`, and `countries`  
**When** the Platform Admin submits the form  
**Then** the Migration App should fetch the `yei_rails.company.ObjectId` for the `subdomain` provided  
_and_ then fetch all `yei_rails.group_types` where the `yei_rails.group_type.companyId == yei_rails.company.ObjectId`  
_and_ then create a `kw.group` for each with `kw.group.GroupType == affinity` and the field mappings listed in table 1 below  
_and_ then fetch all `yei_rails.groups` where the `yei_rails.groups.companyId == yei_rails.company.ObjectId`  
_and_ then create a `kw.group` for each  
_where_ `kw.group.GroupType == affinity`  
_and_ the `kw.group.parent == the kw.group.id` for the `group_type`  
_and_ then fetch all `yei_rails.users._id` where the `yei_rails.users.companyId == yei_rails.company.ObjectId`  
_and_ then store all `yei_rails.users._id` in a staging table with a status  
_and_ then begin to iterate through all records in the staging table  
_and_ then create a `kw.user` for each with the field mappings available on the YEI Mapping tab in `google drive`  
_and_ set `kw.user.country == yei_rails.user.user_setting.shipping_address.country`  
_and_ if `yei_rails.user.user_setting.shipping_address.country` is empty or does not exist, fall back to `yei_rails.company.contact_address.country`  
_and_ set the kw.user management hierarchy
_and_ set the kw.user group assignments
