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
Switching to yet another NAS software would require a testing phase (did I emphasize on the fact that you need to have backups before playing with your data ?).
And foremost, I'd need more space, so:
- either I add disks
- or I buy bigger disks
Adding disks beyond RAID1 is always tempting. You get RAID5 starting with 3 drives, so, compared to RAID1 you immediately double your storage.
However, the old PC I was using had only 2 drive bays and 2 sata ports, so adding drives would come at the cost of buying a new chassis, maybe a new motherboard ... see the rappithole there ?
So, in order to avoid loosing yet more time and solving the issue for, hopefully, a long time, I looked around for other ways. I'd be able to play with the rest once my real life need is secured.

## Commercial NAS appliances
I actually was already using a commercial all-in-one NAS appliance. Along with my local homebuilt NAS, I bought something that would easily survive at my parent's home, be simple to setup & maintain, low on power and as silent as possible.
I chose a 1 drive synology device as my offsite backup. In all honesty, after the initial fiddeling to get rsync+ssh working the way I wanted, there was not much to say. I connected maybe twice a year into the web interface to find that there was nothing to do. The system is quiet, spinning down the disk most of the time, never missed a backup coming from my systems, performed it's own software updates with no hassle and I really feel, after a couple of years, that there really was not much happeninig. Exactly what one would expect.
I lurked on the other players, namely Qnap and Drobo. Both were, AFAIK, more expensive than Synology. The reason I looked at Drobo was because of their ability to dispatch the data on disks of different sizes and maximize usage while keeping redundancy. It so happened that talking t oa friend about it, he pointed out that Synalogy also had that kind of technology, called SHR (Synology Hybrid RAID, more about that later). So, given the better price-point, and my satisfaction with the 1 drive product, I decided to give it a try.

## How to not make it simple
Here's the plan.
I have 2X4TB drives being used in my current NAS.
I bought a 4 drive NAS from Synology (DS418j) and 2 additional 4TB drives.
I could have just started the NAS with the 2X4TB drives, tansferred the data over and added the 2 other 4TB drives in the 4 bay NAS.
But I didn't.
I had 4X1TB WD Green drives laying around. Given their bad reputation (from more or less day 1 of their shipment) and low capacity (by today's standards), I hadn't used them in a while. I decided to build a 4 drive RAID off those drives.
One reason was, I wanted to make sure the 4TB drives I bought were ok, so I ran the linux program 'badblocks' on them. The 'badblocks' program essentially writes patterns on the whole drive and reads them back. The default is 4 patterns, 1 write pass and 1 read pass per pattern, so 4 full writes and 4 full reads. Trying the 1TB drives in the NAS, would allow me to test things while waiting for the real drives.
The other reason was, I got curious about the SHR (Synology Hybrid RAID). Supposedly, I would be able to start from 4x1TB drives and upgrade to 4X4TB drives transparently and maximizing the usable capacity with every drive. Having quiet a good idea on how RAID (hard & soft) works, I wanted to take advantage of the full fledged linux shell access to spy on the innards of the system.
Finally, never understimate the fun of doing it represents ;)

## SHR (Synology Hybrid RAID)
For a start, the documentation on the site (https://www.synology.com/en-us/knowledgebase/DSM/tutorial/Storage/What_is_Synology_Hybrid_RAID_SHR) gives a good idea on how SHR works.
Basically, it does try to maximize disk usage while never loosing redundancy.
The interesting part is, if you have disks of different sized, the software will slice things in a way it can have multiple software RAID groups on the drives, stitching the resulting splinters toghether with LVM.
Clever indeed, with the caveat that it is biased towards growth. scaling down the drives won't work.
I must admit, that situation probably won't happen and the upscaling scenario is actually the most probable.

## Scaling up ...
So I started my RAID setup with 4x1TB drives. The initial install phase went through so easily, I barely noticed.
I won't go into details because, if that step is stopping you, you probably don't want to mess with the rest.
SO here we are, with 3TB or so on the RAID5 arrai of 4x1TB disks.
I started copying some data over to the new NAS as soon as it was up.
After the initial burst of data, I stopped the NAS and pulled one of the drives replacing it with the first of the 2 new 4TB drives.
After powering it up, it took a couple of minutes for the device to be up again. And it started beeping.
Indeed, I just broke the RAID integrity and we're running in degraded mode. The GUI pops up a screen as soon as you log in allowing you to stop the beeping (great idea). From there, it's easy to just tell the thing to 'repair' the volume.
And it does so. The new drive is used partially for now, as there is no way to use the 3TB in a redundant way.
Once the RAID group is in sync, I proceed with replacing a second 1TB drive.
After fixing the RAID group a second time, I now can use the 2x3TB of space left on the 2x4TB drives.
When asked, the UI created a RAID1 group. I now have 3TB (4x1TB in RAID5) plus 3TB (2x3TB in RAID1).
Once all was back in sync, I completed the data transfer from my old NAS.
Once all was tansferred, I replaced yet another 1TB drive. Now things become interesting.
Under the hood, mdamd was reshaping my RAID1 group to a RAID5 group.
Obviously all these processes are lengthy.
Even with the change in parameters on the NAS to favor speed of resync over usability of the NAS, this took several days in aggregate.

## Aftermath
Now that all is in sync and all 4 4TB drives are in place, the trace of the operations remain.
I have 2 RAID5 groups: one with 1TB shards and one with 3TB shards.
LVM happily shows a unique volume and that's what matters.

Being able to mix and match disks is a reassuring feature.
The next upgrade probably will be 8TB or 12TB disks, once I'll have filled up the existing space.

Meanwhile, I'll play around with all the other nice features synology brings for free.

That's it folks.

R. Daneel olivaw.
