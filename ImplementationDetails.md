# Introduction #

In case people want to know how easy/hard it is to interface with a
Carbon application like iTunes through Carbon Apple Events, here is an
explaination of all the mechanisms involved.

Note: A lot of this information is gathered from mailing lists, and
bits of code and old webpages that are around.

## Overview ##

The basis of EyeTunes is an ETAppleEventObject that acts as a proxy
between your application code and the remote object that is accessed
through AppleEvents. On top of that, ETTrack and ETPlaylists are
subclasses of this object, and use the functions in there to perform
the following possible actions:

  1. Get a property (int, float, string) of a remote object.
> 2. Set a property (int, float, string) of a remote object.
> 3. Get the all the elements of a class that belongs to the remote object.
> 4. Modify the nth element of a class that belongs to the remote object.

As an example of what the above means, imagine the remote object being an iTunes track. An example property that you can set/get would be the trackName. However, the artwork is not a primitive type (int, float, string), so that instead is stored as a "class" of the remote object.

So the actual implementation of `[ETTrack setArtwork:atIndex:]` is actually more complicated as it is dealing with "class" rather than a primitive type.

To that end, modify EyeTunes is quite straightforward. If there is a property that is not exposed, then just find out the DescType (see SDEF) and copy a similar field in ETTrack or ETPlaylist. When working with custom types it is a little more difficult.

Because every property query will result in an AppleEvent being sent, thus if performance is an issue, the programmer should cache theseresults.

## About Apple Events ##

Here are some notes about Apple Events that I have discovered over the course of writing the EyeTunes framework and may be important to other people who would like to hack on the code or perform similar feats on other Apple Scriptable products.

## Rsrc, AETE and SDEF ##

iTunes is a Carbon application, therefore it stores its AppleScriptability information in a slightly different way. That is is uses a format called AETE. You can find the definition in the `iTunes.rsrc` file inside `/Application/iTunes.app/Contents/Resources`.

Copy the `iTunes.rsrc` file from the iTunes.app bundle to a temporary directory, and use [Aete2sdef](http://webperso.easyconnect.fr/bdesgraupes/DocHTML/Aete2sdefHelp.html) to generate an sdef file. This is an XML file that defines all the classes, parameters and commands similar to what Apple's Script Editor Dictionary pane uses. Except the difference is that all the "code" elements are named. These are the 4 character codes that are very similar to HFS File Type codes.

## Carbon Apple Events ##

Short explanation of what Carbon Apple Events and how it fits into the grand sch
eme of things such as Apple Script.

## AEDesc and AEGizmo ##

Where to find information on AEDesc and AEGizmo

## Running Script Editor through gdb ##

Use this to sniff AEGizmo strings and understand how event codes are encoded. Apple explains how to do this very well in this [Technical Note TN2045](http://developer.apple.com/technotes/tn/tn2045.html)

## Running iTunes through gdb ##

Kinda useless unless you want to do some mach\_inject magic on it, but its complicated enough that it deserves a mention. Not that EyeTunes framework actually does anything as evil as that.
