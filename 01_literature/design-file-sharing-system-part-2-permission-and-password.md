---
tags:
  - directory-structure
  - file-management
  - file-system
authors:
  - datpv
date: 2024-08-09
title: "Design file-sharing system - Part 1: Permission & Password"
description: "In this part, I will discuss how I handle the logic and design the data model for the following features: setting permissions, sharing files for public access, and setting a password for a file."
---
In this part, I will discuss how I handle the logic and design the data model for the following features: setting permissions, sharing files for public access, and setting a password for a file. Refer to the diagram below to understand how permissions work in this system.

![Untitled](assets/design-file-sharing-system_3.webp)

## Permissions

### Functional requirements
- Each file will be set permission, and when users interact with it, they must satisfy it.
- Files will inherit permission from the parent by default.
- There are 2 scopes, allow permission for all members in workspace or invite only.

### Data model

![Untitled](assets/design-file-sharing-system_4.webp)

**Access Levels field:**
- WORKSPACE: Applies to all project members
- INVITE: Specific to invited users

**Role field:**
Show the permission for invitee or project's members in this record

- FULL_ACCESS: Can perform all operations
- EDITOR: Can view and edit
- VIEWER: Can only view
- NO_ACCESS: Cannot access the asset
- INHERIT: Inherit from parent folder

**Public Role field:**
Showing the role for guest user when access the file
- EDITOR: Can view and edit
- VIEWER: Can only view

**Child Role field:**
Show the permission workspace members and only has value
- FULL_ACCESS: Can perform all operations
- EDITOR: Can view and edit
- VIEWER: Can only view
- NO_ACCESS: Cannot access the asset
- INHERIT: Inherit from parent folder

### Logic

**Permission Hierarchy**
Permissions are checked in this order:
- Direct user permission on the asset
- Main permission's childRole (default for project members)
- Inherited permission from parent directories

**Permission record rules:**
- Each asset has one and only one main permission record(contain `childRole` value) which is show the permission for the project and public level.
- If the access role is INHERIT then the role is inherited from the parent folder.
- The role for an specific email is also created in the permission table but `childRole` = null

## Sharing file

### Functional requirement:

- **Setting Public Access**
    - Asset owners can set the `publicRole` in the permission table.
    - The role can be either `VIEW,` `EDIT` or `NULL`.
- **Accessing Public Assets**
    - When a guest user attempts to access a public asset, the system checks the `publicRole`.
    - Based on the `publicRole`, the user is granted view or edit permissions.
    - If the user is not logged in, they are restricted to view-only access.

![Untitled](assets/design-file-sharing-system_5.webp)

### Key Components

**`publicRole` in Permission Table**

- **publicRole:** Defines the level of access for public users.
    - `VIEW`: Allows public users to view the asset.
    - `EDIT`: Allows public users to edit the asset.
    - `NULL`: Null value means the files is not allow public asset

## Password Protection Feature

### Functional requirement

The password protection feature allows users to set a password on their assets to restrict access.  If an asset has a password, any user attempting to access the file must provide the correct password.

### Key Components

**`passwordHash` in Permission Table**
- **passwordHash:** Stores the hashed password for the asset.
- When this field has a value, password protection is enabled for the asset.
- The password is securely hashed before being stored to ensure security.

### Workflow

To make it easy to understand, I will show the workflow for getting file details. This process also applies to other features like updating and setting permissions.

The idea is that the user will get an `asset-file-token` by using `GET /assets/:id/login`. Then, add it to the header for authorization when calling `GET /assets/:id` to get detailed info.

![Untitled](assets/design-file-sharing-system_6.webp)

## Conclusion

In conclusion, this system implements a robust and flexible permissions model for file management. It covers essential features such as upload, manage, setting permissions, public file sharing, and password protection. The design allows for granular control over access levels, inheritance of permissions, and secure sharing options. This comprehensive approach ensures that users can effectively manage their files while maintaining appropriate levels of security and collaboration within the workspace.