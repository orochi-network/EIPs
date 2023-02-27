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

This EIP offers a standard for building a bit-based permission and role system. Each permission is represented by a single bit in `uint256` from which we can defined up to `256` permissions and `2²⁵⁶` roles. We are able to specify the importance of permission based on the order of the bits.

## Motivation

Currently permission and access control are done using `string` or `keccak256(string)` comparaisons (see [EIP-5982](./eip-5982.md)). This is a very user friendly way to do this.
But by using bitwise and bitmask operations to determine access rights, we get a lot of benefits such as efficiency, flexibility and others, let's walk through them.

### **Gas cost efficiency**:

Bitwise operations are very cheap and fast. For example doing an `AND` bitwise operation on a permission bitmak is cheaper than other comparaison.

### **Flexibility**:

With the `256` bits of the `uint256`, we can create up to `256` differents permissions which leads to `2²⁵⁶` unique combinaisons also called roles.
*(A role is a combination of multiple permissions).* All roles don't have to be predefined.

Since permissions are defined as powers of two, we can use the `OR` operator to create new role based on multiple permissions.

### **Ordering permissions by importance**:

We can use the most significant bit to represent the most important permission, the comparison between permissions can then be done easily since they all are `uint256`s.

### **Permission subsets**:
TODO

## Specification

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in RFC 2119 and RFC 8174.

_Note_ The following specifications use syntax from Solidity `0.8.7` (or above)

- Permission and role MUST be defined as an `uint256`
- Permission MUST be defined as power of two
- Permission MUST be unique

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
