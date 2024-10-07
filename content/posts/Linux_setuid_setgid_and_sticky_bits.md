---
title: 'Linux Setuid Setgid and Sticky Bits'
date: 2024-10-06T21:03:38-07:00
draft: false
hidden: false
externalURL: false
showDate: true
showModDate: true
showReadingTime: true
showTags: true
showPagination: true
invertPagination: true
showToC: true
openToC: false
showComments: false
showHeadingAnchors: true
tags: ['linux']
---

It can be confusing figuring out what the `setuid` or `setgid` or even the `sticky` bit options are for file and folder permissions on Linux systems. Especially if you are just starting out and trying to convert from the symbolic method to using the numeric (octal) method (more on that in a later post).

In this post I’ll detail the specific knowledge items you should grasp to comprehend the `setuid` and `setgid` bits, as well as the `sticky` bit (also called restrictive deletion).

## Recap on basics using chmod

Before we get into that, here is a quick recap on the basics for file and folder permissions using the `chmod` command.

The `chmod` command allows changing of the file mode bits, aka the access permissions for files and directories. This can be done using symbolic representation, or numeric (octal):

### Symbolic method

In this method, you represent the User, Group, Other, or All using the following letters: `ugoa`

For permissions, you specify adding with `+` or removing with `-`. You can also use `=` to add the mentioned bits, and remove those not mentioned. If you do not specify `ugoa` then it defaults to `a`.

Permissions can be `rwxXst` which stand for read, write, execute, set user or group ID, and the sticky bit.

- uppercase `X` is used when recursively changing permissions, where it will assign execute permissions if the file is a directory, and only assign it to a file if that file already has execute permissions.

For example

```bash
chmod u+rwx,g-w,o-rwx notes.txt
```

### Numeric (octal) method

In this method, you define permissions using four octal digits, which you derive by adding up the values 4, 2, and 1.

The first digit is not required but is where we select the `setuid`, `setgid`, and `sticky` bits, which will be explained further down.

The next three digits are for defining permissions for the User, Group, and Others.

- read → 4
- write → 2
- execute → 1

To assign the User bit read, write, and execute, you would add each number together (4 + 2 + 1) which gives 7. This would be the digit used. For read and execute that would be 5 (by adding 4 + 1).

For example:

```bash
chmod 740 notes.txt
```

## SETUID, SETGID, and STICKY

If you had an executable file that needed to be ran as a specific user, then setting the `setuid` bit would accomplish this. So if a file that is owned by Alice needs to be executed as Alice, setting this bit would allow Bob to execute the file as Alice, instead of Bob. This can be handy if the executable needs specific user permissions to perform tasks such as changing system files.

The symbolic method for `setuid` is done with the following:

- `u+s` → sets the set-user-id bit (use `4` for numeric/octal)
- `u-s` → clears the set-user-id bit (use `2` for numeric/octal)

Using the same example with Alice and Bob above, but for the `setgid` bit: when set, the file would execute as the group that owns the file, not the group that Bob belongs to (primary group). This is handy similarly, where certain group permissions might be required for the file when executed.

Another important point to make here is that also apply when a user creates a file in a directory with either of these bits set. So if you want all the files created in a directory to have a specific group owner, then set the `setgid` bit on the directory. Same would be true for the user owner.

### The sticky bit, or restrictive deletion

While this is often referred to as the sticky bit, it might be easier to understand thinking of it in the other term: restricted deletion.

In simplest terms, it means just that. Having this bit set will restrict deletion of files in a directory if the user is not the owner of the file. So if Alice and Bob both access an Accounting file, and each have their own created Clients file:

- Bob is not the owner of Alice’s client file, and Alice is not the owner of Bob’s client file.
- Bob can not delete Alice’s client file, and Alice can not delete Bob’s client file.

One possible caveat here is that the group owner of the directory (and subsequent files) must be set (so the `setgid` bit), so that the files Bob or Alice create for their client lists have the same group owner. 

To set the `sticky` or `restrictive deletion` bit use the following:

- `+t` → sets the bit (`1` for numeric/octal)
- `-t` → clears the bit (`00` or `=` for numeric/octal, but this will remove `setuid` and `setgid` if set previously as well)

## Put it all together, with examples

The special permissions `setuid`, `setgid` and `sticky` bits allow additional privileges over just the standard read, write, execute permission sets.

To set bit for `setuid`:

```bash
chmod u+s example.sh
```

To set bit for `setgid`:

```bash
chmod g+s example.sh
```

To set bit for `sticky`:

```bash
chmod +t examples/
```