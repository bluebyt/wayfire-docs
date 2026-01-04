Since the cycle of v0.11.0, the WCM is localised using gettext and can display localised metadata info for the plugins which provide suitable gettext catalogues. Translation catalogues are installed in `<PREFIX>/locale` and the WCM looks for a catalogue/domain named `wf-plugin-PLUGINNAME` for each plugin, which will provide translations for the option names, description and enum value names in the XML file; these can be extracted to a pot file with [itstool](//itstool.org) using the [Wayfire ITS schema](https://raw.githubusercontent.com/WayfireWM/wayfire/refs/heads/master/util/wayfire-itstool.xml):

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<its:rules xmlns:its="http://www.w3.org/2005/11/its"
           version="2.0">
    <its:translateRule translate="no" selector="//*"/>
    <its:translateRule translate="yes" selector="//_short"/>
    <its:translateRule translate="yes" selector="//_long"/>
    <its:translateRule translate="yes" selector="//_name"/>
</its:rules>
~~~

Example command:

~~~
itstool --its=SCHEMA-FILE.xml METADATA-FILE.xml -o OUTPUT.pot
~~~

## Translator's workflow

1. Create a new directory for the translation in the root of the source directory `locale/<LANGUAGE>/LC_MESSAGES/`. This will be the location of the .po file.
2. Extract your pot file as mentioned above.
3. Copy the .pot file as `wf-plugin-PLUGINNAME.po` in the newly created directory.
4. Edit the .po file to add the translated message strings.
5. Remove path comments (`#:` lines) from the po files:

    ~~~
    msgcat --no-location POFILE -o POFILE
    ~~~

    You can also use [the script](https://raw.githubusercontent.com/WayfireWM/wayfire/refs/heads/master/util/strip_po_comments.py) included in the Wayfire repository.

6. Remove the meson build directory and reinstall to test.

NOTE: Meson uses debug build type by default, which effectively disables translation installation for Wayfire. You may pass `-Dbuild_locales=enabled` as a meson option or set the build type `-Dbuildtype=release` to enable translation generation and installation. This does not apply to other repositories with translation support; they build locales unconditionally when configuring with meson.

7. Finally, add and commit `locale/<LANGUAGE>` directory with git.

To merge a pot with a po:

~~~
msgmerge -o YOUR_PO.po YOUR_PO.po YOUR_POT.pot
~~~

*It is mandatory to make a fresh POT and merge it into your PO whenever you update translations.* Otherwise the messages you translate won't be up to date and you might end up with missing new messages or translating ones that are no longer used.

Note to use the correct format as the basename of the catalogue files, so `pixdecor` has, for instance, `pixdecor/locale/ro_RO/LC_MESSAGES/wf-plugin-pixdecor.po`, which gets compiled to `<PREFIX>/locale/ro_RO/LC_MESSAGES/wf-plugin-pixdecor.mo`.

Don't generate a mo because they will be made at build time and also don't commit the pot. You can use the usual gettext commands or a graphical editor such as [Poedit](//poedit.org) which will greatly simplify navigating the po files.

To translate wcm, do a similar process but use xgettext normally, extracting from all cpp and hpp files in the source directory.

Command to extract a pot from WCM's own UI:

~~~
xgettext --language=C++ --verbose -k_ -o /tmp/wcm.pot src/*.cpp src/*.hpp
~~~

For repositories of many plugins, the Wayfire repository provides an example python script (`util/make_pots.py`) that can be used to extract each XML file to a POT and then you can use them normally, but remember not to commit them.

## For plugin authors

The xml metadata files remain in English and the catalogues can be added for each translation independently, by simply adding the .po file in the appropriate directory and the build system will pick it up. This means that plugin authors can work independently without worrying about translations and translators can add .po files, only needing to merge with a newly generated .pot file if some options change in the xml. That is, unless your plugin displays user-visible messages somewhere else in which case it is recommended to use the same catalogue (with dgettext) and instruct translators to also merge the translations from xgettext.

If you want to use locales for a standalone plugin installation, you have to install the catalogues (compiled to mo) in `<PREFIX>/locale/<LANGUAGE>/LC_MESSAGES`. See the `locale/meson.build` in Wayfire, WCM or Pixdecor repositories for an example. Then follow the steps above.
