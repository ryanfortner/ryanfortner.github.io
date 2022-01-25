---
template: overrides/main.html
---

# Changing the logo and icons

When installing Material for MkDocs, you immediately get access to _over 8.000 
icons_ ready to be used for customization of specific parts of the theme and/or 
when writing your documentation in Markdown. Not enough? You can also add
[additional icons] with minimal effort.

  [additional icons]: #additional-icons

## Configuration

### Logo

[:octicons-tag-24: 0.1.0][logo support] ·
:octicons-milestone-24: Default: [`material/library`][logo default]

The logo can be changed to a user-provided image (any type, incl. `*.png` and
`*.svg`) located in the `docs` folder, or to any icon bundled with the theme.
Add the following lines to `mkdocs.yml`:

=== ":octicons-image-16: Image"

    ``` yaml
    theme:
      logo: assets/logo.png
    ```

=== ":octicons-package-16: Icon, bundled"

    ``` yaml
    theme:
      icon:
        logo: material/library # (1)!
    ```

    1.  Enter a few keywords to find the perfect icon using our [icon search] and
        click on the shortcode to copy it to your clipboard:

        <div class="mdx-iconsearch" data-mdx-component="iconsearch">
          <input class="md-input md-input--stretch mdx-iconsearch__input" placeholder="Search icon" data-mdx-component="iconsearch-query" value="material library" />
          <div class="mdx-iconsearch-result" data-mdx-component="iconsearch-result" data-mdx-mode="file">
            <div class="mdx-iconsearch-result__meta"></div>
            <ol class="mdx-iconsearch-result__list"></ol>
          </div>
        </div>

  [logo support]: https://github.com/squidfunk/mkdocs-material/releases/tag/0.1.0
  [logo default]: https://github.com/squidfunk/mkdocs-material/blob/master/material/.icons/material/library.svg
  [icon search]: ../reference/icons-emojis.md#search

Normally, the logo in the header and sidebar links to the homepage of the
documentation, which is the same as `site_url`. This behavior can be changed
with the following configuration:

``` yaml
extra:
  homepage: https://example.com
```

### Favicon

[:octicons-tag-24: 0.1.0][favicon support] ·
:octicons-milestone-24: Default: [`assets/images/favicon.png`][favicon default]

The favicon can be changed to a path pointing to a user-provided image, which 
must be located in the `docs` folder. Add the following lines to `mkdocs.yml`:

``` yaml
theme:
  favicon: images/favicon.png
```

  [favicon support]: https://github.com/squidfunk/mkdocs-material/releases/tag/0.1.0
  [favicon default]: https://github.com/squidfunk/mkdocs-material/blob/master/material/assets/images/favicon.png

## Customization

### Additional icons

In order to use custom icons, [extend the theme] and create a new folder named
`.icons` in the [`custom_dir`][custom_dir] you want to use for overrides.
Next, add your `*.svg` icons into a subfolder of the `.icons` folder. Let's say
you  downloaded and unpacked the [Bootstrap] icon set, and want to add it to
your project documentation. The structure of your project should look like this:

``` sh
.
├─ overrides/
│  └─ .icons/
│     └─ bootstrap/
│        └─ *.svg
└─ mkdocs.yml
```

Then, add the following lines to `mkdocs.yml`:

``` yaml
markdown_extensions:
  - pymdownx.emoji:
      emoji_index: !!python/name:materialx.emoji.twemoji
      emoji_generator: !!python/name:materialx.emoji.to_svg
      options:
        custom_icons:
          - overrides/.icons
```

You can now use all :fontawesome-brands-bootstrap: Bootstrap icons.

  [extend the theme]: ../customization.md#extending-the-theme
  [custom_dir]: https://www.mkdocs.org/user-guide/configuration/#custom_dir
  [Bootstrap]: https://icons.getbootstrap.com/
