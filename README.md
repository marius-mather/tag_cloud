tag_cloud: A Plugin for Pelican
====================================================

[![Build Status](https://img.shields.io/github/workflow/status/pelican-plugins/tag_cloud/build)](https://github.com/pelican-plugins/tag_cloud/actions)
[![PyPI Version](https://img.shields.io/pypi/v/pelican-tag_cloud)](https://pypi.org/project/pelican-tag_cloud/)
![License](https://img.shields.io/pypi/l/pelican-tag_cloud?color=blue)

Generates a tag cloud for the tags in your posts

Installation
------------

This plugin can be installed via:

    python -m pip install pelican-tag_cloud

Usage
-----

### Setup

In order to use to use this plugin, you have to edit(*) or create(+) the following files:

```
blog/
├── pelicanconf.py *
├── content
├── plugins +
│     └── tag_cloud.py +
└── themes
        └── mytheme
            ├── templates
            │      └── base.html *
            └── static
                    └── css
                        └── style.css *
```

In `pelicanconf.py` you have to activate the plugin::

```python
PLUGINS = ["tag_cloud"]
```


In your theme files, you should change `base.html` to apply formats 
(and sizes) defined in `style.css`, as specified in "Settings", below.

### Settings

================================================    =====================================================
Setting name (followed by default value)            What does it do?
================================================    =====================================================
`TAG_CLOUD_STEPS = 4`                               Count of different font sizes in the tag
                                                      cloud.
`TAG_CLOUD_MAX_ITEMS = 100`                         Maximum number of tags in the cloud.
`TAG_CLOUD_SORTING = 'random'`                      The tag cloud ordering scheme.  Valid values:
                                                      random, alphabetically, alphabetically-rev, size and
                                                      size-rev
`TAG_CLOUD_BADGE = True`                            Optional setting: turn on **badges**, displaying 
                                                      the number of articles using each tag.
================================================    =====================================================

The default theme does not include a tag cloud, but it is pretty easy to add one::

```html
<ul class="tagcloud">
    {% for tag in tag_cloud %}
        <li class="tag-{{ tag.1 }}">
            <a href="{{ SITEURL }}/{{ tag.0.url }}">
            {{ tag.0 }}
                {% if TAG_CLOUD_BADGE %}
                    <span class="badge">{{ tag.2 }}</span>
                {% endif %}
            </a>
        </li>
    {% endfor %}
</ul>
```

You should then also define CSS styles with appropriate classes (tag-1 to tag-N,
where N matches `TAG_CLOUD_STEPS`), tag-1 being the most frequent, and
define a `ul.tagcloud` class with appropriate list-style to create the cloud.
You should copy/paste this **badge** CSS rule `ul.tagcloud .list-group-item <span>.badge`
if you're using `TAG_CLOUD_BADGE` setting. (this rule, potentially long, is suggested to avoid
conflicts with CSS libraries like Bootstrap)

For example:

```css
ul.tagcloud {
    list-style: none;
    padding: 0;
}

ul.tagcloud li {
    display: inline-block;
}

li.tag-1 {
    font-size: 150%;
}

li.tag-2 {
    font-size: 120%;
}

/* ... add li.tag-3 etc, as much as needed */

ul.tagcloud .list-group-item span.badge {
    background-color: grey;
    color: white;
}
```

By default the tags in the cloud are sorted randomly, but if you prefer to sort alphabetically use the `alphabetically` (ascending) and `alphabetically-rev` (descending). Also is possible to sort the tags by size (number of articles with this specific tag) using the values `size` (ascending) and `size-rev` (descending).

Contributing
------------

Contributions are welcome and much appreciated. Every little bit helps. You can contribute by improving the documentation, adding missing features, and fixing bugs. You can also help out by reviewing and commenting on [existing issues][].

To start contributing to this plugin, review the [Contributing to Pelican][] documentation, beginning with the **Contributing Code** section.

[existing issues]: https://github.com/pelican-plugins/tag_cloud/issues
[Contributing to Pelican]: https://docs.getpelican.com/en/latest/contribute.html

License
-------

This project is licensed under the AGPL-3.0 license.
