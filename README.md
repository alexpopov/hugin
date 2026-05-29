# Hugin — unofficial GitHub mirror

This is an **unofficial mirror** of the Hugin panorama stitcher source code,
maintained for convenience by [@alexpopov](https://github.com/alexpopov).

- **Upstream (canonical) repository:** https://sourceforge.net/p/hugin/hugin/
  — Mercurial (`hg`), actively maintained.
- **Mirror snapshot date:** 2026-05-29
- **Pull requests should go upstream**, not here. The `default` branch in this
  mirror tracks upstream `default` and is intended to stay in sync. The
  `macos-arm64-build` branch carries patches needed to build a native
  Apple Silicon `.app` (see [build notes](#macos-arm64-build-notes) below).

The original upstream `README` is preserved verbatim as
[`README.md.orig`](./README.md.orig) so this `README.md` can be regenerated
cleanly when re-mirroring.

---

## About Hugin

Hugin is a toolchain to create panoramic images, from quick holiday snaps to
images with hundreds of megapixels. It is **GPLv2-or-later** licensed (see
[`COPYING.txt`](./COPYING.txt)).

For a full overview of the included programs (hugin, nona, fulla,
autooptimiser, enblend/enfuse, cpfind, and many more) see the
[upstream README](./README.md.orig) or the
[Hugin wiki](http://wiki.panotools.org/Hugin).

## macOS arm64 build notes

The Homebrew cask is deprecated as of 2025-11 (x86_64-only, runs under
Rosetta, deadlocks on macOS 14+ inside the Tip-of-the-Day startup dialog).
The `macos-arm64-build` branch in this mirror contains two small patches
needed to produce a native Apple Silicon `.app`:

1. **Bump `CMAKE_OSX_DEPLOYMENT_TARGET` from 10.9 to 11.0** so that
   `std::filesystem` (used in `src/hugin_base/hugin_utils/utils.cpp`) is
   available.
2. **Decouple bundle tool lookup from `MAC_SELF_CONTAINED_BUNDLE`** so that
   builds against Homebrew dependencies can still find their own bundled CLI
   tools (cpfind, nona, enblend) inside `Hugin.app/Contents/MacOS/` —
   without having to enable the full self-contained bundling flow which is
   broken on modern macOS.

Both patches are minimal (single-file or guard-only changes) and intended
to be upstreamable. They are *not* a fork — they exist only because the
official build path doesn't currently produce a working arm64 binary.

A full build recipe (Homebrew deps, vigra, enblend/enfuse, post-install
rpath/codesign) lives outside this repo at:
- `~/dots/docs/hugin/build-from-source-macos.md` in the maintainer's
  dotfiles.

## Contributing

**Do not send PRs to this mirror.** Send patches upstream to the
[SourceForge project](https://sourceforge.net/p/hugin/hugin/). Issues filed
here will be redirected.

The only PR-able surface is the `macos-arm64-build` branch — feel free to
suggest fixes for the build process there.

## License

GPLv2-or-later, identical to upstream. See [`COPYING.txt`](./COPYING.txt).
