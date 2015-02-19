# Jekyll-Multipage

by [Matt Gemmell](http://mattgemmell.com/)


## What is it?

It's a Jekyll generator plugin that splits posts across multiple pages, with extensive configuration options.


## What are its requirements?

All you need is Ruby, and of course [Jekyll](http://jekyllrb.com).


## What does it do?

This plugin lets you designate posts as **multipage**, which means those posts will be split into multiple separate pages below their permalinks.

You commonly see such multipage articles on the web in tutorials, or lengthy reviews with multiple sections, or even with stories split into chapters. With this plugin, you can write the entire article in a single Markdown file, and let the plugin split it up for you.

There are many configuration options available, to control how the plugin works.


## How do I use it?

First, install the plugin and its supporting files:

1. Install the templates first. They're called `multipage-paging.html` and `multipage-pagebreak.html`, and they go into your Jekyll site's `_includes` folder.

2. Install the plugin. It's the file called `multipage-generator.rb`, and it goes into your Jekyll site's `_plugins` folder.

You're now ready to create multipage posts. Read the [Getting started](#getting-started) section of this document for a quick guide.


## Who made it?

Matt Gemmell (that's me).

- My website is at [mattgemmell.com](http://mattgemmell.com)

- I'm on Twitter as [@mattgemmell](http://twitter.com/mattgemmell)

- This code is on github at [github.com/mattgemmell/Jekyll-Multipage](http://github.com/mattgemmell/Jekyll-Multipage)


## What license is the code released under?

Creative Commons [CC0 Universal (public domain)](https://creativecommons.org/publicdomain/zero/1.0/deed.en).


## Getting started

To mark a post as multipage, simply use the YAML front-matter:

`multipage: true`

You can then choose where the post is split into pages in two different ways:

1. Put a `{% page_break %}` tag wherever you want to take a new page.

2. Enable [auto-breaking at headings](#about-headings), and configure which heading-level you'd like to act as a page-break.

You can do both at the same time too.

Read the rest of this document for extensive customisation options, including [page-numbering](#page-numbers), [page titles](#page-titles), automatic [anchor-link rewriting](#anchor-links), and more.

Please note that, in each case, you can specify a customisation option in your Jekyll site's `_config.yml` file as well as in each specific post. A post's settings will override the site-wide setting, as you'd expect.


## The collated page

By default, the plugin will create a **collated page** for each multipage post. The collated page is just as it sounds: it's a single page containing the contents of all the other pages, gathered together.

This can be useful for your readers, if they want to print the entire article at once, for example.

You can customise several aspects of the collated page:

- You can disable the collated page entirely by setting `multipage_collation_enabled` to `false`.

- You can choose the slug (partial path, below your post's own path) for the collated page via the `multipage_collated_page_slug` setting. The default is `all`.

- You can move the collated page to the post's root URL by setting `multipage_collate_on_root_page` to `true`. See also the section of this document on [page numbering](#page-numbers).

- You can choose the title of the collated page via the `multipage_collated_page_title_format` setting. Within the value for that setting, you can use the `:post_title` token, which will be replaced with the title of the original post.

By default, the collated page has the same title as the original post.

When the collated page is assembled, the **page-break template** is inserted between each sub-page. A default template is included with the plugin (in the `_includes` folder), but you can specify another template via the `multipage_pagebreak_template` setting.

As with every setting, you can override this on a per-post basis, and also set a site-wide default.


## Page numbers

By default, the plugin will number your pages logically, as follows:

- The first page will be at the post's root URL (permalink).

- Subsequent pages will be at `/2`, `/3`, and so forth.

- The [collated page](#the-collated-page) will be at `/all`.

As stated in the previous section, you can choose for the collated page to be at the post's root URL - in this case, the first page will be at `/1` instead.

You can customise this behaviour in various ways.

- You can choose the first page number via the `multipage_first_page_number` setting, if you wish numbering to begin at something other than 1.

- You can choose the page slug (partial path) via the `multipage_page_slug_format` setting. Within the value for that setting, you can use the token `:num`, and it will be replaced with the proper page number.


## Page titles

By default, titles use both the post title and the page number. For example, if your post is called **"Just a test"**, your pages will be called:

- Just a test
- Just a test - Page 2
- Just a test - Page 3
- etc.

It's important to understand that pages actually have **two** titles: the **page title**, and the **expanded page title**. In the above example, "Just a test - Page 2" is the expanded title, and the page title is "Page 2".

The reason for this is convenience when linking between pages. You might, for example, want to add previous/next navigation links between pages (see the [paging](#paging) section for more about that), and it would be awkward to have the full post title in that context. However, you wouldn't want a page's actual HTML title to just be "Page 2".

Thus, there are two titles for each page: the expanded title which includes the original post's title, and the page-specific title.

You can customise page titles in various ways:

- You can choose the expanded title format via the `multipage_page_title_expanded_format` setting. You can use the tokens `:post_title`, `:page_title`, and `:num`.

- You can choose the (non-expanded) page title format via the `multipage_page_title_format` setting. You can use the tokens `:post_title` and `:num`.

- By default, the root URL of your post contains the first page. You probably don't want the root page to have "Page 1" (or similar) appended to it, so by default the plugin will use the original post's title for first pages that are at the root URL. If you instead want root-URL first pages to follow the same title options as other pages, you can set `multipage_root_first_page_uses_post_title` to `false`.

- You can choose the title of the [collated page](#the-collated-page). See that section of this document for more.

- If you've enabled [auto page-breaks at headings](#about-headings), you can also choose to have the headers override their page's titles. See that section of this document for more.

You can also override titles on a **per-page** basis, via the `{% page_title "foo" %}` and `{% expanded_page_title "bar" %}` tags. Use them in the relevant section of your post's original Markdown file, and they'll affect the page that they fall on.

Each tag can use the same tokens as its corresponding setting, as described above.


## Paging

Multipage posts will usually have special navigation between the pages. A common style is to have links for the first and last pages, the previous and next pages, and individual links to each numbered page (and perhaps also a link to the collated page). These navigational constructs are called **paging**.

This is all handled for you. By default, the plugin will add suitable paging to the top and bottom of every page, including the collated page. You can customise this behaviour in various ways:

- You can choose a paging template, either site-wide or a per-post basis. A default template is included with this plugin (in the `_includes` folder), but you can also choose your own via the `multipage_paging_template` setting. Refer to the default template for guidance.

- If you don't wish pages to have paging automatically added to them, you can disable that feature by setting `multipage_auto_wrap_paging` to `false`.

- You can disable automatically-added paging on just the **collated page** by setting `multipage_paging_on_collated_page` to `false`.

- If you would prefer that the automatic paging only be added to the top (or only to the bottom) of each page, you can set the `multipage_auto_wrap_type` setting to either `header` or `footer` respectively. The default value is `both`.

In all cases, you can manually insert the paging template into your page as usual, via Liquid's `include` tag. For convenience, you can also use the `{% paging %}` tag.


## About headings

You may find it convenient to automatically break your post into pages at certain headings, either instead of or alongside the `{% page_break %}` tag. This feature is called **auto page-breaks**, and is disabled by default.

When enabled, your post will be split into pages just before each HTML heading (i.e. `h2` etc tag) of a given level. You can enable this feature by setting `multipage_auto_pagebreak_at_headings` to `true`. By default, posts will be split at `h2` headings.

Note that this feature works not just with literal HTML heading tags, but also with both of the types of heading syntax that Markdown supports.

You can choose which level of headings to auto-pagebreak on via the `multipage_auto_pagebreak_heading_level` setting. The value should be a number, usually between 1 and 6.

By default, the titles of auto-pagebroken pages will be set to the heading that the page was broken on. This will often be what you want. If you'd like to disable this feature, set `multipage_use_headings_for_page_titles` to `false`.


## Anchor links

In HTML (and Markdown), it's possible to link to **anchors** on the same page, allowing in-page navigation. This is commonly used for references, tables of contents, and so on.

Logically, anchor links which worked on a single-page post will no longer work when that post is split across multiple pages. Thus, this plugin can automatically **rewrite anchor links** for you. This feature is enabled by default.

Anchor link rewriting works with links of the form `#anchor-name`, i.e. those beginning with a `#` symbol. It will work not just with HTML links, but also Markdown-style links. It will _also_ work with anchors that link to kramdown-style extended ID attributes.

If you'd like to disable this feature, you can set `multipage_rewrite_anchor_links` to `false`. Naturally, anchors will _not_ be rewritten on the **collated page**.

You may also wish to manually cause an anchor link to be rewritten, when necessary. You can do so with the `{% page_link x %}` tag, as follows.

Put the tag immediately before the `#` symbol in an anchor link, for example:

- `<a href="{% page_link 3 %}#my-anchor">link text</a>`

- `[link text]({% page_link 3 %}#my-anchor)`

The resulting link will be rewritten on non-collated pages, to go to the specified destination page. Note that there are two types of value you can specify in the tag:

- A bare number, for example **3**, is a **zero-based page index**, i.e. the first page is **0**, and so on. The collated page is not included, regardless of whether it's at the post's root URL or not. This is a programmer-centric value, which you may find easier to work with if you're so inclined.

- A number prefixed with `n`, for example **n3**, is a **page number** according to your [page-numbering](#page-numbers) settings. This is a human-centric value, and corresponds to the displayed page numbers.

For example, if your page-numbering is set to start at **1**, then using the value **n1** would yield the **first** page, and using the value **1** (without the `n`), would yield the **second** page (because the index of the first page is zero).

Finally, note that the auto-rewriting feature will deliberately ignore any anchor-links which are adorned with a `{% page_link %}` tag.


## Multipage data

There's a wealth of useful multipage-related data available to your posts, and to any templates that you use within a multipage-enabled post. You can access it via the `page.multipage` object, for example:

`{{ page.multipage.total_pages }}` and `{{ page.multipage.page_titles }}`

To see all the data that's available, enable the multipage feature for a post, and put the `{{ page.multipage }}` tag somewhere in the post's content.


## Things to note

Some things to be aware of:

1. On index pages of your site, Jekyll displays an excerpt from your post (usually up to the `<!--more-->` marker). Be aware that, with multipage posts, Jekyll generates the excerpt _before_ the post is split into multiple pages. If you need to override the default excerpt, you can use the `excerpt` YAML front-matter setting in your post, as usual.

2. Your site's feed(s) will have always have the content of the _root_ page of a multipage post, whether that root page is the post's first page, or its collated page. If you need more customised behaviour, you can of course modify your feed template.


## Can you provide support for this code?

Unfortunately not. If you find a bug, please fix it and [submit a pull request on github](https://github.com/mattgemmell/Jekyll-Multipage/pulls).


## I have a feature request or bug report.

Please [create an issue on github](https://github.com/mattgemmell/Jekyll-Multipage/issues).


## How can I thank you?

You can:

- [Support my writing](http://mattgemmell.com/support-me/).

- Check out [my Amazon wishlist](http://www.amazon.co.uk/registry/wishlist/1BGIQ6Z8GT06F).

- Say thanks [on Twitter](http://twitter.com/mattgemmell).
