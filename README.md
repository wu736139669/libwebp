          __   __  ____  ____  ____
         /  \\/  \/  _ \/  _ )/  _ \
         \       /   __/  _  \   __/
          \__\__/\____/\_____/__/ ____  ___
                / _/ /    \    \ /  _ \/ _/
               /  \_/   / /   \ \   __/  \__
               \____/____/\_____/_____/____/v0.5.1

Description:
============

WebP codec: library to encode and decode images in WebP format. This package
contains the library that can be used in other programs to add WebP support,
as well as the command line tools 'cwebp' and 'dwebp'.

See http://developers.google.com/speed/webp

The latest source tree is available at
https://chromium.googlesource.com/webm/libwebp

It is released under the same license as the WebM project.
See http://www.webmproject.org/license/software/ or the
"COPYING" file for details. An additional intellectual
property rights grant can be found in the file PATENTS.

Building:
=========

Windows build:
--------------

By running:

  nmake /f Makefile.vc CFG=release-static RTLIBCFG=static OBJDIR=output

the directory output\release-static\(x64|x86)\bin will contain the tools
cwebp.exe and dwebp.exe. The directory output\release-static\(x64|x86)\lib will
contain the libwebp static library.
The target architecture (x86/x64) is detected by Makefile.vc from the Visual
Studio compiler (cl.exe) available in the system path.

Unix build using makefile.unix:
-------------------------------

On platforms with GNU tools installed (gcc and make), running

  make -f makefile.unix

will build the binaries examples/cwebp and examples/dwebp, along
with the static library src/libwebp.a. No system-wide installation
is supplied, as this is a simple alternative to the full installation
system based on the autoconf tools (see below).
Please refer to makefile.unix for additional details and customizations.

Using autoconf tools:
---------------------
Prerequisites:
A compiler (e.g., gcc), make, autoconf, automake, libtool.
On a Debian-like system the following should install everything you need for a
minimal build:
$ sudo apt-get install gcc make autoconf automake libtool

When building from git sources, you will need to run autogen.sh to generate the
configure script.

./configure
make
make install

should be all you need to have the following files

/usr/local/include/webp/decode.h
/usr/local/include/webp/encode.h
/usr/local/include/webp/types.h
/usr/local/lib/libwebp.*
/usr/local/bin/cwebp
/usr/local/bin/dwebp

installed.

Note: A decode-only library, libwebpdecoder, is available using the
'--enable-libwebpdecoder' flag. The encode library is built separately and can
be installed independently using a minor modification in the corresponding
Makefile.am configure files (see comments there). See './configure --help' for
more options.

Building for MIPS Linux:
------------------------
MIPS Linux toolchain stable available releases can be found at:
https://community.imgtec.com/developers/mips/tools/codescape-mips-sdk/available-releases/

# Add toolchain to PATH
export PATH=$PATH:/path/to/toolchain/bin

# 32-bit build for mips32r5 (p5600)
HOST=mips-mti-linux-gnu
MIPS_CFLAGS="-O3 -mips32r5 -mabi=32 -mtune=p5600 -mmsa -mfp64 \
  -msched-weight -mload-store-pairs -fPIE"
MIPS_LDFLAGS="-mips32r5 -mabi=32 -mmsa -mfp64 -pie"

# 64-bit build for mips64r6 (i6400)
HOST=mips-img-linux-gnu
MIPS_CFLAGS="-O3 -mips64r6 -mabi=64 -mtune=i6400 -mmsa -mfp64 \
  -msched-weight -mload-store-pairs -fPIE"
MIPS_LDFLAGS="-mips64r6 -mabi=64 -mmsa -mfp64 -pie"

./configure --host=${HOST} --build=`config.guess` \
  CC="${HOST}-gcc -EL" \
  CFLAGS="$MIPS_CFLAGS" \
  LDFLAGS="$MIPS_LDFLAGS"
make
make install

CMake:
------
The support for CMake is minimal: it only helps you compile libwebp, cwebp and
dwebp.

Prerequisites:
A compiler (e.g., gcc with autotools) and CMake.
On a Debian-like system the following should install everything you need for a
minimal build:
$ sudo apt-get install build-essential cmake

When building from git sources, you will need to run cmake to generate the
configure script.

mkdir build && cd build && cmake ../
make
make install

If you also want cwebp or dwebp, you will need to enable them through CMake:

cmake -DWEBP_BUILD_CWEBP=ON -DWEBP_BUILD_DWEBP=ON ../

or through your favorite interface (like ccmake or cmake-qt-gui).

Gradle:
-------
The support for Gradle is minimal: it only helps you compile libwebp, cwebp and
dwebp and webpmux_example.

Prerequisites:
A compiler (e.g., gcc with autotools) and gradle.
On a Debian-like system the following should install everything you need for a
minimal build:
$ sudo apt-get install build-essential gradle

When building from git sources, you will need to run the Gradle wrapper with the
appropriate target, e.g. :

./gradlew buildAllExecutables

SWIG bindings:
--------------

To generate language bindings from swig/libwebp.swig at least swig-1.3
(http://www.swig.org) is required.

Currently the following functions are mapped:
Decode:
  WebPGetDecoderVersion
  WebPGetInfo
  WebPDecodeRGBA
  WebPDecodeARGB
  WebPDecodeBGRA
  WebPDecodeBGR
  WebPDecodeRGB

Encode:
  WebPGetEncoderVersion
  WebPEncodeRGBA
  WebPEncodeBGRA
  WebPEncodeRGB
  WebPEncodeBGR
  WebPEncodeLosslessRGBA
  WebPEncodeLosslessBGRA
  WebPEncodeLosslessRGB
  WebPEncodeLosslessBGR

See swig/README for more detailed build instructions.

Java bindings:

To build the swig-generated JNI wrapper code at least JDK-1.5 (or equivalent)
is necessary for enum support. The output is intended to be a shared object /
DLL that can be loaded via System.loadLibrary("webp_jni").

Python bindings:

To build the swig-generated Python extension code at least Python 2.6 is
required. Python < 2.6 may build with some minor changes to libwebp.swig or the
generated code, but is untested.

Encoding tool:
==============

The examples/ directory contains tools for encoding (cwebp) and
decoding (dwebp) images.

The easiest use should look like:
  cwebp input.png -q 80 -o output.webp
which will convert the input file to a WebP file using a quality factor of 80
on a 0->100 scale (0 being the lowest quality, 100 being the best. Default
value is 75).
You might want to try the -lossless flag too, which will compress the source
(in RGBA format) without any loss. The -q quality parameter will in this case
control the amount of processing time spent trying to make the output file as
small as possible.

A longer list of options is available using the -longhelp command line flag:

> cwebp -longhelp
Usage:
 cwebp [-preset <...>] [options] in_file [-o out_file]

If input size (-s) for an image is not specified, it is
assumed to be a PNG, JPEG, TIFF or WebP file.

Options:
  -h / -help ............. short help
  -H / -longhelp ......... long help
  -q <float> ............. quality factor (0:small..100:big)
  -alpha_q <int> ......... transparency-compression quality (0..100)
  -preset <string> ....... preset setting, one of:
                            default, photo, picture,
                            drawing, icon, text
     -preset must come first, as it overwrites other parameters
  -z <int> ............... activates lossless preset with given
                           level in [0:fast, ..., 9:slowest]

  -m <int> ............... compression method (0=fast, 6=slowest)
  -segments <int> ........ number of segments to use (1..4)
  -size <int> ............ target size (in bytes)
  -psnr <float> .......... target PSNR (in dB. typically: 42)

  -s <int> <int> ......... input size (width x height) for YUV
  -sns <int> ............. spatial noise shaping (0:off, 100:max)
  -f <int> ............... filter strength (0=off..100)
  -sharpness <int> ....... filter sharpness (0:most .. 7:least sharp)
  -strong ................ use strong filter instead of simple (default)
  -nostrong .............. use simple filter instead of strong
  -partition_limit <int> . limit quality to fit the 512k limit on
                           the first partition (0=no degradation ... 100=full)
  -pass <int> ............ analysis pass number (1..10)
  -crop <x> <y> <w> <h> .. crop picture with the given rectangle
  -resize <w> <h> ........ resize picture (after any cropping)
  -mt .................... use multi-threading if available
  -low_memory ............ reduce memory usage (slower encoding)
  -map <int> ............. print map of extra info
  -print_psnr ............ prints averaged PSNR distortion
  -print_ssim ............ prints averaged SSIM distortion
  -print_lsim ............ prints local-similarity distortion
  -d <file.pgm> .......... dump the compressed output (PGM file)
  -alpha_method <int> .... transparency-compression method (0..1)
  -alpha_filter <string> . predictive filtering for alpha plane,
                           one of: none, fast (default) or best
  -exact ................. preserve RGB values in transparent area
  -blend_alpha <hex> ..... blend colors against background color
                           expressed as RGB values written in
                           hexadecimal, e.g. 0xc0e0d0 for red=0xc0
                           green=0xe0 and blue=0xd0
  -noalpha ............... discard any transparency information
  -lossless .............. encode image losslessly
  -near_lossless <int> ... use near-lossless image
                           preprocessing (0..100=off)
  -hint <string> ......... specify image characteristics hint,
                           one of: photo, picture or graph

  -metadata <string> ..... comma separated list of metadata to
                           copy from the input to the output if present.
                           Valid values: all, none (default), exif, icc, xmp

  -short ................. condense printed message
  -quiet ................. don't print anything
  -version ............... print version number and exit
  -noasm ................. disable all assembly optimizations
  -v ..................... verbose, e.g. print encoding/decoding times
  -progress .............. report encoding progress

Experimental Options:
  -jpeg_like ............. roughly match expected JPEG size
  -af .................... auto-adjust filter strength
  -pre <int> ............. pre-processing filter

The main options you might want to try in order to further tune the
visual quality are:
 -preset
 -sns
 -f
 -m

Namely:
  * 'preset' will set up a default encoding configuration targeting a
     particular type of input. It should appear first in the list of options,
     so that subsequent options can take effect on top of this preset.
     Default value is 'default'.
  * 'sns' will progressively turn on (when going from 0 to 100) some additional
     visual optimizations (like: segmentation map re-enforcement). This option
     will balance the bit allocation differently. It tries to take bits from the
     "easy" parts of the picture and use them in the "difficult" ones instead.
     Usually, raising the sns value (at fixed -q value) leads to larger files,
     but with better quality.
     Typical value is around '75'.
  * 'f' option directly links to the filtering strength used by the codec's
     in-loop processing. The higher the value, the smoother the
     highly-compressed area will look. This is particularly useful when aiming
     at very small files. Typical values are around 20-30. Note that using the
     option -strong/-nostrong will change the type of filtering. Use "-f 0" to
     turn filtering off.
  * 'm' controls the trade-off between encoding speed and quality. Default is 4.
     You can try -m 5 or -m 6 to explore more (time-consuming) encoding
     possibilities. A lower value will result in faster encoding at the expense
     of quality.

Decoding tool:
==============

There is a decoding sample in examples/dwebp.c which will take
a .webp file and decode it to a PNG image file (amongst other formats).
This is simply to demonstrate the use of the API. You can verify the
file test.webp decodes to exactly the same as test_ref.ppm by using:

 cd examples
 ./dwebp test.webp -ppm -o test.ppm
 diff test.ppm test_ref.ppm

The full list of options is available using -h:

> dwebp -h
Usage: dwebp in_file [options] [-o out_file]

Decodes the WebP image file to PNG format [Default]
Use following options to convert into alternate image formats:
  -pam ......... save the raw RGBA samples as a color PAM
  -ppm ......... save the raw RGB samples as a color PPM
  -bmp ......... save as uncompressed BMP format
  -tiff ........ save as uncompressed TIFF format
  -pgm ......... save the raw YUV samples as a grayscale PGM
                 file with IMC4 layout
  -yuv ......... save the raw YUV samples in flat layout

 Other options are:
  -version ..... print version number and exit
  -nofancy ..... don't use the fancy YUV420 upscaler
  -nofilter .... disable in-loop filtering
  -nodither .... disable dithering
  -dither <d> .. dithering strength (in 0..100)
  -alpha_dither  use alpha-plane dithering if needed
  -mt .......... use multi-threading
  -crop <x> <y> <w> <h> ... crop output with the given rectangle
  -resize <w> <h> ......... scale the output (*after* any cropping)
  -flip ........ flip the output vertically
  -alpha ....... only save the alpha plane
  -incremental . use incremental decoding (useful for tests)
  -h ........... this help message
  -v ........... verbose (e.g. print encoding/decoding times)
  -quiet ....... quiet mode, don't print anything
  -noasm ....... disable all assembly optimizations

Visualization tool:
===================

There's a little self-serve visualization tool called 'vwebp' under the
examples/ directory. It uses OpenGL to open a simple drawing window and show
a decoded WebP file. It's not yet integrated in the automake build system, but
you can try to manually compile it using the recommendations below.

Usage: vwebp in_file [options]

Decodes the WebP image file and visualize it using OpenGL
Options are:
  -version ..... print version number and exit
  -noicc ....... don't use the icc profile if present
  -nofancy ..... don't use the fancy YUV420 upscaler
  -nofilter .... disable in-loop filtering
  -dither <int>  dithering strength (0..100), default=50
  -noalphadither disable alpha plane dithering
  -mt .......... use multi-threading
  -info ........ print info
  -h ........... this help message

Keyboard shortcuts:
  'c' ................ toggle use of color profile
  'i' ................ overlay file information
  'q' / 'Q' / ESC .... quit

Building:
---------

Prerequisites:
1) OpenGL & OpenGL Utility Toolkit (GLUT)
  Linux:
    $ sudo apt-get install freeglut3-dev mesa-common-dev
  Mac + XCode:
    - These libraries should be available in the OpenGL / GLUT frameworks.
  Windows:
    http://freeglut.sourceforge.net/index.php#download

2) (Optional) qcms (Quick Color Management System)
  i. Download qcms from Mozilla / Chromium:
    http://hg.mozilla.org/mozilla-central/file/0e7639e3bdfb/gfx/qcms
    http://src.chromium.org/viewvc/chrome/trunk/src/third_party/qcms
  ii. Build and archive the source files as libqcms.a / qcms.lib
  iii. Update makefile.unix / Makefile.vc
    a) Define WEBP_HAVE_QCMS
    b) Update include / library paths to reference the qcms directory.

Build using makefile.unix / Makefile.vc:
$ make -f makefile.unix examples/vwebp
> nmake /f Makefile.vc CFG=release-static \
    ../obj/x64/release-static/bin/vwebp.exe

Animated GIF conversion:
========================
Animated GIF files can be converted to WebP files with animation using the
gif2webp utility available under examples/. The files can then be viewed using
vwebp.

Usage:
 gif2webp [options] gif_file -o webp_file
Options:
  -h / -help ............. this help
  -lossy ................. encode image using lossy compression
  -mixed ................. for each frame in the image, pick lossy
                           or lossless compression heuristically
  -q <float> ............. quality factor (0:small..100:big)
  -m <int> ............... compression method (0=fast, 6=slowest)
  -min_size .............. minimize output size (default:off)
                           lossless compression by default; can be
                           combined with -q, -m, -lossy or -mixed
                           options
  -kmin <int> ............ min distance between key frames
  -kmax <int> ............ max distance between key frames
  -f <int> ............... filter strength (0=off..100)
  -metadata <string> ..... comma separated list of metadata to
                           copy from the input to the output if present
                           Valid values: all, none, icc, xmp (default)
  -mt .................... use multi-threading if available

  -version ............... print version number and exit
  -v ..................... verbose
  -quiet ................. don't print anything

Building:
---------
With the libgif development files installed, gif2webp can be built using
makefile.unix:
$ make -f makefile.unix examples/gif2webp

or using autoconf:
$ ./configure --enable-everything
$ make

Comparison of animated images:
==============================
Test utility anim_diff under examples/ can be used to compare two animated
images (each can be GIF or WebP).

Usage: anim_diff <image1> <image2> [options]

Options:
  -dump_frames <folder> dump decoded frames in PAM format
  -min_psnr <float> ... minimum per-frame PSNR
  -raw_comparison ..... if this flag is not used, RGB is
                        premultiplied before comparison

Building:
---------
With the libgif development files and a C++ compiler installed, anim_diff can
be built using makefile.unix:
$ make -f makefile.unix examples/anim_diff

or using autoconf:
$ ./configure --enable-everything
$ make

Encoding API:
=============

The main encoding functions are available in the header src/webp/encode.h
The ready-to-use ones are:
size_t WebPEncodeRGB(const uint8_t* rgb, int width, int height, int stride,
                     float quality_factor, uint8_t** output);
size_t WebPEncodeBGR(const uint8_t* bgr, int width, int height, int stride,
                     float quality_factor, uint8_t** output);
size_t WebPEncodeRGBA(const uint8_t* rgba, int width, int height, int stride,
                      float quality_factor, uint8_t** output);
size_t WebPEncodeBGRA(const uint8_t* bgra, int width, int height, int stride,
                      float quality_factor, uint8_t** output);

They will convert raw RGB samples to a WebP data. The only control supplied
is the quality factor.

There are some variants for using the lossless format:

size_t WebPEncodeLosslessRGB(const uint8_t* rgb, int width, int height,
                             int stride, uint8_t** output);
size_t WebPEncodeLosslessBGR(const uint8_t* bgr, int width, int height,
                             int stride, uint8_t** output);
size_t WebPEncodeLosslessRGBA(const uint8_t* rgba, int width, int height,
                              int stride, uint8_t** output);
size_t WebPEncodeLosslessBGRA(const uint8_t* bgra, int width, int height,
                              int stride, uint8_t** output);

Of course in this case, no quality factor is needed since the compression
occurs without loss of the input values, at the expense of larger output sizes.

Advanced encoding API:
----------------------

A more advanced API is based on the WebPConfig and WebPPicture structures.

WebPConfig contains the encoding settings and is not tied to a particular
picture.
WebPPicture contains input data, on which some WebPConfig will be used for
compression.
The encoding flow looks like:

-------------------------------------- BEGIN PSEUDO EXAMPLE

#include <webp/encode.h>

  // Setup a config, starting form a preset and tuning some additional
  // parameters
  WebPConfig config;
  if (!WebPConfigPreset(&config, WEBP_PRESET_PHOTO, quality_factor))
    return 0;   // version error
  }
  // ... additional tuning
  config.sns_strength = 90;
  config.filter_sharpness = 6;
  config_error = WebPValidateConfig(&config);  // not mandatory, but useful

  // Setup the input data
  WebPPicture pic;
  if (!WebPPictureInit(&pic)) {
    return 0;  // version error
  }
  pic.width = width;
  pic.height = height;
  // allocated picture of dimension width x height
  if (!WebPPictureAllocate(&pic)) {
    return 0;   // memory error
  }
  // at this point, 'pic' has been initialized as a container,
  // and can receive the Y/U/V samples.
  // Alternatively, one could use ready-made import functions like
  // WebPPictureImportRGB(), which will take care of memory allocation.
  // In any case, past this point, one will have to call
  // WebPPictureFree(&pic) to reclaim memory.

  // Set up a byte-output write method. WebPMemoryWriter, for instance.
  WebPMemoryWriter wrt;
  WebPMemoryWriterInit(&wrt);     // initialize 'wrt'

  pic.writer = MyFileWriter;
  pic.custom_ptr = my_opaque_structure_to_make_MyFileWriter_work;

  // Compress!
  int ok = WebPEncode(&config, &pic);   // ok = 0 => error occurred!
  WebPPictureFree(&pic);  // must be called independently of the 'ok' result.

  // output data should have been handled by the writer at that point.
  // -> compressed data is the memory buffer described by wrt.mem / wrt.size

  // deallocate the memory used by compressed data
  WebPMemoryWriterClear(&wrt);

-------------------------------------- END PSEUDO EXAMPLE

Decoding API:
=============

This is mainly just one function to call:

#include "webp/decode.h"
uint8_t* WebPDecodeRGB(const uint8_t* data, size_t data_size,
                       int* width, int* height);

Please have a look at the file src/webp/decode.h for the details.
There are variants for decoding in BGR/RGBA/ARGB/BGRA order, along with
decoding to raw Y'CbCr samples. One can also decode the image directly into a
pre-allocated buffer.

To detect a WebP file and gather the picture's dimensions, the function:
  int WebPGetInfo(const uint8_t* data, size_t data_size,
                  int* width, int* height);
is supplied. No decoding is involved when using it.

Incremental decoding API:
=========================

In the case when data is being progressively transmitted, pictures can still
be incrementally decoded using a slightly more complicated API. Decoder state
is stored into an instance of the WebPIDecoder object. This object can be
created with the purpose of decoding either RGB or Y'CbCr samples.
For instance:

  WebPDecBuffer buffer;
  WebPInitDecBuffer(&buffer);
  buffer.colorspace = MODE_BGR;
  ...
  WebPIDecoder* idec = WebPINewDecoder(&buffer);

As data is made progressively available, this incremental-decoder object
can be used to decode the picture further. There are two (mutually exclusive)
ways to pass freshly arrived data:

either by appending the fresh bytes:

  WebPIAppend(idec, fresh_data, size_of_fresh_data);

or by just mentioning the new size of the transmitted data:

  WebPIUpdate(idec, buffer, size_of_transmitted_buffer);

Note that 'buffer' can be modified between each call to WebPIUpdate, in
particular when the buffer is resized to accommodate larger data.

These functions will return the decoding status: either VP8_STATUS_SUSPENDED if
decoding is not finished yet or VP8_STATUS_OK when decoding is done. Any other
status is an error condition.

The 'idec' object must always be released (even upon an error condition) by
calling: WebPDelete(idec).

To retrieve partially decoded picture samples, one must use the corresponding
method: WebPIDecGetRGB or WebPIDecGetYUVA.
It will return the last displayable pixel row.

Lastly, note that decoding can also be performed into a pre-allocated pixel
buffer. This buffer must be passed when creating a WebPIDecoder, calling
WebPINewRGB() or WebPINewYUVA().

Please have a look at the src/webp/decode.h header for further details.

Advanced Decoding API:
======================

WebP decoding supports an advanced API which provides on-the-fly cropping and
rescaling, something of great usefulness on memory-constrained environments like
mobile phones. Basically, the memory usage will scale with the output's size,
not the input's, when one only needs a quick preview or a zoomed in portion of
an otherwise too-large picture. Some CPU can be saved too, incidentally.

-------------------------------------- BEGIN PSEUDO EXAMPLE
     // A) Init a configuration object
     WebPDecoderConfig config;
     CHECK(WebPInitDecoderConfig(&config));

     // B) optional: retrieve the bitstream's features.
     CHECK(WebPGetFeatures(data, data_size, &config.input) == VP8_STATUS_OK);

     // C) Adjust 'config' options, if needed
     config.options.no_fancy_upsampling = 1;
     config.options.use_scaling = 1;
     config.options.scaled_width = scaledWidth();
     config.options.scaled_height = scaledHeight();
     // etc.

     // D) Specify 'config' output options for specifying output colorspace.
     // Optionally the external image decode buffer can also be specified.
     config.output.colorspace = MODE_BGRA;
     // Optionally, the config.output can be pointed to an external buffer as
     // well for decoding the image. This externally supplied memory buffer
     // should be big enough to store the decoded picture.
     config.output.u.RGBA.rgba = (uint8_t*) memory_buffer;
     config.output.u.RGBA.stride = scanline_stride;
     config.output.u.RGBA.size = total_size_of_the_memory_buffer;
     config.output.is_external_memory = 1;

     // E) Decode the WebP image. There are two variants w.r.t decoding image.
     // The first one (E.1) decodes the full image and the second one (E.2) is
     // used to incrementally decode the image using small input buffers.
     // Any one of these steps can be used to decode the WebP image.

     // E.1) Decode full image.
     CHECK(WebPDecode(data, data_size, &config) == VP8_STATUS_OK);

     // E.2) Decode image incrementally.
     WebPIDecoder* const idec = WebPIDecode(NULL, NULL, &config);
     CHECK(idec != NULL);
     while (bytes_remaining > 0) {
       VP8StatusCode status = WebPIAppend(idec, input, bytes_read);
       if (status == VP8_STATUS_OK || status == VP8_STATUS_SUSPENDED) {
         bytes_remaining -= bytes_read;
       } else {
         break;
       }
     }
     WebPIDelete(idec);

     // F) Decoded image is now in config.output (and config.output.u.RGBA).
     // It can be saved, displayed or otherwise processed.

     // G) Reclaim memory allocated in config's object. It's safe to call
     // this function even if the memory is external and wasn't allocated
     // by WebPDecode().
     WebPFreeDecBuffer(&config.output);

-------------------------------------- END PSEUDO EXAMPLE

Bugs:
=====

Please report all bugs to the issue tracker:
    https://bugs.chromium.org/p/webp
Patches welcome! See this page to get started:
    http://www.webmproject.org/code/contribute/submitting-patches/

Discuss:
========

Email: webp-discuss@webmproject.org
Web: http://groups.google.com/a/webmproject.org/group/webp-discuss
