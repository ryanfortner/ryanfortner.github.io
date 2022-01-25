---
template: overrides/main.html
search:
  boost: 1.05
---

# Setting up site search

Material for MkDocs provides an excellent, client-side search implementation,
omitting the need for the integration of third-party services, which might
be tricky to integrate to be compliant with data privacy regulations. Moreover,
with some effort, search can be made available [offline].

  [offline]: #offline-search

## Configuration

### Built-in search

[:octicons-tag-24: 0.1.0][search support] ·
:octicons-cpu-24: Plugin

The built-in search plugin integrates seamlessly with Material for MkDocs,
adding multilingual client-side search with [lunr] and [lunr-languages]. It's 
enabled by default, but must be re-added to `mkdocs.yml` when other plugins
are used:

``` yaml
plugins:
  - search
```

The following configuration options are supported:

`lang`{ #search-lang }

:   :octicons-milestone-24: Default: _automatically set_ – This option allows
    to include the language-specific stemmers provided by [lunr-languages].
    Note that Material for MkDocs will set this automatically based on the
    [site language], but it may be overridden, e.g. to support multiple
    languages:

    === "A single language"

        ``` yaml
        plugins:
          - search:
              lang: ru
        ```

    === "Multiple languages"

        ``` yaml
        plugins:
          - search:
              lang: # (1)!
                - en
                - ru
        ```

        1.  Be aware that including support for other languages increases the
            general JavaScript payload by around 20kb (before `gzip`) and by
            another 15-30kb per language.

    The following languages are supported:

    <div class="mdx-columns" markdown>

    - `ar` – Arabic
    - `da` – Danish
    - `du` – Dutch
    - `en` – English
    - `fi` – Finnish
    - `fr` – French
    - `de` – German
    - `hu` – Hungarian
    - `it` – Italian
    - `ja` – Japanese
    - `no` – Norwegian
    - `pt` – Portuguese
    - `ro` – Romanian
    - `ru` – Russian
    - `es` – Spanish
    - `sv` – Swedish
    - `th` – Thai
    - `tr` – Turkish
    - `vi` – Vietnamese

    </div>

    Material for MkDocs goes to great lengths to support languages that are not
    part of this list by automatically falling back to the stemmer yielding the
    best result.

`separator`{ #search-separator }

:   :octicons-milestone-24: Default: _automatically set_ – The separator for
    indexing and query tokenization can be customized, making it possible to
    index parts of words separated by other characters than whitespace and `-`,
    e.g. by including `.`:

    ``` yaml
    plugins:
      - search:
          separator: '[\s\-\.]' # (1)!
    ```

    1.  Tokenization itself is carried out by [lunr's default tokenizer], which 
        doesn't allow for lookahead or separators spanning multiple characters.

        For more finegrained control over the tokenization process, see the
        section on [tokenizer lookahead].

<div class="mdx-deprecated" markdown>

`prebuild_index`{ #search-prebuild-index }

:   [:octicons-tag-24: 5.0.0][prebuilt index support] · :octicons-archive-24:
    Deprecated · :octicons-trash-24: 8.0.0 · :octicons-milestone-24: Default:
    `false` – MkDocs can generate a [prebuilt index] of all pages during
    build time, which provides performance improvements at the cost of more
    bandwidth, as it reduces the build time of the search index:

    ``` yaml
    plugins:
      - search:
          prebuild_index: true
    ```

    Note that this configuration option was removed, as the [new search
    plugin] generates up to [50% smaller] search indexes, doubling search
    performance.

    [:octicons-arrow-right-24: Read more on the new search plugin]
    [new search plugin]

</div>

The other configuration options of this plugin are not officially supported
by Material for MkDocs, which is why they may yield unexpected results. Use
them at your own risk.

  [search support]: https://github.com/squidfunk/mkdocs-material/releases/tag/0.1.0
  [lunr]: https://lunrjs.com
  [lunr-languages]: https://github.com/MihaiValentin/lunr-languages
  [lunr's default tokenizer]: https://github.com/olivernn/lunr.js/blob/aa5a878f62a6bba1e8e5b95714899e17e8150b38/lunr.js#L413-L456
  [site language]: changing-the-language.md#site-language
  [tokenizer lookahead]: #tokenizer-lookahead
  [prebuilt index support]: https://github.com/squidfunk/mkdocs-material/releases/tag/5.0.0
  [prebuilt index]: https://www.mkdocs.org/user-guide/configuration/#prebuild_index
  [50% smaller]: ../blog/2021/search-better-faster-smaller.md#benchmarks

### Rich search previews

[:octicons-heart-fill-24:{ .mdx-heart } Insiders][Insiders]{ .mdx-insiders } ·
[:octicons-tag-24: insiders-3.0.0][Insiders] ·
:octicons-beaker-24: Experimental

Insiders ships rich search previews as part of the [new search plugin], which
will render code blocks directly in the search result, and highlight all
occurrences inside those blocks:

=== "Insiders"

    ![search preview now]

=== "Material for MkDocs"

    ![search preview before]

  [Insiders]: ../insiders/index.md
  [new search plugin]: ../blog/2021/search-better-faster-smaller.md
  [search preview now]: ../blog/2021/search-better-faster-smaller/search-preview-now.png
  [search preview before]: ../blog/2021/search-better-faster-smaller/search-preview-before.png

### Tokenizer lookahead

[:octicons-heart-fill-24:{ .mdx-heart } Insiders][Insiders]{ .mdx-insiders } ·
[:octicons-tag-24: insiders-3.0.0][Insiders] ·
:octicons-beaker-24: Experimental

Insiders allows for more complex configurations of the [`separator`][separator] 
setting as part of the [new search plugin], yielding more influence on the way 
documents are tokenized:

``` yaml
plugins:
  - search:
      separator: '[\s\-,:!=\[\]()"/]+|(?!\b)(?=[A-Z][a-z])|\.(?!\d)|&[lg]t;'
```

The following section explains what can be achieved with tokenizer lookahead:

=== "Case changes"

    ```
    (?!\b)(?=[A-Z][a-z])
    ```

    `PascalCase` and `camelCase` are used as naming conventions in many
    programming languages. By adding this match group to the [`separator`]
    [separator], [words are split at case changes], tokenizing the word
    `PascalCase` into `Pascal` and `Case`, so both terms can be searched 
    individually.

    [:octicons-arrow-right-24: Read more on tokenizing case changes]
    [tokenize case changes]

=== "Version numbers"

    ```
    \.(?!\d)
    ```

    When `.` is added to the [`separator`][separator], version numbers would be
    split into parts, rendering them undiscoverable via search. By adding
    this match group, a small lookahead is introduced, so version numbers will
    remain as they are, and can be found through search.

    [:octicons-arrow-right-24: Read more on tokenizing version numbers]
    [tokenize version numbers]

=== "HTML/XML tags"

    ```
    &[lg]t;
    ```

    If your documentation includes HTML/XML code examples, you may want to allow
    users to find specific tag names. Unfortunately, the `<` and `>` control
    characters are encoded in code blocks as `&lt;` and `&gt;`. Adding this
    expression to the separator allows for just that.

    [:octicons-arrow-right-24: Read more on tokenizing HTML/XML tags]
    [tokenize html-xml tags]

  [separator]: #search-separator
  [words are split at case changes]: ?q=searchHighlight
  [tokenize case changes]: ../blog/2021/search-better-faster-smaller.md#case-changes
  [tokenize version numbers]: ../blog/2021/search-better-faster-smaller.md#version-numbers
  [tokenize html-xml tags]: ../blog/2021/search-better-faster-smaller.md#htmlxml-tags

### Search suggestions

[:octicons-tag-24: 7.2.0][search.suggest support] ·
:octicons-unlock-24: Feature flag ·
:octicons-beaker-24: Experimental

When search suggestions are enabled, the search will display the likeliest
completion for the last word which can be accepted with the ++arrow-right++ key.
Add the following lines to `mkdocs.yml`:

``` yaml
theme:
  features:
    - search.suggest
```

Searching for [:octicons-search-24: search su][search.suggest example] yields 
^^search suggestions^^ as a suggestion.

  [search.suggest support]: https://github.com/squidfunk/mkdocs-material/releases/tag/7.2.0
  [search.suggest example]: ?q=search+su

### Search highlighting

[:octicons-tag-24: 7.2.0][search.highlight support] ·
:octicons-unlock-24: Feature flag ·
:octicons-beaker-24: Experimental

When search highlighting is enabled and a user clicks on a search result,
Material for MkDocs will highlight all occurrences after following the link.
Add the following lines to `mkdocs.yml`:

``` yaml
theme:
  features:
    - search.highlight
```

Searching for [:octicons-search-24: code blocks][search.highlight example]
highlights all occurrences of both terms.

  [search.highlight support]: https://github.com/squidfunk/mkdocs-material/releases/tag/7.2.0
  [search.highlight example]: ../reference/code-blocks.md?h=code+blocks

### Search sharing

[:octicons-tag-24: 7.2.0][search.share support] ·
:octicons-unlock-24: Feature flag ·
:octicons-beaker-24: Experimental

When search sharing is activated, a :material-share-variant: share button is
rendered next to the reset button, which allows to deep link to the current
search query and result. Add the following lines to `mkdocs.yml`:

``` yaml
theme:
  features:
    - search.share
```

When a user clicks the share button, the URL is automatically copied to the
clipboard.

  [search.share support]: https://github.com/squidfunk/mkdocs-material/releases/tag/7.2.0

### Offline search

[:octicons-tag-24: 5.0.0][offline search support] ·
[:octicons-cpu-24: Plugin][localsearch]

If you distribute your documentation as `*.html` files, the built-in search
will not work out-of-the-box due to the restrictions modern browsers impose for
security reasons. This can be mitigated with the [localsearch] plugin in
combination with @squidfunk's [iframe-worker] polyfill.

For setup instructions, refer to the [localsearch documentation].

  [offline search support]: https://github.com/squidfunk/mkdocs-material/releases/tag/5.0.0
  [localsearch]: https://github.com/wilhelmer/mkdocs-localsearch/
  [iframe-worker]: https://github.com/squidfunk/iframe-worker
  [localsearch documentation]: https://github.com/wilhelmer/mkdocs-localsearch#installation-material-v5

!!! tip

    When distributing documentation as HTML files to be opened from the file
    system, you will also want to set `use_directory_urls: false` in
    `mkdocs.yml` to make page links function correctly.

## Usage

### Search boosting

[:octicons-heart-fill-24:{ .mdx-heart } Insiders][Insiders]{ .mdx-insiders } ·
[:octicons-tag-24: insiders-2.8.0][Insiders]

When [Metadata] is enabled, pages can be boosted in search with custom front
matter, which will make them rank higher. Add the following lines at the top of
a Markdown file:

``` bash
---
search:
  boost: 2 # (1)!
---

# Document title
...
```

1.  :woman_in_lotus_position: When boosting pages, be gentle and start with
    __low values__.

  [Metadata]: extensions/python-markdown.md#metadata

### Search exclusion

[:octicons-heart-fill-24:{ .mdx-heart } Insiders][Insiders]{ .mdx-insiders } ·
[:octicons-tag-24: insiders-3.1.0][Insiders] ·
:octicons-beaker-24: Experimental

When [Metadata] is enabled, pages can be excluded from search with custom front 
matter, removing them from the index. Add the following lines at the top of a 
Markdown file:

``` bash
---
search:
  exclude: true
---

# Document title
...
```

#### Excluding sections

When [Attribute Lists] is enabled, specific sections of pages can be excluded
from search by adding the `{ data-search-exclude }` pragma after a Markdown
heading:

=== ":octicons-file-code-16: docs/page.md"

    ``` markdown
    # Document title

    ## Section 1

    The content of this section is included

    ## Section 2 { data-search-exclude }

    The content of this section is excluded
    ```

=== ":octicons-codescan-16: search_index.json"

    ``` json
    {
      ...
      "docs": [
        {
          "location":"page/",
          "text":"",
          "title":"Document title"
        },
        {
          "location":"page/#section-1",
          "text":"<p>The content of this section is included</p>",
          "title":"Section 1"
        }
      ]
    }
    ```

  [Attribute Lists]: extensions/python-markdown.md#attribute-lists

#### Excluding blocks

When [Attribute Lists] is enabled, specific sections of pages can be excluded
from search by adding the `{ data-search-exclude }` pragma after a Markdown
inline- or block-level element:

=== ":octicons-file-code-16: docs/page.md"

    ``` markdown
    # Document title

    The content of this block is included

    The content of this block is excluded
    { data-search-exclude }
    ```

=== ":octicons-codescan-16: search_index.json"

    ``` json
    {
      ...
      "docs": [
        {
          "location":"page/",
          "text":"<p>The content of this block is included</p>",
          "title":"Document title"
        }
      ]
    }
    ```

## Customization

The search implementation of Material for MkDocs is probably its most
sophisticated feature, as it tries to balance a great typeahead experience,
good performance, accessibility, and a result list that is easy to scan.
This is where Material for MkDocs deviates from other themes.

The following section explains how search can be customized to tailor it to
your needs.

### Query transformation

When a user enters a query into the search box, the query is pre-processed
before it is submitted to the search index. Material for MkDocs will apply the
following transformations, which can be customized by [extending the theme]:

``` ts
export function defaultTransform(query: string): string {
  return query
    .split(/"([^"]+)"/g) /* (1)! */
      .map((terms, index) => index & 1
        ? terms.replace(/^\b|^(?![^\x00-\x7F]|$)|\s+/g, " +")
        : terms
      )
      .join("")
    .replace(/"|(?:^|\s+)[*+\-:^~]+(?=\s+|$)/g, "") /* (2)! */
    .trim() /* (3)! */
}
```

1.  Search for terms in quotation marks and prepend a `+` modifier to denote
    that the resulting document must contain all terms, converting the query
    to an `AND` query (as opposed to the default `OR` behavior). While users
    may expect terms enclosed in quotation marks to map to span queries, i.e.
    for which order is important, `lunr` doesn't support them, so the best
    we can do is to convert the terms to an `AND` query.

2.  Replace control characters which are not located at the beginning of the
    query or preceded by white space, or are not followed by a non-whitespace
    character or are at the end of the query string. Furthermore, filter
    unmatched quotation marks.

3.  Trim excess whitespace from left and right.

If you want to switch to the default behavior of the `mkdocs` and `readthedocs`
themes, both of which don't transform the query prior to submission, or
customize the `transform` function, you can do this by [overriding the
`config` block][overriding blocks]:

``` html
{% extends "base.html" %}

{% block config %}
  {{ super() }}
  <script>
    var __search = {
      transform: function(query) {
        return query
      }
    }
  </script>
{% endblock %}
```

The `transform` function will receive the query string as entered by the user
and must return the processed query string to be submitted to the search index.

  [extending the theme]: ../customization.md#extending-the-theme
  [overriding blocks]: ../customization.md#overriding-blocks

### Custom search

Material for MkDocs implements search as part of a [web worker]. If you
want to switch the web worker with your own implementation, e.g. to submit
search to an external service, you can add a custom JavaScript file to the
`docs` directory and [override the `config` block][overriding blocks]:

``` html
{% extends "base.html" %}

{% block config %}
  {{ super() }}
  <script>
    var __search = {
      worker: "<url>"
    }
  </script>
{% endblock %}
```

Communication with the search worker is implemented using a designated message
format using discriminated unions, i.e. through the `type` property of the
message. See the following interface definitions to learn about the message
formats:

- [:octicons-file-code-24: `SearchMessage`][SearchMessage]
- [:octicons-file-code-24: `SearchIndex` and `SearchResult`][SearchIndex]

The sequence and direction of messages is rather intuitive:

- :octicons-arrow-right-24: `SearchSetupMessage`
- :octicons-arrow-left-24: `SearchReadyMessage`
- :octicons-arrow-right-24: `SearchQueryMessage`
- :octicons-arrow-left-24: `SearchResultMessage`

  [web worker]: https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API/Using_web_workers
  [SearchMessage]: https://github.com/squidfunk/mkdocs-material/blob/master/src/assets/javascripts/integrations/search/worker/message/index.ts
  [SearchIndex]: https://github.com/squidfunk/mkdocs-material/blob/master/src/assets/javascripts/integrations/search/_/index.ts
