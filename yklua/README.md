# cdreduce/cvise support for yklua

This directory contains stuff to help with reducing the yklua codebase to find
small test cases.

It works by preprocessing the yklua source code into one self-contained file
(`onelua.c`), which you can then reduce with `creduce` or `cvise`.

## Usage

FIXME: These instructions were from when I had this directory inside the yklua
source tree. We'll need to adapt this to make it work with an "externally
situated" yklua source tree. Maybe you have to set some env var pointing to the
src or similar.

 - `export YK_BUILD_TYPE=<debug|release>`
 - `export PATH=/path/to/yk/bin:$PATH`
 - `make onelua.c`
 - `cp interest.sh.example interest.sh`
 - edit `interest.sh` and adapt it for your needs.
 - `cvise interest.sh onelua.c`

Note that it's best not to use cvise's built-in timeout support. cvise will
send `SIGTERM`, which often isn't enough to kill yklua. Instead use
`timeout(1)` (in your interest script) to send `SIGKILL` on timeout.
https://github.com/marxin/cvise/issues/145

Note that `onelua.c` is mutated in place, so if you need to start reduction
again from scratch, run `make clean && make onelua.c`.
