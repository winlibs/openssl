# OpenSSL Windows, How to compile

## Building OpenSSL 0.9.8

### Requirements

  * Openssl Sources ([http://openssl.org/source/](http://openssl.org/source/))

  * Native Perl from ActiveState ([http://www.activestate.com/Products/activeperl/](http://www.activestate.com/Products/activeperl/)) installed and perl being in your PATH

  * Common tools used to compile PHP

### Configure

Configure for Win32

    
    cd  C:\phpbuild\libs\openssl-0.9.8k
    perl Configure --openssldir=C:/phpbuild/apps_install/ VC-WIN32

Configure for Win64

    
    cd  C:\phpbuild\libs\openssl-0.9.8k
    perl Configure --openssldir=C:/phpbuild/apps_install/ VC-WIN64A

The configure will prepare the sources to compile OpenSSL for windows 32bit
and install it under c:\phpbuilds\apps.

Modify the path using your configurations. Please note the unix directory
separators / instead of the Windows backslash \.

The following steps are only for 0.9.8k, later versions have been fixed.

    
    The headers are not compatible with VC9 which seems to be more picky. The openssl/ossl_typ.h header has a forward typedef that reads:
    
    typedef struct ocsp_response_st OCSP_RESPONSE;
    
    ocsp_response_st is defined in openssl/ocsp.h, this typedef has to be move there.
    
    bufferoverflowu.lib is also not present anymore (not needed) in VC9. Comment out the reference in the util/pl/VC-32.pl script on line 142, 276 and 286.

#### Assembly languages options

PHP build default in 5.2 don't use ASM. PHP build default in 5.3 and later
uses ASM.

    
    For 0.9.8k you have to modify  the file crypto\perlasm\x86ms.pl. On the line 273 remove "$extra".

For Win64 builds, use only:

    
    ms\do_win64a

For win32 builds only:

If you don't want to use assembly language:

    
    ms\do_ms

If you like to use the assembly language files:

If you have MASM (aka “ml”), run:

    
    ms\do_masm

Or if you use NASM:

    
    ms\do_nasm

You should not get any error until now. If you see errors messages, please
check the troubleshooting section in “INSTALL.W32” in the OpenSSL sources.

And for win64:

    
    ms\do_win64a

## Compiling

    
    nmake -f ms\ntdll.mak

If you prefer to build a static library, use this command:

    
    nmake -f ms\nt.mak

If you get the following compilation error (with other letters):

    
    .\crypto\cversion.c(105) : warning C4129: 'p' : unrecognized character escape sequence
    .\crypto\cversion.c(105) : warning C4129: 'l' : unrecognized character escape sequence

Go back to the configure phase and be sure to use only slashed and no
backslashes for –openssldir (or –prefix)

## Testing

The possible point of failures in the OpenSSL implementations are numerous.
Thanks to their tests suite, it is possible to minimize the risk by running it
after each update:

    
    nmake -f ms\ntdll.mak test

## Install

    
    nmake -f ms\ntdll.mak install

or

    
    nmake -f ms\nt.mak install

if you built the static library.

### Copy OpenSSL 0.9.8 development files in the PHP SDK

To be done

### Copy OpenSSL 0.9.8 in the PHP release Template

To be done