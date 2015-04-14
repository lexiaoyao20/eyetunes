# About #

EyeTunes.framework is a Cocoa Framework that abstracts away all the
ugly Carbon Apple Events magic and allows you to directly control
iTunes from any Cocoa Application.

# Features #

  * Get all references to iTunes playlists, tracks and album art.
  * Set any writable fields that iTunes exposes such as Track name, artwork and much more.
  * Control iTunes and select playlists and tracks by using either track filenames or database ids.
  * Search the iTunes library just like the search box does.
  * Extract persistent ID and fetch tracks using such ids.

# Example #

To grab an NSImage from the current playing track (say you're
implementing some new album art viewier), you can use this simple snippet:

```
    #import <EyeTunes/EyeTunes.h>
    
    - (NSImage *) getArtworkOfPlayingSong {
        EyeTunes *eyetunes = [EyeTunes sharedInstance];
        ETTrack *currentTrack = [eyetunes currentTrack];
        if (!currentTrack)
           return nil;

        return [[currentTrack artwork] objectAtIndex:0];
     }
```

To see more examples of how to use EyeTunes in your projects, look at
the [EyeTunesDebug main.m](http://code.google.com/p/eyetunes/source/browse/EyeTunes/trunk/main.m).

# Building and Using EyeTunes #

You need Mac OS X 10.4 and Xcode 2.x. EyeTunes itself is written for
iTunes 6.0, but possibly works with iTunes 4.7 without and problems
except access to some fields may return an error.

## Obtaining EyeTunes ##

It is highly recommended you checkout the latest source from SVN
rather than the release tarball.

To checkout through SVN, install Subversion. if you haven't
already (via Webkit) and type this command in the terminal:

```
   svn checkout http://eyetunes.googlecode.com/svn/EyeTunes/trunk/ eyetunes
```

Otherwise, grab a download form the EyeTunes. See: ChangeLog.

## Building/Including EyeTunes ##

You will probably want to include it in your project rather than
building it standalone. There are two ways to include the code into
your project.

First one is to just copy all the files into your project and add them to your target. This way you do not have to deal
with copying the framework into your `YourApplication.app/Contents/Frameworks`.

The second method, is probably the preferred method because it means you can build a version of EyeTunes and never have to worry about it again.

  1. Build the EyeTunes project as usual using the target `EyeTunes.framework` and Deployment build type in Xcode.
> 2. Drag the resulting framework from Finder (located in `./EyeTunes/build/Deployment/`) into your Project's framework tree. You will probably be prompted to add to a target.
> 3. Now add a new Project Build Phase to your target called Copy Files.
> 4. In the properties of this Build Phase, select "Frameworks" as the type and drag the EyeTunes.framework from your project tree into this build phase. For more information on this, please read the [Apple Documentation](http://developer.apple.com/documentation/MacOSX/Conceptual/BPFrameworks/index.html) or [CocoaDev](http://www.cocoadev.com/index.pl?EmbeddingFrameworksInApplications) on Embedding Frameworks.




