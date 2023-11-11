+++ 
title = "Permission Bitmasking : Compressing and Decompressing User Permissions in JavaScript"
description = "User permissions are an essential part of any application's security model. One major use case for compressing and decompressing user permissions is to include the compressed permissions in an authentication token. When a user logs in or obtains an access token, you can attach their permissions to the token."
date = "2023-11-10"
author = "Kamesh Sethupathi"
tags = ["algorithm", "jwt", "security", "javascript", "privacy"]
draft = true
+++

User permissions are an essential part of any application's security model. One major use case for compressing and decompressing user permissions is to include the compressed permissions in an authentication token. When a user logs in or obtains an access token, you can attach their permissions to the token.

```
Compressed: 3963
Decompressed: 
{
  forms: [1, 2, null, 4],
  users: [5, 6, 7, null],
  archive: [9, 10, 11, 12],
}
```

### Understanding the Problem

Let's assume you have a set of resources and a list of permissions for each resource. For example:

```js
const permissions = {
  forms: [1, 2, 3, 4], // Resource: [get, create, update, delete]
  users: [5, 6, 7, 8], 
  archive: [9, 10, 11, 12]
}
```

You also have user-specific permissions:

```js
const userPermissions = {
  forms: [1, 2, null, 4],
  users: [5, 6, 7, null],
  archive: [9, 10, 11, 12],
};
```

Now, let's discuss how we can compress and decompress these permissions using bitwise operations.

### Compressing User Permissions

- We use the bitwise OR operator ('|') to set the corresponding bits in the compressed value. 
- If a permission is null, we don't set its bit. The resulting number represents the user's compressed permissions.
- The compress function iterates through the user permissions, sets the corresponding bits for each permission, and returns the compressed value.

```js
function compress(permissionsObject) {
  let compressedValue = 0;

  for (const resource in permissionsObject) {
    if (permissionsObject.hasOwnProperty(resource)) {
      const resourcePermissions = permissionsObject[resource];

      for (let i = 0; i < resourcePermissions.length; i++) {
        if (resourcePermissions[i] !== null) {
          compressedValue |= 1 << resourcePermissions[i] - 1;
        }
      }
    }
  }

  return compressedValue;
}
```

### Decompressing User Permissions
- To decompress user permissions, we need to reverse the process. 
- We start with the compressed value and use the bitwise AND operator ('&') to check if a specific bit is set. 
- If it's set, we include that permission, otherwise, we set it as null. 
- We iterate through the bits and rebuild the user's permissions for each resource.

```js
function decompress(compressedValue, resources) {
  const decompressedPermissions = {};

  for (const resource of resources) {
    decompressedPermissions[resource] = [];

    for (let i = 0; i < permissions[resource].length; i++) {
      if ((compressedValue & (1 << permissions[resource][i] - 1)) !== 0) {
        decompressedPermissions[resource].push(permissions[resource][i]);
      } else {
        decompressedPermissions[resource].push(null);
      }
    }
  }

  return decompressedPermissions;
}
```

### Putting It All Together

```js
const permissions = {
  forms: [1, 2, 3, 4],
  users: [5, 6, 7, 8],
  archive: [9, 10, 11, 12],
};

const userPermissions = {
  forms: [1, 2, null, 4],
  users: [5, 6, 7, null],
  archive: [9, 10, 11, 12],
};

function compress(permissionsObject) {
  let compressedValue = 0;

  for (const resource in permissionsObject) {
    if (permissionsObject.hasOwnProperty(resource)) {
      const resourcePermissions = permissionsObject[resource];

      for (let i = 0; i < resourcePermissions.length; i++) {
        if (resourcePermissions[i] !== null) {
          compressedValue |= 1 << resourcePermissions[i] - 1;
        }
      }
    }
  }

  return compressedValue;
}

function decompress(compressedValue, resources) {
  const decompressedPermissions = {};

  for (const resource of resources) {
    decompressedPermissions[resource] = [];
    for (let i = 0; i < permissions[resource].length; i++) {
      if ((compressedValue & (1 << permissions[resource][i] - 1)) !== 0) {
        decompressedPermissions[resource].push(permissions[resource][i]);
      } else {
        decompressedPermissions[resource].push(null);
      }
    }
  }

  return decompressedPermissions;
}

const compressedValue = compress(userPermissions);
console.log('Compressed:', compressedValue);

const decompressedPermissions = decompress(compressedValue, Object.keys(userPermissions));
console.log('Decompressed:', decompressedPermissions);
```