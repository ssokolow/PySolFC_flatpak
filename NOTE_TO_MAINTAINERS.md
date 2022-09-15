# Note To Maintainers

Regarding `io.sourceforge.pysolfc.PySolFC.json`:

1. `tkinter.json` is simply copied verbatim from
   <https://github.com/iwalton3/tkinter-standalone> with `x-checker-data`
   sections added (I've opened
   [a bug](https://github.com/iwalton3/tkinter-standalone/issues/4) about them
   being absent) because Tkinter is sort of in limbo in the Freedesktop runtime,
   with Python being too common a dependency to omit, but Tkinter being too rare
   a dependency for its size to be included by default, and both being part of
   the same source package.
2. `python3-modules.json` was produced by running the
   [flatpak-pip-generator](https://github.com/flatpak/flatpak-builder-tools/blob/master/pip/flatpak-pip-generator)
   script as
   `python3 flatpak-pip-generator --checker-data attrs configobj pillow pycotap 'pygame>=2' ttkthemes pysol-cards`
3. `*.a` cannot be added to the `cleanup` list because `libmodplug.a` is needed
   at runtime by PyGame for music playback to work.

`x-checker-data` serves two purposes:

1. If you've set up Flathub as a package source with ID `flathub` (what the
   quick start instructions guide you through), then you can
   `flatpak install flathub org.flathub.flatpak-external-data-checker` to
   install `flatpak-external-data-checker` locally and then
   `flatpak run org.flathub.flatpak-external-data-checker io.sourceforge.pysolfc.PySolFC.json`
   whenever you want to automatically check for new releases of your
   dependencies.

   (Flatpak treats command-line packages sort of like how Rust treats
   `cargo install`. They don't show up in the web catalogue and are meant more
   as a means for distributing developer tools. I wrote
   [a script](https://gist.github.com/ssokolow/db565fd8a82d6002baada946adb81f68)
   which retrofits regular command names onto them.)

2. Flathub's bot on GitHub will use it to automatically detect when your
   dependencies get updated and submit PRs to bump your release. Since Flathub
   will also run a buildbot run on any PRs, you can incorporate whatever
   automated testing you want into your Flatpak build process and then have the
   pass/fail show up right in the PR to streamline evaluating version bumps.