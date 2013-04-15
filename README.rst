=========================================
Offsite backup exchange programme (OBEP)
=========================================

Overview
=========
OBEP is a social framework for participants to enjoy cheap offsite-backups.

* Hosters set up one or more servers (hosts) that can be accessed over the internet.
* Users provide their hard drives which they want the hosters to host in the servers

Why OBEP?
===========
Backups are very important. Off-site backups add an extra layer of protection
in that it ensures that the data that is stored off-site is secure even when your
house burns down or someone destroys all the computers and hard drives in your
house. Sure, the probability of the above happening is slim, but when it really does happen,
you would probably wish that you backed up your 2TB business-critical porn collection :).

We have the following requirements for an offsite-backup system:

1. Connection to the backup server must be encrypted
2. Users must be able to guarantee their encryption, as well as privacy-through-encryption,
   of their data on the backup server.
3. Users must be able to upload their huge amount of data (~3TB) quickly (requiring
   a fast upload speed). Alternatively, users must be able to send in their
   hard drive/flash drive for FREE and have it available on the offsite-backup system.
4. It just be cheap (e.g. same or cheaper than having a hard drive + electricity costs)
5. Support all major operating systems (Windows, Mac, Linux, BSDs)

However, none of the established offsite-backup services fulfilled all of
the requirements. Backblaze_ is cheap, but the client does not support Linux,
and it is closed-source, meaning that users cannot guarantee their data is encrypted.
`Amazon S3`_, through their API, allows users to guarantee encryption, but is very
expensive.

Introducing the offsite backup exchange programme, where you can host your
hard drive(s) on a friend's (or stranger's) computer for free, hence having
cheap offsite backups.

.. _Backblaze: http://www.backblaze.com/
.. _`Amazon S3`: http://aws.amazon.com/s3/

What OBEP is and is not
=========================

* OBEP is **not** a VPS/dedicated server for you to run your own processing tasks -- Hosters, at
  their own discretion, can choose to limit users to the capabilities of the host
  that is not relevant to storage (e.g. network access, software etc.)
* OBEP is **not** free storage space -- Users must provide their own hard drives
* OBEP is **not** RAID -- while hosters should try to preserve users' hard drives 
  in good working condition, they do not provide any redundancy guarantees.

Terminology
============

User
    The person providing the hard drive to be hosted.

Hoster
    The person providing the host server/computer to host users' hard drives.

Host
    The server/computer owned by the hoster which will host users' hard drives.
    Hosts have certain requirements which are detailed in `Host Requirements`_.

Host Requirements
==================
Ownership
----------
The hoster MUST *fully* own the host server/computer. It cannot be a VPS,
cloud host or dedicated server. The hoster must have physical access to the host
at all times.

Host access over Internet
--------------------------
Hosters must have a host that is accessible through the internet using
a constant domain name. If the host has a dynamic IP, the hoster
must ensure that the DNS records of the domain is kept
constantly updated, as well as propagated in a timely manner.

The host should be accessible at all times, and
failing that, be accessible at known intervals during the day (and night).
This means that the hosts should not be on metered bandwidth.

Hardware
----------
The host hardware must support SATA 2 and above. The host's chassis must
support 3.5 inch hard drives.

Operating System, drivers and software
------------------------------------------
The host must run a POSIX-compatible Operating System (e.g. Linux)

The SSH server on the host must support SFTP, as well as allowing users
to authenticate using RSA key pairs.

The operating system must support the filesystems and software
as detailed in `Supported Drivers and Software`_.

Each user must have his own account on the host, as well as his own
group with the same group name as the username. It is recommended
that UID == GID.

Services Requirements
------------------------
Technically, users still own their own hard drives inside the host,
and hence must be allowed to do anything with the data on the hard drive,
including formatting the partition. Users are only allowed
ONE partition on their hard drive.

Hosters are not liable for any damage to the users' hard drives
due to user error (e.g. misusing the format filesystems command).
However, hosters *are* liable for damage to users' hard drives
due to faulty software.

Users are not allowed to have full drive encryption of their hard
drive on the host. (It is insecure anyway, as the unencrypted data
will be available to the hoster). If a user wants
to encrypt his data on the host, he should encrypt the data *before*
sending it for storage to the host. (Hint: encfs with sshfs)

Security Requirements
-----------------------
Hosters can host multiple users.

The hoster must ensure that only the user owning the hard drive can access
their hard drive in the host.

Reliability Requirements
-----------------------------
The host, to the best of his ability, should preserve users hard drives
in working condition. 

That said, we do understand that stuff happens, and so hosters
are not liable for any damage or loss of data. It should
be noted, though, that OBEP operates on the basis of reputation,
so hosters should really try to have a reliable service.

While it is nice, hosters are not required to provide or support
any redundancy (e.g. RAID).

User Requirements
===================
Hard Drive
-----------
The hard drive must be either a 3.5 inch *INTERNAL* hard drive.
The hard drive must support SATA 2 and above.

The hard drive must have only ONE partition.

The hard drive, when handed over to the hoster, must already be formatted.
The filesystem must be one of those specified in `Supported Drivers and
Software`_.

Full-drive encryption is not allowed.

Risks
------
Note that by being a user, you are subjected to the following risks:

* You may not get your hard drive back.

Supported Drivers and Software
===============================

Filesystem Drivers
-------------------

* ext2
* ext3
* ext4

Hosters must ensure that these filesystem drivers work as intended.

Additional filesystem drivers may be provided at the hoster's discretion.

Software
---------

* dash (or a compatible alternative such as bash or zsh)
* rsync

Hosters must ensure that these software work as intended.

Additional software may be provided at the hoster's discretion.

How it works
==============

1. A user reachs an agreement with a hoster.
2. The hoster creates an account for the user on the host and sends the user his
   username, password, UID and GID, SSH Server RSA Fingerprint through a 
   secure channel.
3. The user meets up with the hoster and the user gives his hard drive to the host.
   The hard drive should preferably contain the user's backup already.

Tips and Tricks for Users
===========================

Use encfs and sshfs to store your data encrypted on the host
---------------------------------------------------------------

TODO

Related Reading
================
We are not the first people to come up with OBEP. Here are some others who did,

* http://www.documentsnap.com/how-i-do-offsite-backup-to-a-friends-computer-using-crashplan/
