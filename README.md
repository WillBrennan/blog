# Blog 
This is a customised version of the [Type Theme repo](https://github.com/rohanchandra/type-theme) and hosted in github pages. 

## Usage 

```bash
# install the dependencies 
bundle install 
# start serving the page locally
bundle exec jekyll serve --watch --livereload
```

### Meta

Meta variables hold basic information about your Jekyll site which will be used throughout the site and as meta properties for search engines, browsers, and the site's RSS feed.

Change these variables in `_config.yml`:

| Variable    | Example                          | Description                                                                                                                    | Optional |
| ----------- | -------------------------------- | ------------------------------------------------------------------------------------------------------------------------------ | -------- |
| title       | My Jekyll Blog                   | Name of website                                                                                                                | Yes      |
| avatar      | assets/img/avatar.png            | Path of avatar image, to be displayed in the theme's header                                                                    | Yes      |
| gravatar    | f9879d71855b5ff21e4963273a886bfc | [MD5 hash of your email address](https://secure.gravatar.com/site/implement/hash/) to load your Gravatar in the theme's header | Yes      |
| description | My blog posts                    | Short description, primarily used by search engines                                                                            | Yes      |

### Scripts

Change these variables in `_config.yml`:


| Variable         | Example      | Description                                                                                                                         | Optional |
| ---------------- | ------------ | ----------------------------------------------------------------------------------------------------------------------------------- | -------- |
| google_analytics | UA-123456-01 | Google Analytics [tracking ID](https://support.google.com/analytics/answer/1032385?hl=en)                                           | Yes      |
| disqus_shortname | shortname    | Disqus [shortname](https://help.disqus.com/customer/portal/articles/466208-what-s-a-shortname-)                                     | Yes      |
| katex            | true         | Takes boolean value (true/false) to conditionally load [KaTeX](https://khan.github.io/KaTeX/) scripts required for math typesetting | Yes      |

Scripts listed here are only loaded if you provide a value in the `_config.yml` file.


### Math typesetting
Wrap math expressions with `$$` signs in your posts and make sure you have set the `katex` variable in `_config.yml` to `true` for math typesetting.

For inline math typesetting, type your math expression on the *same line* as your content. For example:

```latex
Type math within a sentence $$2x^2 + x + c$$ to display inline
```

For display math typesetting, type your math expression on a *new line*. For example:

```latex
$$
  \bar{y} = {1 \over n} \sum_{i = 1}^{n}y_i
$$
```

Type Theme makes use for [KaTeX](https://khan.github.io/KaTeX/) for typesetting.
