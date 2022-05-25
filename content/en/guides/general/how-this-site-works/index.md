+++
authors = [ "dan" ]
categories = [ "This Site" ]
description = "A guide to how this very developer document site works."
keywords = [ "authoring", "writing", "editing", "overview" ]
languages = [ "Markdown", "Kramdown", "YAML" ]
sdk = [ "General" ]
title = "How This Site Works"
type = "guides"
weight = 7

[admin]
TODO = ""
origin = ""
picky_sisters = ""
state = ""

[included_in]
platforms = [ "Windows", "Mac" ]
since = 0

[page_options]
byline = true
toc = true
toc_type = "single"

+++


{{< call-out "warning" "Transition underway" >}}
This site is undergoing a transition from Jekyll to Hugo. This page is out-of-date while this warning is in place.
{{< /call-out >}}

[This site](http://developer.rhino3d.com) is hosted on [GitHub Pages](https://pages.github.com/).  Every time a commit is made to [this git repository](https://github.com/mcneel/developer-rhino3d-com), a static site-generator called [Jekyll](http://jekyllrb.com/) churns through all the markdown content to generate html for the site.  Behind the scenes, Jekyll uses a templating language called [Liquid](https://shopify.github.io/liquid/), which allows for automatic generation of some content based upon yaml fields or page contents.


## Workflow

The best way to understand how this site works is to make a change to it.  Follow these steps:

1. If you have not already, read the [Getting Started with Dev Docs](https://github.com/mcneel/developer-rhino3d-com/blob/main/README.md) guide.  This guide will get you setup building the entire site locally on your computer so you can preview changes before making them live (by committing).
1. With the site up and running on your localhost, make a change to one of the pages (find a typo...there are many).  A good editor for Markdown is the [Atom editor](https://atom.io/).  Once you save your changes to the .md file, wait a moment, then refresh the localhost site in your browser to preview your change.
1. If you are satisfied with your change, use git to commit your change to the GitHub repository (or submit a pull-request for review).
1. Wait a couple minutes - this site is large and it may take a minute or two for Jekyll to process all the markdown and render the html contents.  (If you issued a pull-request, your change won't be live until a git administrator accepts it).
1. On the live [developer.rhino3d.com](http://developer.rhino3d.com), you should see your change.

### Versions

This site is version aware.  What does this mean?  First, open these two links in two separate tabs:

[http://developer.rhino3d.com/guides/general/how-this-site-works/](http://developer.rhino3d.com/guides/general/how-this-site-works/)

and

[http://developer.rhino3d.com/wip/guides/general/how-this-site-works/](http://developer.rhino3d.com/wip/guides/general/how-this-site-works/)

Notice the banner along the top of the "WIP" version of the site.  This tells the reader that they are on the version of the site that corresponds to information found in the Rhino Work-In-Progress (WIP) version of the software.  The "undecorated" ([http://developer.rhino3d.com](http://developer.rhino3d.com)) version of the site represents the current, stable, shipping version of Rhino.  (Legacy versions of documentation for legacy Rhino will be versioned accordingly.)

How does this work for the author, developer, or contributor?  The current (_stable_) version Rhino is defined in the `stable:` field in the `_config.yml` file.  The `n` branch that corresponds to the stable release is built and deployed to [http://developer.rhino3d.com](http://developer.rhino3d.com).  

The `master` branch represents the WIP version of the site.  Changes to the `master` branch will be built by Travis and deployed to [http://developer.rhino3d.com/wip](http://developer.rhino3d.com/wip).  

Branches are defined in `_config.yml` (under `version_branches`) and will be built and deployed into a subpath with the same name; for example: the `5` branch is built and deployed to [http://developer.rhino3d.com/5](http://developer.rhino3d.com/5).

So, if you want to author a guide that applies to the current (_stable_) version of Rhino, you should work on the branch marked `stable` branch in the `_config.yml` file.  If you would like to author a guide that applies to the RhinoWIP, then you should work in the `master` branch.

#### Change multiple versions

What if you want to write a guide that applies to both the current stable version of Rhino *and* the RhinoWIP?  

The easiest way is to work in the `master` branch and then you can [cherry-pick](https://git-scm.com/docs/git-cherry-pick) your commit from the `master` branch into the stable `n` branch as well.

#### Typos

What if you want to just correct a typo in both the `master` and the stable `n` versions of the site?  

In this case, the fastest and easiest way is to simply correct the typo in both branches on the GitHub site itself.  For example, if you wanted to correct a typo on [this very guide](https://github.com/mcneel/developer-rhino3d-com/blob/main/content/en/guides/general/how-this-site-works/index.md), just click the "Edit this file" - the little pencil icon in the upper right-hand corner.  Make your correction, then switch to the `wip` branch and do the same thing.  For small fixes, this works just as well as a [cherry-picked](https://git-scm.com/docs/git-cherry-pick) commit.

## Markdown & Kramdown

Use the [Style Guide](/guides/general/developer-docs-style-guide/) guide as a reference when writing content for this site.

Nearly all content on this site uses [Markdown](http://daringfireball.net/projects/markdown/basics) as the base format.  We are using the [Kramdown](http://kramdown.gettalong.org/quickref.html) markdown parser, which is the default parser with Jekyll.  A complete guide to Markdown and Kramdown is beyond the scope of this guide.  For markdown syntax, refer to the [Kramdown Quick Reference](http://kramdown.gettalong.org/quickref.html) or use other files on this site as examples.

## Layout

Each page on this site is generated by Liquid using a layout.  These layouts are found in the `/_layouts/` folder.

You determine which layout should be used by typing the layout's title in the `layout: ` yaml field at the top of the markdown file.

For example, this page uses the `toc-guide-page.html` layout, so you will find...

```yaml
layout: toc-guide-page
```

...at the top of this file.

Layouts can (and do) inherit from each other.  You will notice that the parent layout for all other layouts is `bootstrap.html`.

## Types of content

There are 4 types of content on this site:

 1. [Pages](#pages)
 1. [Guides](#guides)
 1. [APIs](#apis)
 1. [Samples](#samples)

All types of content - with the exception of APIs - begin with YAML, which the site uses to categories and sort the content into appropriate areas.


### Pages

Pages are interspersed throughout the site.  The [Welcome page](/), the [Guides page](/guides), and the [Samples page](/samples), are all examples of Pages.

Here is an example of the YAML for [a page](/guides/):

```yaml
---
layout: toc-page
title: Guides
permalink: /guides/
---
```

The YAML fields for Pages determine:

* *layout*: The layout html file used by Liquid (found in `/_layouts/`) on the page.
* *title*: This is the title of the page.  This is the html page title.
* *order*: The relative sort-order of this page in any collection of pages.


### Guides

Guides are contained in the `/_guide_topics/` directory.  This very document you are reading is a Guide.

To create a new guide, simply create a new markdown file and place it in the `/_guide_topics/` folder.  The file must have a `.md` file extension and begin with some YAML that is used to determine what it is and where it should be placed on the site.

Here is an example of the YAML for this guide:

```yaml
---
title: How This Site Works
description: A guide to how this very developer document site works.
authors: ['dan_rigdon_bel']
sdk: ['General']
languages: ['Markdown', 'Kramdown', 'YAML']
platforms: ['Mac', 'Windows']
categories: ['General']
origin: unset
order: 2
keywords: ['authoring', 'writing', 'editing', 'overview']
layout: toc-guide-page
---
```

The YAML fields for Guides determine:

* *title*: This is the title of the guide.  This is the html page title.
* *description*: This is a brief description of the guide.
* *authors*: The original - or responsible - author(s).  These values are in the _authors.yml_ file in */_data*.
* *sdk*: The Rhino SDK(s) that this guide pertains to.
* *languages*: The programming languages this guide references.
* *platforms*: The operating systems this guide is relevant to.
* *categories*: The category of the guide (General, Overview, Advanced).
* *origin*: If this guide was ported from another location, the URL backline to the origin(al) page.
* *order*: The relative sort-order of this guide in any list.
* *keywords*: Keywords related to this guide (un-used, as of yet).
* *layout*: The layout html file used by Liquid (found in `/_layouts/`) on the guide.

### APIs

The [API documentation](/api/) is automatically generated from source-code and cannot be edited "by hand."

### Samples

Samples are contained in the `/_samples/` directory.

Here is an example of the YAML for [this sample](/samples/cpp/add-a-cone-surface/):

```yaml
---
title: Add a Cone Surface
description: Demonstrates how to create a cone using ON_BrepCone.
authors: ['dale_fugier']
sdk: ['C/C++']
languages: ['C/C++']
platforms: ['Windows']
categories: ['Unsorted']
origin: http://wiki.mcneel.com/developer/sdksamples/addcone
order: 1
keywords: ['rhino']
layout: code-sample-cpp
---
```

The YAML fields for Samples determine:

* *title*: This is the title of the sample.  This is the html page title.
* *description*: A brief description of what the sample does.
* *authors*: The original - or responsible - author(s).  These values are in the _authors.yml_ file in */_data*.
* *sdk*: The Rhino SDK(s) that this sample pertains to.
* *languages*: The programming languages this sample references.
* *platforms*: The operating systems this sample is relevant to.
* *categories*: The category of the sample (Recipes, Snippet, Boilerplate, etc.).
* *origin*: If this sample was ported from another location, the URL backline to the origin(al) page.
* *order*: The relative sort-order of this sample in any list.
* *keywords*: Keywords related to this sample (un-used, as of yet).
* *layout*: The layout html file used by Liquid (found in `/_layouts/`) on the sample.

## Authors

To add a new author to the site, you must do three things:

1. Add an entry to the */_data/authors.yml* file with the appropriate fields filled in.
1. Duplicate one of the markdown.md files in the */_authors* folder and use the `name:` value that you added to the */_data/authors.yml* file in step 1 above.
1. Use the author's `name:` value as a string in the `authors: ['your_new_author_name']` yaml entries in guides or samples.

That's it.  *NOTE*: If you are building the site locally, results may be cached so a new author might not show up in the list.  You can force a full rebuild of the site by deleting the */_site* folder.

## TODO & origin fields

Many of the pages, guides, and samples have a `TODO` and `origin` yaml field.  These fields are used by this site to [report content](/admin/index.html#todo-list) that:

- Needs review
- Has not yet been authored
- Needs to be ported from another source

#### TODO

If `TODO` is present and set to `some string` (e.g.: `TODO: 'needs porting'`), the site will add this content to the [TODO list](/admin/index.html#todo-list).

If the TODO field is not present, the content will not be on the list.

#### origin

Much of this site is (or was) ported from a previous location.  The `origin` yaml field is reserved for a backlink to the original content.  If the `origin` yaml field is set to a URL - and `TODO` is set to `some value` - the content will show up on the [TODO list](/admin/index.html#todo-list) as needs porting from the `origin` URL.

## Jekyll Plugins

In general, we are attempting to stick to those [Jekyll dependencies that are ship with GitHub Pages](https://pages.github.com/versions/).  That said, since we are building this site with Travis CI, we can indeed use Jekyll plugins that are not part of the canonical GitHub pages dependencies.

To add a plugin to the site:

1. Verify that it is not in the [Dependency versions list](https://pages.github.com/versions/) already.
1. Test the plugin locally on your machine to make sure it does what you want it to do.
1. Add the plugin to the *Gemfile* on this site.
1. Add the plugin to the the *_config.yml* list under the `plugins:` entry.

## Related topics

- [Rhino Developer Docs Style Guide](/guides/general/developer-docs-style-guide/)
- [Jekyll Documentation](http://jekyllrb.com/docs/home/)
- [Liquid Docs](https://shopify.github.io/liquid/)
