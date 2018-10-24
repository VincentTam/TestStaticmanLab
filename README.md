# Test Staticman Lab
_Minimal_ GitHub static site powered by [Jekyll](https://jekyllrb.com),
served by @staticmanlab running on [Staticman](https://staticman.net).

+ demonstrate how [Staticman v3](https://github.com/eduardoboucas/staticman/pull/219) works with Jekyll.
+ facilitate other developers to change my repo.  (e.g. add CSS)

## Minimal setup
Clone this repo.

    git clone https://github.com/VincentTam/TestStaticmanLab.git <your-site-name>
    
Remove existing comments under the folder `_data` and the public domain license in file `LICENSE`.
Then modify the following fields in the Jekyll config file `_config.yml`.

```yml
# Site settings
title: Your title
subtitle: Optional subtitle
url: "https://<username>.github.io/<repo>/"

staticman:
  api: "https://staticman3.herokuapp.com/v3/entry/github/<username>/<repo>/<branchname>/<propertyname>"
  path: "_data/comments/test-slug"
```
I hardcoded `test-slug` in `_config.yml`, `index.html` and `_includes/comments.html`.  The property name defaults to `comments`.

By default, comments are sent to the GitHub repo as pull requests because of the `moderation` parameter in `staticman.yml`.

```yml
  moderation: true
```

Changing it to `false` will enable automatic merge.

Finally, make your Git repo available on GitHub and invite **@staticmanlab** as a collaborator.
Enter the URL

    https://staticman3.herokuapp.com/v3/connect/github/<username>/<repo>
    
so that **@staticmanlab** can accept your invitation and start delivering comments to your site.

## Minimal site infrastructure
The source code for this Jekyll site is made up of six pieces.
The infrastructure follows [Popcorn](http://popcorn.staticman.net/), Staticman's official demo.

1. Homepage: `index.html`
  - contain the HTML form (copied from [Staticman's guide](https://staticman.net/docs/))
  which sends a POST request to the `api` specified in `_config.yml`.
  - outsource HTML code for comment display to `_includes/comments.html`
  so as to allow reader's to focus on the HTML form, which is the _main_ focus of this project
  - outsource wrapping HTML code to `_layouts/default.html`
2. Generated comments: `_data/<propertyname>/<slug>/entry-<timestamp>.yml`
  - store static site comments as Jekyll site data
  - file/path creation is handled by Staticman's API
3. Comment renderer: `_includes/comments.html`
  - Retrieve Jekyll site data
  - Wrap each field with suitable HTML tag and class(es)
4. Staticman config file: `staticman.yml`
  - root-level file, keep the file name
  - contain configurations for comment _delivery_ to the GitHub repo
  - only responsible for logical side
    + e.g. generate the MD5 hash of commentator's email
    + _not_ responsible for comment display/site layout
5. Jekyll config file: `_config.yml`
  - Present in _every_ static site generator under a similar name and format
  - Store site config parameters so as to facilitate site setup by avoiding hardcoding.  Some important parameters include
    + `title`: site name
    + `staticman.api`: hook up the HTML form with Staticman's API server's "/entry" endpoint
6. Page layout: `_layouts/default.html`
  - contain necessary HTML code that wraps the form
  - display the site title and the optional subtitle supplied in `_config.yml`
  - add license
  - link to [W3C's HTML validator](https://validator.w3.org/) for _HTML validation in two clicks_.
