---
layout: post
published: true
title: "First thoughts on Oil Change Ramps"
---
Consider this a precursor to a longer post that I'm currently writing on the subject of preservation of digital media (games in particular), and the sorry state of tools that are built for the purpose.

In brief, I am dissatisfied with the state of rom library management tools (software for keeping track of ROM dumps of ancient games, or of homebrew games written for old consoles). There's a lot to say about this - the history of the efforts, the current state of things, and the extraordinary challenges faced by people attempting to preserve these pieces of an increasingly distant history. 

All that will have to wait, though. What got me started on this tangent in the first place is that all of these tools - CLRMamePro, RomCenter, Romvault, etc - are deficient in various ways, especially for the task of dealing with huge volumes of heterogeneous files. It seems that these tools were primarily made for people pirating old games, so they tend to have a few of the same issues:

1) most are windows-only or at least designed for windows first. This is not itself a problem but combined with 2...
2) they are graphical-only, meant to be used real-time and interactively. I can think of reasons that this would be, but when you combine it with these tools often being windows-only, it means that managing collections that are in flux is extremely manual and annoying. 
3) they're *SLOW*. some of this is just that doing checksums and hashes of large files can be slow, and some of this is that storage of files for preservation requires a networked file share and redundant backups, meaning every directory scan and checksum is being performed over the network to spinning-platter hard drives, not across a local PCIe bus to an SSD.

Taking a hint from the design of the Manta object storage system, I decided that if the data couldn't come to the compute, the compute would have to come to the data - and to do that, I'd need a rom indexing, archiving, and reporting tool that was terminal-driven, scriptable, and designed for automated configuration AND execution. It needed to be unix-based (my file storage systems are Unix or Linux based), portable, and ideally function the same way between Linux, BSD, and MacOS. To that end, I have started working on a tool i'm just calling RomManager, for now, and after pounding away at python code for longer than I'd like to admit (I remain, as always, the worst programmer) I have what I'm calling version 1.0.

It's definitely more a Minimum Viable Product than it is a complete project, but it's good enough that I'm able to put it into use. It performs the basic functions I need, and I can extend it later:

- scans a folder for .dat files, and maps each of them to a directory of ROM files
- scans the rom folders for files in .7z format and reads their CRCs directly, or computes the CRC of non-7z files
- determines which ROM files are entirely missing, present but incorrectly named, present but incorrectly packed, or waiting in an "Unsorted" directory
- cures those problems "automatically"

There's a long way to go - configuration is bare-bones, reporting is all but nonexistent, persistence is entirely absent (RomManager has no idea if it has ever been run before, or what the prior state of any of the directories was), and it's performing some dangerous operations (unpacking 7z files, renaming and deleting files) with what I would consider "insufficient validation of input", but it exists, and it works, and it solves the main problems I've had with other rom managers for quite a while now.

Next steps are to cleanup the code (v1 of anything I make is always kind of a mess), document behaviors, build more sanity checks into the system, and make the behavior more configurable. Once that's done, I'll see what else I need it to do, and go from there. 