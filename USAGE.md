The BoringSSL library needs to be compiled with 

```
cmake -B build -DCMAKE_BUILD_TYPE=Release -DCMAKE_POSITION_INDEPENDENT_CODE=ON -DOPENSSL_NO_ASM=ON -DBORINGSSL_SHARED_LIBRARY=ON -DBUILD_SHARED_LIBS=1
```
in order to successfully compile curl-impersonate on Android. 
If you try and force shoving down the statically compiled BoringSSL libraries, you will get "discouraged" by libtool first with the warning that the library is not portable, then full stop comes to the compilation.

But even if you complete the compilation by the shared libraries, it'll try resolving them into the system's libraries. You can override this using the `LD_PRELOAD` trick.

So to perform this, you'll need the BoringSSL libraries you've used during compilation then do 

```
LD_PRELOAD=boringssl-libs/crypto.so:boringssl-libs/ssl.so curl-impersonate
```
Then, it should work just as usual, but with every launch you'll need the `LD_PRELOAD` addition before the executable's name. 

To persistently create a shortcut that makes it ready to use, you'll likely going to need a wrapper that does it.

Also, do not attempt to replace the system libs, you'll break the whole coreutils suite if you'll try replacing the prefix's `libcrypto` library, from which you can only recover from using the failsafe launch or starting from scratch.
