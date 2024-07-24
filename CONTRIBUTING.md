# Contributing

All contributions are welcome to the project, the curators try to review all PRs as quickly as possible. 
Yet, this is a volunteer run project, so please be patient with it. If you are planning on submitting bigger 
changes to the projects, please open a github issue first and talk to the team before submitting to make 
sure your work will be accepted.

The curators might reject any contribution without stating a reason, but especially if the contribution or 
contributor in question is not following the [code of conduct](./CODE_OF_CONDUCT.md) or doesn't follow the 
right procedure outlined below.

## Adding or Removing a package

If a package isn't listed on the website yet, feel free to send us a pull-request if it satisfies the following requirements:

- The package is related to the web-stack, open source and publicly available via [crates.io](https://crates.io)
- The package's repository is not archived
- The package is not flagged as unmaintained in [the Rust security advisory database](https://rustsec.org/)
- The package has at least 4k recent downloads on [crates.io](https://crates.io)

Packages are separated into separate `topics`. Topics are located in the `context/topics` directory. Every topic contains 
a TOML frontmatter, from which the HTML is generated:

```
+++
title = "Web Frameworks"

[extra]

packages = [
  "actix-web",
  "rocket"
]
+++
```

To add or remove crates from a topic, simple add or remove the crate from it's package array (`extra.packages`). 
We won't list more than 10 entries for each topic.

If you are in a hurry and don't have the time to prepare the PR, you can also submit an issue requesting for the 
package to be added. *Make sure you mark the issue with the `please-add`-tag*.

And add a few words to the PR (or issue) about the package and why it should be listed (e.g. novel approach, 
popularity, first of its kind...). The more info we have (and the less work it is), the more likely it will be 
added soon.

## Submitting Project News

You have an important announcement or latest news related to web development in rust and you'd like to share 
them with the wider audience of this website? GREAT, here is how you can do that:

Just create a new file in `content/news` - we recommend to use Markdown. Make sure you set the `title`, the
`date`, the `source` and add all appropriate `tags`. Once you are done, please submit all of that as a pull-request 
and we'll get right on it!

## Complaining about incorrect text

Please open a github issue if you feel that anything is represented or out of date. Keep in mind that the description 
of a package is directly pulled from crates.io and won't be changed here.

## Local Development

This project is hosted on github pages and uses the [Zola](https://www.getzola.org/). Zola is a static website 
compiler – it uses our data to generate a bunch of HTML pages. If you do not have zola yet, you can install it 
by using brew (`brew install zola`), snap (`snap install --edge zola`) or any other way 
[outlined on the official website](https://www.getzola.org/documentation/getting-started/installation/).

To compile the documents, run from the main directory

```bash
zola serve
```

Now open a browser on `http://localhost:1111` and you'll be able to see the homepage in its latest version. 
Any changes you make immediately rerender the websites after saving the file, you just need to refresh the 
browser.

To generate a production build, run:

```bash
zola build
```

## Topic Levels

Aside from it's title and intro, a topic also has the "maturity" level – an indicator from 0-6, which indicates how 
mature the curators believe this topic to be in rust (0 being the most mature and tested):

```
+++
title = "Templating"

[extra]

level = 0
+++
```

## SubTopics

Some topics do not have a `packages` array. These topics contain content after the frontmatter (after the second `+++`).
Anything after the second `+++` is then rendered after the title in the top of the page, before the packages are listed.
For example, the database topic is separated into `drivers`, and `orms`:
```
+++
[extra]

drivers = [
  "mysql",
  "postgres"
]

orms = [
  "diesel",
  "rustorm"
]
+++
```

These sub-topics are then rendered at the bottom of the page:
```html
<h2 id="drivers">Drivers</h2>

{{ packages(packages='drivers') }}

<h2 id="orms">ORMs</h2>

{{ packages(packages='orms') }}
```

For more information about templating, see the `templating` below.

## Templating

### The `Level` Macro

You can easily render the level indicator (colored and with the right icon) by including the `level.html` macro. It takes 
one parameter `level`, which is an integer from 0-5, anything else will be understood as "worse than 6".

Example:

```html
{% import "macros/level.html" as level %}

{{ level::level(level=2) }}
```

Note that macros can only be used in templates (`.html`), not in markdown files. To get around this, we use 
[zola shortcodes](https://www.getzola.org/documentation/content/shortcodes/) that link to the corresponding macro. 
In markdown you do not need to import the file:

```markdown
{{ level(level=2) }}
```

### The `Package` Macro

You can render the package `li` element at any point by just rendering the `package` macro. It takes one parameter `name`, 
which is the name of the package in question.

Example:

```html
{{ package(pkg_name=pkg_name) }}
```

### The `Packages` Macro

To render a list of packages as a `ul` block, just include the `packages` macro or shortcode. It takes one parameter 
`packages`, which is the list of names of packages.

HTML Template Example:

```html
{% import "macros/packages.html" as packages %}

{{ packages::packages(packages=page.extra.packages) }}
```

In markdown, you can use the `packages` shortcode and pass in the name of the toml frontmatter array containing the packages:

```
+++
[extra]

queries = [
  "datafusion"
]
+++

{{ packages(packages='queries') }}
```

## Becoming a curator

You want to help curate the listing? Awesome. Just open a issue on github, giving some info about you and why you want to join.
