# curl-impersonate-termux

This version, based on [lambdamechanic's fork](https://github.com/lambdamechanic/curl-impersonate) compiles under Termux.

# Differences between the original and this version
Most of the features are left as is, but after a series of investigations, I've made some adjustments to the `Makefile.in` file:
* The BoringSSL library is sourced from [lexiforest](https://github.com/lexiforest/boringssl/tree/673e61fc215b178a90c0e67858bbf162c8158993), which is already patched for use with curl-impersonate, so, no patching needed.
* The BoringSSL library's compiled result's folder, `lib` was specified instead instead of the libraries themselves.
* When compiling a program as statically linked, it is against the C standard to pack another static library into another, so libtool and the compiler itself will refuse to make such an action.
* So the solution was to build BoringSSL as shared and the relevant flag for it. And to make sure that the compiled BoringSSL library gets picked up instead, the library directory was set by the variables accordingly.
* Tests are disabled for `libcares`, as they're not needed and sometimes the compiler on Termux errors out when they're enabled.

# Limitations
* To get a proper compile on Android, ECH has been disabled. If there's a way to get that working, you can edit the `Makefile.in` file, rerun `./configure` to generate new Makefile then use `make build` to compile.
