# Isometric Titanium

> This is just a placeholder for now around some thoughts about an experimental Ti.Next API.Nothing in this repo should be considered stable, possible, eventual or likely.


## Uniform Core

I'd like to re-think the Titanium API for Ti.Next.  We have an opportunity with [Hyperloop](https://github.com/hyperloop) to create a uniform core API, something that by its very nature is isometric.

One of the big challenges that Ti.Current provides is that each implementation of the API is distinct and unique -- mostly in the implementation, but often, in the interpretation of the implementation by the developer.  Even slight changes in the interpretation can create drastically different, and frustrating, bugs or errors.

I'd like to look at a layered cake approach to the Ti.Next API.  Something that starts with Hyperloop at the bottom of the stack and with each stack become a little more narrow and specific and less uniform as you get to the top of the cake.  So, the lower in the stack, the more broad and common.

The core would be a specified set of APIs that are required to be 100% uniform across every platform to be certified as a valid implementation.  Let's call this Level 1.   Each level would leverage the previous layer, much like the [OSI model](http://en.wikipedia.org/wiki/OSI_model).  Hyperloop, I suppose, could be considered Level 0.

## Level 0 API

The following would be provided by the Hyperloop compiler as part of the generated code.

- Module loading (Node.JS require, etc)
- Memory handling (buffers, pointers, etc)
- Native API exposure
- Loading Third-party Libraries (such as third-party iOS Frameworks)
- Background processing / async
- Timers
- Logging
- Profiling / Debugging
- Global scope support
- Process API

The generated implementations would be written in 100% portable native code as part of the compilation pipeline.

## Level 1 API

The Level 1 API would be 100% uniform (cross platform) and would be implemented in pure JavaScript (and possibly optimized either as part of the hyperloop distribution or during compilation).

- Networking
- Filesystem
- Eventing
- Database
- Configuration / Properties
- UI Layout system
- Basic UI primitives (window, view, colors, etc)
- Localization
- XML

The Level 1 API would likely be the lowest level of the Ti.Next framework.  These APIs would either be generated or hand written and likely would be just packaged (possibly in a special way) as modules.

Level 1 API would intentionally be small and concise.  Once stable, they would likely not change.  Adding APIs to this Level 1 would be done reluctantly, preferring higher Levels if possible (specifically Level 3).

## Level 2 API

The Level 2 API would provide the first level of platform specialization and would provide the secondary common APIs.  A good example is the secondary UI primitives common in Titanium such as Animations.  The APIs would be consistently the same but the implementations would be either fully or partially supplied as part of native APIs in Hyperloop.


## Level 3 API

The Level 3 API would be primarily specialized for platform specific APIs -- APIs that would never live in the levels below and would more than likely never be common.  A good example of this is the UIPopover control or an Android Notification.

The Level 3 APIs could be packaged as part of the compiler toolchain to make it an experience similar to Ti.Current (i.e. feels like it is part of the Ti namespace), but would not specifically have to be part of the Ti.Next distribution.

## Backwards Compatibility

First off, do we need it?  Probably.  Ideally.  Could this be handled with a shim or a specialized pre-processor as part of the compiler pipeline?

The syntax likely should also be part of the pre-processor.  For example, the following code:

	var window = Ti.UI.createWindow({backgroundColor:'red'});

Could still be written by the developer, however, the compiler might pre-process to something like:

	var window = new (require('ti/ui/window'))({backgroundColor:'red'});

In this way, the experience that developers currently have (and their representative code) could stay the same.  The compiler would know how to pull in the appropriate modules as needed as part of the build process.

## Async versus Sync

Today, the Ti.Current API is synchronous for the most part.  This has provided a much more simple developer experience at the slight tradeoff in performance.

For Ti.Next, we should think hard about asynchronous APIs.  We could provide a promise style way to provide an easier API for developers.  We could also use the compiler to turn synchronous code into async code for execution (although, this could be itself a problem if not done well).

This is a really hot and controversial topic in the Node community and something we should really study and learn from the good and bad.


