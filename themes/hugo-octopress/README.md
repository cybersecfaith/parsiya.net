# Hugo-Octopress
Hugo-Octopress is a port of the classic [Octopress][octopress-link] theme to [Hugo][hugo-link]. For a live demo of the website please see [http://parsiya.net](http://parsiya.net).

![screenshot](/images/tn.png)

## Contents
- [Config file parameters](#config)
- [Code highlight](#highlight)
- [Navigation menu](#menu)
- [Markdown options](#markdown)
- [Sidebar](#sidebarlinks)
- [Shortcodes](#shortcodes)
  - [Code caption](#codecaption)
  - [Image caption](#imgcap)
- [License page](#licensepage)
- [Issues/TODO](#issues)
- [Attribution](#attribution)
- [Ported by](#porterby)
- [Theme license](#themelicense)


## <a name="config"></a>Configuration
This section is about parameters in the [configuration file](https://gohugo.io/overview/configuration/) and how they can be used to customize the output. A working config file `sample-config.toml`is provided and parameters are explained below:

``` python
baseurl = "http://example.com/"
disablePathToLower = true
languageCode = "en-us"
theme = "hugo-octopress"

# this will appear in the header at the top of the page and in the landing page's title
title = "Website's title"

# number of blog posts displayed in each page
paginate = 6

# change the article links
[permalinks]
post = "/blog/:year-:month-:day-:title/"

# to generate tags and categories
# tags are generated using the template hugo-octopress/layouts/indexes/tags.html
# categories are generated using the template hugo-octopress/layouts/indexes/categories.html
# if you change anything here, rename the templates/directories accordingly
[indexes]
  tag = "tags"
  category = "categories"

[params]

  # number of recent posts that will be shown in the sidebar - default is 5
  SidebarRecentLimit = 5

  # if false, all of posts' content will appear on front page (and in pagination) - not recommended
  # be sure to use the <!--more--> delimiter
  truncate = true

  # author's name (this will appear in metadata and at the bottom of posts)
  author = "Site Author"

  # appears in the site header under website title
  subtitle = "Site Subtitle"

  # text of the Continue Reading label - if not set, it will default to "Read On &rarr;"
  # &rarr; == right arrow, but it gets messed up in the string so it is added to hugo-octopress/layouts/index.html manually
  # this can be modified in hugo-octopress/layouts/index.html
  continue_reading = "Would you like to know more?"

  # disqus - simply enter your disqus - using the template from https://gohugo.io/extras/comments/ at /hugo-octopress/layouts/partials/disqus.html that disables disqus when running on localhost (if you are testing it offline remember to comment out the if in the template that checks for localhost)
  # the template is injected into the pages in /hugo-octopress/layouts/partials/post-footer.html which is in every post (and not non-post pages like license.html)
  disqusShortname = "your disqus short name"

  # Google analytics - _internal/googleanalytics.html in injected in hugo-octopress/layouts/partials/header.html
  googleAnalytics = "Your Google Analytics tracking code"

  # switch to true to enable RSS icon link in the navigation menu
  rss = true  

  # Website's default description used in meta tags
  defaultDescription = ""

  # keywords used in meta tags
  # does this even work in action?
  # defaultKeywords = ["keyword1" , "keyword2" , "keyword3" , "keyword4"]

```

## <a name="highlight"></a>Code highlight
Octopress classic theme uses the pygments' `solarized dark` theme. It is not installed by default. It should be installed from https://github.com/john2x/solarized-pygment.git. Then there are three options:

* solarized_light: default
* solarized_dark: the default one
* solarized_dark256

The following options in `config.toml` modify the behavior:

```
[params]
  # if true, code highlight needs to be done using a css - not supported in this theme
  pygmentsuseclasses = false

  # if nothing is set, then solarized_light is used
  # other styles can be viewed in [http://pygments.org/](http://pygments.org/)
  pygmentsstyle = "solarized_dark"

  # will make the highlight shortcode and code fences (```) being treated similarly
  pygmentscodefences = true

  # pygment options can be added here
  # Hugo supports these pygments options: style, encoding, noclasses, hl_lines, linenos - for example:
  # pygmentsoptions = "linenos=true"
```
## <a name="markdown"></a>Markdown options

Blackfriday is Hugo's markdown engine. For a list of options visit [https://gohugo.io/overview/configuration/](https://gohugo.io/overview/configuration/) (scroll down to `Configure Blackfriday rendering`). Blackfriday options can be set in the `config.toml` files as follows:

    [blackfriday]
      hrefTargetBlank = true # open the external links in a new window
      fractions = false

## <a name="menu"></a>Navigation menu
Links to the left of the navigation menu (everything other than the search input field and RSS icon) can be configured here. Navigation menu is generated using the `hugo-octopress/layouts/partials/navigation.html` template.

All links open in a new window except links to root. If the URL is "/" the link will not open in a new window.

Links are sorted according to weight from left to right. For example a link with weight of `-10` will be to the left of another link with weight `0` or `10`. Links can be added to the `config.toml` as follows:

    [[menu.main]]
      Name = "Blog"
      URL = "/"
      weight = -10

    [[menu.main]]
      Name = "The other guy from Wham!"
      URL = "https://www.google.com/search?q=andrew+ridgeley"
      weight = -5

    [[menu.main]]
      Name = "This theme - add link"
      URL = "https://www.github.com"

The searchengine can also be customized in the `config.toml` file as follows:

    [params]
      # search enginer paramete in the navigation menu
      search_engine_url = "https://www.google.com/search"

## <a name="sidebarlinks"></a>Sidebar links
The sidebar is generated using the partial template at `hugo-octopress/layouts/partials/sidebar.html`.

Sidebar links are read from the config file as follows:

    [params]
      github = "https://github.com/parsiya/"
      bitbucket = "https://bitbucket.org/parsiya/"
      twitter = "https://twitter.com/cryptogangsta/"
      keybase = "https://keybase.io/parsiya/"
      stackoverflow = ""
      linkedin = ""
      googleplus = ""
      youtube = ""
      facebook = ""

If more than links are added, then add a `</br>` between the first four and the rest. Code to display links (and the idea to use these icons) was taken from [Hyde-x](https://github.com/zyro/hyde-x/).

Icons are from [http://fontawesome.io](http://fontawesome.io) by Dave Gandy. To use icons with square dark backgrounds add `-square`. For example `<i class="fa fa-twitter-square fa-3x"></i>`. Size can be from 1 to 5 or `fa-lg` to be adaptive.

## <a name="shortcodes"></a>Shortcodes
Creating [shortcodes](https://gohugo.io/extras/shortcodes/) in Hugo was surprisingly easy. I used two plugins in Octopress that I re-created in Hugo using shortcodes. They add captions to code blocks and images. These shortcodes are located at `hugo-octopress/layouts/shortcodes/`

### <a name="codecaption"></a>Code caption
This shortcode adds a caption to codeblocks. The codeblock is wrapped in a `<figure>` tag and caption is added using `<figcaption>`. It has two parameters, `title` which is the caption of the code block and `lang` which is the language that is passed to the Hugo `highlight` function along with `linenos=true` which adds line numbers to the codeblock.

Shortcode usage (and source) is as follows (please note that parameters are named and not positional):

``` html
{{< codecaption lang="html" title="Code caption shortcode" >}}
<figure class="code">
  <figcaption>
    <span>{{ .Get "title" }}</span>
  </figcaption>
  <div class="codewrapper">
    {{ highlight .Inner (.Get "lang") "linenos=true" }}
  </div>
</figure>
{{< /codecaption >}}
```

And will look like this:

![picture](/images/codecaption1.png).

If the code inside the tag overflows, a horizontal sidebar will be added to the table. It took me a while to achieve this as the `highlight` function created tables that were out of my control. The output from `highlight` is wrapped in `<div class="codewrapper">` and the scroll bar will be added for the whole `div`. The following in the `css` (starting from line 2225) enables this behavior:

``` css
div.codewrapper {
    overflow-x: auto;
    overflow-y: hidden;
    background-color: #002B36;
}
```

### <a name="imgcap"></a>Image caption
This shortcode adds captions to pictures. Due to the way the original `css` file was organized, this shortcode does not use `<figure>` and `<figcaption>`. `Alt` will also be set to `title`.

Usage is as follows (please note that parameters are named and not positional):

``` go
{{< imgcap title="Sample caption" src="/images/2016/thetheme/1.png" >}}
```

Will result in:

``` html
  <span class="caption-wrapper">
    <img class="caption" src="/images/2016/thetheme/1.png" title="Sample caption" alt="Sample caption">
    <span class="caption-text">Sample caption</span>
  </span>
```

## <a name="licensepage"></a>License page
The generated license page will be located at `example.com/license/`. Markdown code for the license page can be anywhere in the content page, however the type of the markdown file should be set to `license` in front material. For example:

    ---
    title: "License"
    type: license
    ---

    License text

Lisence page template is located at: `hugo-octopress\layouts\license\single.html`.

## <a name="issues"></a>Issues/TODO

If you see any issues/bugs or you are looking for some features please use the Github issue tracker. Please keep in my mind that my dayjob is not development and I may be slow in fixing things (or I may ask you to help me with it).

**The css is a mess.** Yes I agree with that. The `css` file is `screen.css` taken from the default Octopress theme. I found it easier to just modify the templates to generate HTML code similar to Octopress and use the `css` file. It's bulky (around 53KBs and 2300 lines) and it probably has code for elements that are never used (also duplicates). It also contains the highlight code.

If you know how to clean it up, please let me know or better yet help me do it :)

## <a name="attribution"></a>Attribution
* [Octopress](octopress-link) is created by [Brandon Mathis](https://github.com/imathis). Octopress source can be found on [https://github.com/imathis/octopress](https://github.com/imathis/octopress).

* Some code was taken from the [Hyde-x](https://github.com/zyro/hyde-x) Hugo theme by [Andrei Mihu](http://andreimihu.com/).

* Sidebar icons are from Font Awesome by Dave Gandy - http://fontawesome.io.

## <a name="Ported by"></a>Ported by
Ported by Parsia Hakimian:

* website: [parsiya.net](http://parsiya.net)
* twitter: [@CryptoGangsta](https://twitter.com/cryptogangsta)

## <a name="themelicense"></a>Theme license
Open sourced under the [MIT license](https://github.com/parsiya/Hugo-Octopress/blob/master/LICENSE.md).


[octopress-link]: http://octopress.org
[hugo-link]: https://gohugo.io
