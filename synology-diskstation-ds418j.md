WIP
# Synology DiskStation ds418j
## Putting Synology Hybrid Raid (SHR) to the test

## Introduction
After years of using nas4free (formerly freenas) for my personal network attached storage needs, I decided to switch.
This change did not come easily nor naturally.
Strong advocate of opensource software and projects, nas4free (formerly freenas) was THE solution to my geeky needs.
So why ? Why go spend a non-negligible amount of money on a paid-for appliance.
Well, let's step back a bit for a fuller picture.

## Data storage
Of all the parts that compose your typical computing environment, data storage sits in a peculiar spot.
All hardware and software components, all of them, can misbehave. But if you were to pick one that you absolutely want to make sure works, you'd pick storage.
CPUs may burn, RAM may fail, Operating systems may put your server close to a halt and network frames may disappear in the void, but the data you wrote on your disk shall not disappear.
In the worst case, pull out the drive and plug it on a machine that is working. You're safe, your por^H^H^H important documents are safe.
Now, having rescued multiple hard drives that were physically or logically dying, I am very confident that data can be retrieved.
This however becomes a bit trickier, if you start growing out of a single drive.
When you start using multiple drives that transparently 'merge' toghether to hold you data, you enter the realm of RAID.
Your data in no longer nicely written on one disk, but scattered acress multiple drives, and, if you have a ot of sanity, you chose to use one of the RAID modes (called levels) that offer some kind of redundancy.
Now that your data sits on multiple drives, you better trust the software that does the spreading, because there is no way in hell for you to stitch the data back if the software you were relying on fails to do it.

## My beloved file server
My first file server had only 2 drives. Ran a basic linux distribution.
In order to get redundancy in a 2 drive system, the only way to go is mirroring (also called RAID 1).
So the software that came with linux happily built my RAID 1 array an worked fine.
(well, until a defective and undetected RAM failure corrupted all the data, but that is out of the scope of this post).

## Free network attached storage software
I used freenas in ... ok, a looooong time ago.
Back then, it was more or less the only solution for a fully integrated, rock solid (*BSD + ZFS) storage solution.
ZFS was (and probably still is) THE ultimate filesystem/disk management tool.
BSD on the other hand is an equally rock solid opensource operating system.
The freenas software (which became nas4free) pulls in those 2 unbreakable components and adds a nice web UI on top.
This can be installed on any PC you may have laying around.
Until this recent change, it was providing an excellent service.
Other software bundles, based on linux are also out there. I'm sure they would all have done the job.

## While we're at it ...
As my current nas4free system was filling up to a dangerous 96% (2 x 4TB drives zRAID1), I was considering a hardware upgrade.
I was ready for a 'while we're at it ...' moment.
As I'm changing the hardware, wouldn't it be nice if I uprgaded nas4free to the latest version ? Or use one of these other cool NAS projects based on linux ?
Yes, I was lagging behind nas4free upgrades, because this one would require a reinstall (sometimes the changes are so deep, there is no upgrade path), so I'd need to make sure I have a full backup in case I couldn't reimport the ZFS pool.
Switching to yet another NAS software would require a testing phase (did I emphasize on the fact that you need to have backups ?).
And foremost, I'd need more space, so:
- either I add disks
- or I buy bigger disks
Adding disks beyond RAID1 is always tempting. You get RAID5 starting with 3 drives, so, compared to RAID1 you immediately double your storage.
However, the old PC I was using had only 2 drive bays, so adding drives would come at the cost of buying anew chassis, maybe a new motherboard ... see the rappithole there ?

To be continued ...

