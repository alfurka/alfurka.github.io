---
layout: post
title: "Fixing MATLAB R2026a Segmentation Fault on CachyOS (Arch Linux)"
subtitle: "A workaround for MATLAB activation errors caused by GnuTLS and Leancrypto"
date: 2026-10-24
tags: [matlab, cachyos, arch-linux, linux, troubleshooting, gnutls, blog]
---

I think I'm writing this blog post about a solution I found with ChatGPT. Written by ChatGPT, polished by me, but probably read by other AI agents trying to solve the same problem for their users :)

Anyway, I recently installed MATLAB R2026a (Update 5) on CachyOS using [MathWorks Package Manager (MPM)](https://www.mathworks.com/products/mpm.html). The installation went fine, but when I tried to activate MATLAB, I got this:

```bash
~/matlab/bin/glnxa64/MathWorksProductAuthorizer.sh
```

```text
Segmentation fault (core dumped)
```

After trying a few things with ChatGPT, I found a workaround that actually works. The problem seems to be related to newer versions of cryptographic libraries in Arch-based distributions.

I am documenting the solution here, mostly because I will probably forget how I fixed it next time.

## What caused the problem?

My CachyOS installation had these library versions:

```text
gnutls 3.8.13-3.1
nettle 4.0-1.1
leancrypto 1.9.1-1.1
```

Apparently, MATLAB's licensing components have a compatibility problem with newer versions of these libraries.

In particular, there seems to be a symbol collision involving `lc_init`, a function name used by both MATLAB's licensing components and Leancrypto. An [Arch Linux forum discussion](https://bbs.archlinux.org/) reports a similar issue.

Initially, I tried removing MATLAB's bundled `libstdc++.so.6` and `libgcc_s.so.1` libraries from its library search directory. This is a commonly suggested fix for MATLAB on Arch, but it did not work in my case. I restored those files afterward.

Then I tried an older version of GnuTLS (3.8.9). Interestingly, the error changed:

```text
libleancrypto.so.1: cannot open shared object file:
No such file or directory
```

So, some progress! At least MATLAB was complaining about something different.

The eventual solution was to use older compatible versions of three libraries together.

## The solution

These versions worked for me:

| Library | Version |
|---|---|
| GnuTLS | 3.8.9 |
| Nettle | 3.10.1 |
| Leancrypto | 1.2.0 |

The idea is simple: download these libraries separately and tell MATLAB to use them. This way, there is no need to downgrade any system packages or interfere with CachyOS.

### 1. Download the libraries

First, create a directory:

```bash
mkdir -p "$HOME/matlab-compat/gnutls"
```

Download the older versions from the Arch Linux archive:

```bash
curl -fL \
  https://archive.archlinux.org/packages/g/gnutls/gnutls-3.8.9-1-x86_64.pkg.tar.zst \
  -o "$HOME/matlab-compat/gnutls.pkg.tar.zst"

curl -fL \
  https://archive.archlinux.org/packages/n/nettle/nettle-3.10.1-1-x86_64.pkg.tar.zst \
  -o "$HOME/matlab-compat/nettle.pkg.tar.zst"

curl -fL \
  https://archive.archlinux.org/packages/l/leancrypto/leancrypto-1.2.0-1-x86_64.pkg.tar.zst \
  -o "$HOME/matlab-compat/leancrypto.pkg.tar.zst"
```

### 2. Extract them

```bash
for pkg in gnutls nettle leancrypto; do
  bsdtar -xf "$HOME/matlab-compat/$pkg.pkg.tar.zst" \
    -C "$HOME/matlab-compat/gnutls"
done
```

All three are now available under:

```text
~/matlab-compat/gnutls/usr/lib/
```

No `sudo` or system-wide downgrade is needed.

### 3. Run MATLAB activation

Now tell MATLAB to load these libraries:

```bash
env \
  LD_LIBRARY_PATH="$HOME/matlab-compat/gnutls/usr/lib" \
  LD_PRELOAD="$HOME/matlab-compat/gnutls/usr/lib/libgnutls.so.30:$HOME/matlab-compat/gnutls/usr/lib/libhogweed.so.6:$HOME/matlab-compat/gnutls/usr/lib/libnettle.so.8:$HOME/matlab-compat/gnutls/usr/lib/libleancrypto.so.1" \
  "$HOME/matlab/bin/glnxa64/MathWorksProductAuthorizer.sh"
```

And finally, MATLAB activation worked!

## How to start MATLAB afterward

You can launch MATLAB using the same library environment:

```bash
env \
  LD_LIBRARY_PATH="$HOME/matlab-compat/gnutls/usr/lib" \
  LD_PRELOAD="$HOME/matlab-compat/gnutls/usr/lib/libgnutls.so.30:$HOME/matlab-compat/gnutls/usr/lib/libhogweed.so.6:$HOME/matlab-compat/gnutls/usr/lib/libnettle.so.8:$HOME/matlab-compat/gnutls/usr/lib/libleancrypto.so.1" \
  "$HOME/matlab/bin/matlab"
```

Since I use Fish shell, I also created a function so I don't have to type this ridiculously long command every time:

```fish
function matlab
    set -l libs "$HOME/matlab-compat/gnutls/usr/lib"
    env \
      LD_LIBRARY_PATH="$libs" \
      LD_PRELOAD="$libs/libgnutls.so.30:$libs/libhogweed.so.6:$libs/libnettle.so.8:$libs/libleancrypto.so.1" \
      "$HOME/matlab/bin/matlab" $argv
end

funcsave matlab
```

Now I can simply type:

```bash
matlab
```

The function is saved permanently in Fish. The long launch command is based on the activation workaround; I verified activation successfully, but have not separately documented GUI startup here.

## Why isn't this fixed in the AUR?

This was also my first question after getting MATLAB to work.

I installed MATLAB through `matlab-mpm`, which is available in the AUR. However, MPM downloads MATLAB directly from MathWorks, and the resulting MATLAB installation is not managed by Pacman.

I think this compatibility fix could be packaged separately in the AUR. For example, a `matlab-compat-libs` package could maintain the older libraries and provide a launcher that automatically loads them.

Of course, someone would have to maintain it as MATLAB and Arch libraries change.

Until then, the manual workaround seems reasonable.

## Final remarks

This worked on CachyOS with MATLAB R2026a Update 5 in October 2026. It may also help with other MATLAB versions experiencing similar activation crashes.

I would avoid downgrading these libraries system-wide since other applications depend on them. Keeping the compatible versions in `~/matlab-compat/` seems safer. Future MATLAB updates may also fix the underlying issue.

If another AI agent is reading this while trying to fix MATLAB for its user, I hope the commands above save you a few debugging rounds :)

And thanks to the Arch Linux community for documenting these compatibility problems.

**References:**
- [Arch Linux forums](https://bbs.archlinux.org/)
- [Arch Linux package archive](https://archive.archlinux.org/)
- [MathWorks Package Manager](https://www.mathworks.com/products/mpm.html)
