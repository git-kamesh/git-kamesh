+++ 
title = "Compressing and Decompressing User Permissions in JavaScript"
description = "User permissions are an essential part of any application's security model. One major use case for compressing and decompressing user permissions is to include the compressed permissions in an authentication token. When a user logs in or obtains an access token, you can attach their permissions to the token."
date = "2024-01-03"
author = "Kamesh Sethupathi"
tags = ["algorithm", "jwt", "security", "javascript", "privacy", "engineering"]
+++

User permissions are an essential part of any application's security model. One major use case for compressing and decompressing user permissions is to include the compressed permissions in an authentication token. When a user login and obtained the access token, you can attach their permissions to the token.

```
Compressed: 3963n
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
  let compressedValue = 0n;

  for (const resource in permissionsObject) {
    if (permissionsObject.hasOwnProperty(resource)) {
      const resourcePermissions = permissionsObject[resource];

      for (let i = 0; i < resourcePermissions.length; i++) {
        if (resourcePermissions[i] !== null) {
          compressedValue |= 1n << BigInt(resourcePermissions[i] - 1);
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
      if ((compressedValue & (1n << BigInt(permissions[resource][i] - 1))) !== 0n) {
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
  let compressedValue = 0n;

  for (const resource in permissionsObject) {
    if (permissionsObject.hasOwnProperty(resource)) {
      const resourcePermissions = permissionsObject[resource];

      for (let i = 0; i < resourcePermissions.length; i++) {
        if (resourcePermissions[i] !== null) {
          compressedValue |= 1n << BigInt(resourcePermissions[i] - 1);
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
      if ((compressedValue & (1n << BigInt(permissions[resource][i] - 1))) !== 0n) {
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

## Breaking down the math behind

1. **Bitwise OR Operation:**
   - `compressed |= currentPermission`: The bitwise OR operator (`|`) is used to pack permissions into the `compressed` variable. This operation sets the bits in `compressed` to 1 at the positions where the corresponding permissions are present in `currentPermission`.

2. **Bitwise AND Operation:**
   - `compressed & currentPermission`: The bitwise AND operator (`&`) is employed to check whether a permission is present in the `compressed` variable. If the result is not zero, it indicates that the permission is already compressed.

3. **Bit Shifting for Space Efficiency:**
   - `1 << permissionValue - 1`: This expression ensures that each permission occupies only one bit in the compressed permission bit space. It achieves this by left-shifting the binary digit 1 to the left by `permissionValue - 1` positions, effectively creating a unique bitmask for each permission.

4. **Use of BigInt for Larger Numbers:**
   - `BigInt` is utilized instead of `Int` to overcome the limitations imposed by regular JavaScript integers. This is crucial when dealing with larger numbers, as `BigInt` provides extended precision, allowing the representation of values beyond the range supported by standard integers.


{{< include "reachout.md" >}}