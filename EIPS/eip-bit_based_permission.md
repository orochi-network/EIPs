---
eip:
title: Bit Based Permission
description: A permission and role system based on bits
author: Chiro (@chiro-hiro), Victor Dusart (@vdusart)
discussions-to: https://ethereum-magicians.org/t/bit-based-permission/13065
status: Draft
type: Standards Track
category: ERC
created: 2023-02-27
---

## Abstract

This EIP offers a standard to build permission and role system based on bits. Each permission is represented by a single bit in `uint256` from which we can defined up to `256` permissions and `2²⁵⁶` roles. This approach use bitwise operator and bitmask to determine the access right which is much more efficient and flexible than `string` comparison or `keccak256(string)`. We are able to specify the importance of permission based on the bit order.

## Motivation

**Role is a combination of multiple permissions**, we can use `OR` operator to combine new role based on multiple permissions, since permissions were defined as a power of two.

**Cheaper verification**, Doing `AND` operator on a permission bitmask, it’s much more cheaper than other approach `keccak256(string)`.

**Ordering permission by its importance**, We can use the most significant bit to represent for the most important permission, the comparison can be done easily since it all are `uint256`s.

**Flexibility**, 256 permissions define up to 2²⁵⁶ different roles, and all roles don't have to be predefined.

## Specification

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in RFC 2119 and RFC 8174.

_Note_ The following specifications use syntax from Solidity `0.8.7` (or above)

- Permission and role MUST be defined as an `uint256`
- Permission MUST be defined as power of two
- Permission MUST be unique
- `0` MUST be used for none permission

An example of define new permissions and roles:

```text
const PERMISSION_NONE = 0;
const PERMISSION_READ = 1;     // 2⁰
const PERMISSION_WRITE = 2;    // 2¹
const PERMISSION_EXECUTE = 4;  // 2²

// Role admin = 4 | 2 | 1 = 7
const ROLE_ADMIN = PERMISSION_READ | PERMISSION_WRITE | PERMISSION_EXECUTE;
// Role operator = 1 | 2 = 3
const ROLE_OPERATOR = PERMISSION_READ | PERMISSION_WRITE;
```

An example of checking for required permissions:

```text
checkingPermission & ROLE_OPERATOR == ROLE_OPERATOR
```

## Rationale

Needs discussion.

## Reference Implementation

Needs discussion.

## Security Considerations

Need more discussion.

## Copyright

Copyright and related rights waived via [CC0](../LICENSE.md).
