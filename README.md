PortableGL
==========

"Because of the nature of Moore's law, anything that an extremely clever graphics programmer can do at one point can be replicated by a merely competent programmer some number of years later." - John Carmack


In a nutshell, PortableGL is an implementation of OpenGL 3.x core in clean C99 as a single header library (in the style of the
[stb libraries](https://github.com/nothings/stb)).

It can theoretically be used with anything that takes a framebuffer/texture as input (including just writing images to disk manually or using something like stb_image_write) but all the demos use SDL2 and it currently only supports 8-bits per channel RGBA as a target (and also for textures).

Its goals are,

1. Portability
2. Matching the API within reason, at the least matching features/abilities
3. Ease of Use
4. Straightforward code
5. Speed

Obviously there are trade-offs between several of those.  An example where 4 trumps 2 (and arguably 3) is with shaders.  Rather than
write or include a GLSL parser and have a built in compiler or interpreter, shaders are special C functions that match a specific prototype.
Uniforms are another example where 3 and 4 beat 2 because it made no sense to match the API because we can do things so much simpler by
passing a pointer to a user defined struct (see the examples).

Download
========
Get the source from [Github](https://github.com/rswinkle/PortableGL).

You can also install it via [Homebrew](https://formulae.brew.sh/formula/portablegl).

Gallery
=======
![gears](https://raw.githubusercontent.com/rswinkle/PortableGL/master/media/screenshots/gears.png)
![sphereworld](https://raw.githubusercontent.com/rswinkle/PortableGL/master/media/screenshots/sphereworld.png)
[![backpack](https://raw.githubusercontent.com/rswinkle/PortableGL/master/media/screenshots/backpack_model.png)](https://github.com/rswinkle/LearnPortableGL/blob/main/src/3.model_loading/model_loading.cpp)
![craft](https://raw.githubusercontent.com/rswinkle/PortableGL/master/media/screenshots/craft.png)

The last is a [PortableGL port](https://github.com/rswinkle/Craft/tree/portablegl) of Michael Fogleman's [Craft](https://www.michaelfogleman.com/projects/craft/).

See the [demos README.md](https://github.com/rswinkle/PortableGL/tree/master/demos)
for more screenshots, or look in the
[screenshots directory](https://github.com/rswinkle/PortableGL/tree/master/media/screenshots).

History
=======
PortableGL started as a very simple wireframe software renderer based on a tutorial in summer 2011.  I kept playing with it, adding minor features
over the next year, until in early 2013 I decided I should turn it into a software implementation of OpenGL.  This would save me a huge amount of time
and energy on API design since I'd just be implementing an existing good API (though some disagree) and also make the project more useful both to me
and potentially others.  Also, at the time Mesa3D was still years away from full 3.x support, not that I'm really competing, and the fact that there
was no finished implementation was a little motivating.  I made a lot of progress that year and had a few bursts here and there since, but once I got it
mostly working, I was less motivated and when I did work on it I spent my time on creating new demos/examples and tweaking or fixing minor things.  I
could have released an MVP back in 2014 at the earliest but late 2016 would have been the best compromise.  Anyway, after somewhere over 2000
hours spread out over 10 years, it is as you see it today. Software is never finished, and I'll be the first to admit PortableGL could use more polish.

Why
===
Aside from the fact that I just wrote it for fun and because I thought it was cool (maybe others will too), I can think of a few semi-practical purposes.

### Educational
I took a 400 level 3D Graphics course in college in fall 2010 months after OpenGL 3.3/4.0 was released.  It was taught based on the original
Red Book using OpenGL 1.1.  Fortunately, the professor let me use OpenGL 3.3 as long as I met the assignment requirements.  Sadly, college graphics
professors still teach 1.x and 2.x OpenGL today in 2022 more commonly than 3.x/4.x (or Vulkan).  A few are using WebGL 2.0 which I kind of
consider 1 step forward, 2 steps back.

While Vulkan is the newest thing (already 5 years old time flies), it really is overkill for learning 3D graphics.  There is rarely anything that students
make in your standard intro to 3D graphics that remotely stresses the performance of any laptop built in the last decade plus.  Using modern OpenGL (ie 3.3+ core profile)
to introduce all the standard concepts, vertices, triangles, textures, shaders, fragments/pixels, the transformation pipeline etc. first is much better
than trying to teach them Vulkan and graphics at the same time, and obviously better than teaching OpenGL API's that are decades old.

PortableGL could be a very convenient base for such a class.  It's easy to walk through the code and see the pipeline and how all the steps flow together.
For more advanced classes or graduate students in a shared class, modifying PortableGL in some way would be a good project.  It could be some
optimization or algorithm, maybe a new feature.  Theoretically it could be used as starter code for actual research into new graphics algorithms or techniques because it's such a convenient small foundation to change and share, vs trying to modify a project the size and complexity of Mesa3D or create a software
renderer from scratch.

### Special Cases
It's hard to imagine any hardware today that has a CPU capable of running software rendered 3D graphics at any respectable speed (especially with full
IEEE floating point) that doesn't *also* have some kind of dedicated GPU.  The GPU might only support OpenGL 2.0 give or take but for performance it'd
be better to stick to whatever the hardware supported than use PortableGL.  However, theoretically, there could be some platform somewhere where the CPU
is relatively powerful that doesn't have a GPU.  Maybe some of the current and future RISC SoC's for example?  In such a case PortableGL might be a
useful alternative to Mesa3D or similar.

Another special case is hobby OS's.  The hardware they run on might have a GPU but it might be impossible or more trouble than it's worth to get Mesa3D
to run on some systems.  If they have a C99 compliant compiler and standard library, they could use PortableGL to get at least some OpenGL-ish 3D support.

### Why C
I recently came across a comment regarding PortableGL that essentially asked, "why implement a dead technology
in a dying language?"

While I would argue that OpenGL is far from dead and C [isn't even close to dying](https://www.tiobe.com/tiobe-index/),
there are many [good reasons](https://chipmunk-physics.net/release/ChipmunkLatest-Docs/#Intro-WhyC) to write a library
in C.  Beyond those, OpenGL is a C API so it would be weird if you couldn't use it from C.  Writing it in clean C means it
compiles as C++ too.  Lastly, I just like C.  It was my first language and is still my favorite for a host of reasons.
Hey, if it's good enough for Bellard, it's certainly good enough for me.


Documentation
=============

There is the documentation in the comments at the top of the
[file](https://raw.githubusercontent.com/rswinkle/PortableGL/master/src/header_docs.txt) but there
is currently no formal documentation.  Looking at the examples and demos (and comparing them to
[opengl_reference](https://github.com/rswinkle/opengl_reference)) should be helpful.

I've also started porting the [learnopengl](https://learnopengl.com/) tutorial code [here](https://github.com/rswinkle/LearnPortableGL)
which is or will be the best resource, combining his tutorials explaining the OpenGL aspects and my comments in the ported code
explaining PortableGL's differences and limitations (at least in the first time they appear).

Honestly, the official OpenGL docs
and [reference pages](https://www.khronos.org/registry/OpenGL-Refpages/gl4/) are good for 90-95% of it as far as basic usage:

[4.6 Core reference](https://www.khronos.org/opengl/wiki/Category:Core_API_Reference)
[4.5 comprehensive reference](https://www.khronos.org/registry/OpenGL-Refpages/gl4/)
[tutorials and guides](https://www.khronos.org/opengl/wiki/Getting_Started#Tutorials_and_How_To_Guides)

Building
========

There are no dependencies for PortableGL itself, other than a compliant C99/C++ compiler.  The examples, demos,
and the performance test use SDL2 for the window/input/getting a framebuffer to the screen.
If you just want to do a quick test that it compiles and runs:

	cd testing
	make run_tests
	...
	./run_tests
	...
	All tests passed

You can look in testing/test_output to see the png's generated by `run_tests` which are compared with those in testing/expected_output.


For the rest, on Debian/Ubuntu based distributions you can install SDL2 using the following command:

`sudo apt install libsdl2-dev`

On Mac you can download the DMG file from their [releases page](https://github.com/libsdl-org/SDL/releases/tag/release-2.24.1) or install it through
a package manager like [Homebrew](https://brew.sh/), [MacPorts](https://ports.macports.org/), or [Fink](https://www.finkproject.org/).  Note, I do
not own a mac and have never tested PortableGL on one.  Worst case, you can always just compile SDL2 from source but one of the above options should work.

Once you have SDL2 installed you should be able to cd into examples, demos, or testing, and just run `make` or `make config=release` for optimized builds.

On Windows you can grab the zip you want from the same releases page linked above.

I use premake generated makefiles that I include in the repo which I use on Linux.  I have used these same Makefiles
to build under [MSYS2](https://www.msys2.org/) on Windows.  However, at least for now, even though PortableGL and all the
examples and demos are cross platform, I don't officially support building them on other platforms.  I've thought about
removing the premake scripts from the repo entirely and just leaving the Makefiles to make that clearer but decided not to
for the benefit of those who want to modify it for themselves to handle different platforms and build systems.

To sum up, the only thing that should be guaranteed to build and run anywhere out of the box with no extra effort on your part
are the regression tests since they don't depend on anything.

Modifying
=========
portablegl.h (and portablegl_unsafe.h) is generated in the src subdirectory with the python script generate_gl_h.py.
You can see how it's put together and either modify the script to leave out or add files, or actually edit any of the code.
Make sure if you edit gl_impl.c that you also edit gl_impl_unsafe.c.

Additionally, there is a growing set of more formal tests in /testing, one set of regression/feature tests, and one for
performance.  If you make any changes to core algorithms or data structures, you should definitely run those and make
sure nothing broke or got drastically slower.  The demos can also function as performance tests, so if one of those
would be especially affected by a change, it might be worth comparing its before/after performance too.

On the other hand, if you're adding a function or feature that doesn't really affect anything else, it might be worth
adding your own test if applicable.  You can see how they work from looking at the code, but I'll add more details and
documentation about the testing system later when it's more mature.


References
==========

While I often used the official OpenGL documentation to make sure I was matching the spec as closely as realistically
possible, what I used most, especially early on were a few textbooks.

The first was [Fundamentals of Computer Graphics 3rd Edition](https://amzn.to/2UIyAor) which I used
extensively early on to understand all the math involved, including the matrix transformation pipeline, barycentric coordinates
and interpolation, texture mapping and more.  There is now a [4th Edition](https://amzn.to/3quOj69) and a soon to be released
[5th Edition](https://amzn.to/2U6nEkh).

The second was the 5th edition of the [OpenGL Superbible](https://amzn.to/3hcbLAS).  I got this in 2010, right
after OpenGL 3.3/4.0 was released, and used it for my college graphics course mentioned above.  A lot of people didn't like
this book because they thought it relied too much on the author`s helper libraries but I had no problems.  It was my first
exposure to any kind of OpenGL so I didn't have to unlearn the old stuff and all his code was free and available online so
it was easy to look inside and not only see what actual OpenGL calls are used, but to then develop
your own classes to your own preferences.  I still use a
[class](https://raw.githubusercontent.com/rswinkle/PortableGL/master/glcommon/rsw_glframe.h)
based on his [GLFrame](https://raw.githubusercontent.com/rswinkle/oglsuperbible5/master/Src/GLTools/include/GLFrame.h)
class for example.

In any case, that's the book I actually learned OpenGL from, and still use as a reference sometimes.  I have a fork of
the [book repo](https://github.com/rswinkle/oglsuperbible5) too that I occasionally look at/update.  Of course they've come
out with a [6th](https://amzn.to/3qF0iOZ) and a [7th edition](https://amzn.to/2UBRbCt) in the last decade.

Lastly, while I haven't used it as much since I got it years later, the
[OpenGL 4.0 Shading Language Cookbook](https://amzn.to/3h7P0hI) has been useful in specific OpenGL topics occasionally.
Once again, you can now get the expanded [3rd edition](https://amzn.to/3qtatWr).

Bindings/Ports
==============

[pgl](https://github.com/TotallyGamerJet/pgl) is a Go port using [CXGO](https://github.com/gotranspile/cxgo), and hand
translating the individual examples/demos.


Similar/Related Projects
========================
I'll probably add others to this list as I find them.

[TinyGL](https://bellard.org/TinyGL/) is Fabrice Bellard's implementation of a subset of OpenGL 1.x.  If you want something like PortableGL
but don't want to write shaders, just want old style glBegin/glEnd/glVertex etc. this is the closest I know of.  Also I shamelessly copied his
clipping code because I'm not 1/10th the programmer Bellard was even as an undergrad and I knew it would "just work".

[TinyGL Updated](https://github.com/C-Chads/tinygl): An updated and cleaned up version of TinyGL that adds several fixes and features, including performance
tuning and limited multithreading.

[Pixomatic](http://www.radgametools.com/cn/pixofeat.htm) is/was a software implementation of D3D 7 and 9 written in C and assembly by Michael Abrash
and Mike Sartain.  You can read a [series](https://www.drdobbs.com/architecture-and-design/optimizing-pixomatic-for-x86-processors/184405765)
[of](https://www.drdobbs.com/optimizing-pixomatic-for-modern-x86-proc/184405807)
[articles](https://www.drdobbs.com/optimizing-pixomatic-for-modern-x86-proc/184405848) about it written by Abrash for Dr. Dobbs.

[TTSIOD](https://www.thanassis.space/renderer.html) is an advanced software renderer written in C++.

As an aside, the way I handle interpolation in PortableGL works as a semi-rebuttal of [this article](https://www.thanassis.space/cpp.html).
The answer is not the terrible strawman C approach he comes up with just to easily say "look how bad that is".  The answer is that interpolation is
an algorithm, a simple function, and it doesn't care what the data means or how many elements there are.  Pass it data and let the
algorithm do its job, same as graphics hardware does.  While the inheritance + template functions method works ok if you only have a few "types" of data,
every time you think of some new feature you want to interpolate, you need to define a new struct and a new template function specialization.
Having a function/pipeline that just takes an arbitrary amount of float data to operate on takes less code *and* even has less runtime overhead
since it's a single function that interpolates all the features at once rather than having to call a function for each feature.  See lines ~1200-1250
of gl_internals.c.  Obviously it looks more complicated with all the other openGL stuff going on but you can see a simpler version on line 308 that's
used for interpolating between line endpoints instead of over a triangle.  This is closer to his example but still longer because it has to support
SMOOTH, PERSPECTIVE and FLAT.  You can see the shape of a straightforward implementation even there though, and the benefits of decoupling the
algorithm from the data it operates on.

[Mesa3D](https://mesa3d.org/) is an open source implementation of OpenGL, Vulkan and other graphics APIs.  It includes several different software renderers including the Gallium rasterizer (softpipe or llvmpipe depending on whether llvm is used) and Intel's OpenSWR.

[libosmesa](https://github.com/starseeker/osmesa) is an extraction and clean-up of the subset of Mesa3D 7.0.4 needed to support the old "swrast"
software rasterizer and OSMesa offscreen renderer (both of which were removed from upstream Mesa3D in 2020.)  libosmesa's intended purpose is to
allow programs to offer a last-resort fallback renderer when system OpenGL capabilities are unavailable (or inadequate), while being fully self
contained and buildable as simple C with no external dependencies (which is why the 7.0.4 version was chosen for ths purpose.)  As that version
of Mesa targets OpenGL v2.1, libosmesa may be a useful middle ground between PortableGL's OpenGL 3.x target and TinyGL's minimalist subset.

[bbgl](https://github.com/graphitemaster/bbgl) is just a very interesting concept.  When I first saw it soon after it was published I was very frightened that it was exactly what PortableGL is but far more polished and from a better programmer.  Fortunately, it is not.

[pixman](http://pixman.org/) I feel like you could use them together or combine useful parts of pixman with PortableGL.

[fauxgl](https://github.com/fogleman/fauxgl) "3D software rendering in pure Go.  No OpenGL, no C extensions, no nothin'."

[swGL](https://github.com/h0MER247/swGL) A GPL2 multithreaded software implementation of OpenGL 1.3(ish) in C++. x86 and Windows only.

[SoftGLRender](https://github.com/keith2018/SoftGLRender) A OpenGL renderer/rasterizer in modern C++.  The very impressive
demo lets you toggle and change various features and settings including switching seamlessly between the GPU and the software
renderer.  AFAIK, this is the only project that lets you write shaders in C++ like PortableGL, however the process is more complicated
since it has to work with the same class/API structure as real OpenGL and can't take the shortcuts that PortableGL does.
An advantage of all the extra plumbing and complexity is the actual C++ shader code is more standardized, looks a little cleaner, and more like GLSL.
Besides supporting many more features (though I didn't see gl_FragDepth?), it is also optimized with threading, and SIMD on x86.
Given all the abstraction and extra overhead necessary to do what it does, it's impressive it manages to still be as fast as it is.
As best I can tell, disabling features to get vaguely close to an apples to apples comparison, it's about as fast as PortableGL.
So if you want OpenGL, don't need to use C, like modern C++/OOP style, and want software rendering while still being able to write your
own shaders and avoid the old fixed-function mess, this might be the best option on this list (or a generic C++ OpenGL wrapper library
linked with modern Mesa).


LICENSE
=======
PortableGL is licensed under the MIT License (MIT)

The code used for clipping is copyright (c) Fabrice Bellard from TinyGL also under
the MIT License, see LICENSE.


TODO/IDEAS
==========
- [ ] Render to texture; do I bother with FBOs/Renderbuffers/PixelBuffers etc.? See ch 8 of superbible 5
- [x] Multitexture (pointsprites and shadertoy) and texture array (Texturing) examples
- [ ] Render to texture example program
- [x] Mapped buffers according to API (just wraps extensions; it's free and everything is really read/write)
- [x] Extension functions that avoid unecessary copying, ie user owns buffer/texture data and gl doesn't free
- [x] Unsafe mode (ie no gl error checking for speedup)
- [ ] ~~Finish duplicating NeHe style tutorial programs from [learningwebgl](https://github.com/rswinkle/webgl-lessons) to [opengl_reference](https://github.com/rswinkle/opengl_reference) and then porting those to use PortableGL~~ Port [learnopengl](https://learnopengl.com/) instead, repo [here](https://github.com/rswinkle/LearnPortableGL) WIP.
- [x] Port medium to large open source game project as correctness/performance/API coverage test (Craft done)
- [x] Fix bug in cubemap demo
- [ ] More texture and render target formats
- [ ] Logo
- [ ] Update premake scripts to Premake5 and handle other platforms once 5 is out of beta
- [x] Formal regression testing (WIP)
- [x] Formal performance testing (WIP)
- [ ] Formal/organized documentation
- [x] Integrated documentation, license etc. a la stb libraries

