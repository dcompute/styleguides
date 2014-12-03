# HAML Style Guide

## <a name='TOC'>Table of Contents</a>

1. [Basic Conventions](#basic-conventions)
  1. [Whitespace](#whitespace)
  1. [Escapement](#escapement)
  1. [Semantics](#semantics)
1. [Naming Conventions](#naming-conventions)
1. [Rails Helpers](#rails-helpers)
1. [Interpolation](#interpolation)
1. [Forms & AJAX](#forms-ajax)
1. [Content For Blocks](#content-for-blocks)
1. [Scripting Hooks](#scripting-hooks)
1. [Progressive Enhancement](#progressive-enhancement)
1. [Accessibility](#accessibility)


## <a name='basic-conventions'>Basic Conventions</a>

  - Use soft tabs (set to 2 spaces).

  - Use double quotes (`class: "hello"`) over single.

  - Soft limit to 80 character line lengths. A little over is _okay_, but avoid anything longer than 100 characters.

  - Line break for readability:

    ```haml
    -# bad
    = link_to "foo", bar, title: "pushing to 80 chars", data: {foo: "bar", biz: "baz",
      fiz: "buzz"}

    -# good
    = link_to "foo", bar, title: "pushing to 80 chars",
      data: {foo: "bar", biz: "baz", fiz: "buzz"}
    ```

  - Indent code breaks with two spaces:

    ```haml
    -# bad
    = link_to bar, data:
      {foo: "bar",
        biz: "baz",
        fiz: "buzz"} do
      Terms and Condtions

    -# okay
    = button_tag "Submit", type: "submit", data:
      {foo: "bar",
      bam: "boom"}

    -# good
    = link_to "FAQ", url,
      class: "blue"

    -# good
    = link_to url,
      class: "blue" do
      Contact Us
    ```

  - Inline styles are **never** appropriate.

  - Inline JavaScript is **never** appropriate.

  - Use `key: value` syntax for attributes when possible. Only use
    hashrocket syntax when you must quote a key. Avoid invalid key
    symbols wherever possible:

    ```haml
    -# bad
    %li{data: {:foo => "bar"}}

    -# bad
    %li{:$monies => "Why did I do this?!"}

    -# okay
    %li{ng: {:"ruined-markup" => "1234"}}

    -# good
    %li{data: {foo: "bar"}}

    -# good
    %li{data: {foo_bar: "biz baz"}}
    ```


### <a name='whitespace'>Whitespace</a>

  - Put one whitespace character after delimiters (eg- `:` or `,`):

    ```haml
    -# bad
    %li{data:{foo:"bar",biz:"baz"}}

    -# good
    %span{class: "icon-#{item.category.name}"}

    -# good
    %li{class: "foo", data: {foo: "icon-#{item.category.name}"}}
    ```

  - Place content on a new line after its parent element:

    ```haml
    -# bad
    %h2 Welcome to the Site!

    -# bad
    %h2= modal.header.title

    -# good
    %h2
      Welcome to the Site!

    -# good
    %h2
      = modal.header.title
    ```

  - Use `<` and `>` when needed:

    ```haml
    -# good
    %ul<
      %li>
        One
      %li>
        Two
    ```


### <a name'escapement'>Escapement</a>

  - Escape self-closing tags:

    ```haml
    -# bad
    %br

    -# good
    %br/

    -# good
    %meta{charset: "utf-8"}/
    ```

  - Use entity escapements when appropriate:

    ```haml
    -# bad
    Hello & Goodbye!

    -# good
    Hello &amp; Goodbye!

    -# good
    = link_to "Truncated", "#", title: "Long string that trunca&hellip;".html_safe
    ```


### <a name='semantics'>Semantics</a>

  - Use semantic elements when possible:

    ```haml
    -# bad
    .list
      .list-item
        / Content...

    -# good
    %ul.list
      %li.list-item
        / Content...
    ```

  - Apply `%span`s to inline elements:

    ```haml
    -# bad
    %p
      .icon-category
      Foo bar

    -# good
    %p
      %span.icon-category
      Foo bar
    ```


### Classes and IDs

  - Apply IDs before classes.

    ```haml
    -# bad
    .i-reuse-this-all-the-time#but-never-more-than-once

    -# good
    #im-one-of-a-kind.but-a-lot-like-others

    -# good
    %article#article-2009-12-17.post
    ```

  - Unless you need to interpolate, apply IDs and classes directly on elements
    rather than as an attribute:

    ```haml
    -# bad
    %span{class: "foo-bar"}

    -# bad
    %span{class: "foo-bar icon-category-#{category.name}"}

    -# good
    %span.foo-bar

    -# good
    %span.foo-bar{class: "icon-category-#{category.name}"}
    ```


## <a name='naming-conventions'>Naming Conventions</a>

  - IDs and Classes should use _lower_ `dash-case-for-names`.

  - Templates must have a Class or ID at the root level which
    matches the partial's file name. Examples:

    - File named: `_stream_card.html.haml`

      ```haml
      -# bad
      %li
        .stream-card
          %h3
            Stream Card

      -# good
      .stream-card
        %h3
          Stream Card

      -# good
      %li.stream-card
        %h3
          Stream Card

      -# good
      %li.stream-card.media-card
        %h3
          Stream Card
      ```

    - File named: `_right_rail.html.haml`

      ```haml
      -# bad
      %h2
        I'm the right rail!

      -# good
      #right-rail
        %h2
          I'm the right rail!
      ```

  - Provide content relational classnames when possible. These become
    particularly resourceful for selectors in tests.

    ```haml
    -# bad
    %dl
      %dt
        = user.login
      %dt
        = quick_meta(user)
      %dd
        = user.location

    -# good
    %dl.media-meta
      %dt.user-login
        = user.login
      %dt.user-meta
        = quick_meta(user)
      %dd.user-location
        = user.location
    ```


## <a name='rails-helpers'>Rails Helpers</a>

  - Use Rails' ActionView element helpers whenever possible:

    ```haml
    -# bad
    %a{href: some_url_path}
      Hello!

    -# bad
    %form{action: "foo", method: "post"}

    -# good
    = link_to user.name, profile_path(user.name)

    -# good
    = form_tag form_endpoint_path, class: "new_form" do

    -# good
    = button_tag "Submit", type: "submit"
    ```

  - Use blocks if you need to wrap an element in another element:

    ```haml
    -# good
    = link_to path do
      %span.icon-edit
        Edit this
    ```

  - Avoid usage of `content_tag` and `tag` in views, they rarely end up
    being useful.

    ```haml
    -# bad
    = content_tag :p, "Not sure if serious"

    -# bad
    = tag "br"

    -# good
    %p
      "Not sure if serious"
    ```


## <a name='interpolation'>Interpolation</a>

  - Always use double quotes, even if you don't need to interpolate. Helps
    keep later code changes cleaner.

  - Use curly braces when you need multiple tags within a single line:

    ```haml
    -# bad
    Contact us via
    = link_to "Email", email.address
    if you need further support.

    -# good
    Contact us via #{link_to "Email", email.address} if you need further support.
    ```

  - Do not mix interpolated attributes with non-interpolated ones:

    ```haml
    -# bad
    %span{class: "foo-bar icon-category-#{category.name}"}

    -# good
    %span.foo-bar{class: "icon-category-#{category.name}"}
    ```


## <a name='forms-ajax'>Forms & AJAX</a>

  - Always use Rails' [Form Tag](http://api.rubyonrails.org/classes/ActionView/Helpers/FormTagHelper.html#method-i-form_tag)
    or [Form For](http://api.rubyonrails.org/classes/ActionView/Helpers/FormHelper.html#method-i-form_for)
    helpers. Set actions to API endpoints, models, or records whenever possible.
    Use actual routes helpers instead of plain strings.

    ```haml
    -# bad
    = form_tag "/api-endpoint", remote: true do

    -# bad
    %form{anything: "you already failed"}

    -# good
    = form_for current_user do

    -# good
    = form_tag api_endpoint_path, remote: true do
    ```

  - Always apply labels to form inputs, radios, and checkboxes.

  - Always use Rails' built in `remote: true` option for links, forms, or buttons
    that trigger AJAX requests.

    ```haml
    -# bad
    = button_tag "Open Modal", data: {resource: api_endpoint_path}

    -# good
    = link_to "Favorite", api_favorites_path, remote: true do

    -# good
    = button_tag "Open Modal", data: {resource: api_endpoint_path}, remote: true
    ```


## <a name='content-for-blocks'>Content For Blocks</a>

  - When using `content_for` blocks within a template, always place them at the
    top of the file or parent block.

  - Create handy `= yield :bottom` and `= yield :head` blocks in your layouts
    for including additional styles or scripts in a page. Include
    `stylesheet_link_tag` or `:css` within `- content_for :head`. Include
    `javascript_include_tags`, `:javascript`, or `:coffeescript` within `- content_for :footer`.


## <a name='scripting-hooks'>Scripting Hooks</a>

  - Never apply JavaScript to a style-oriented selector. Add a `[data]`
    attribute or classname that is free of styles for binding functionality.

  - Use `[data-trigger]` attributes as hooks whenever possible.

  - If you use a classname, prepend your classname with `.trigger-`.

## <a name='progressive-enhancement'>Progressive Enhancement</a>

  - TBD

## <a name='accessibility'>Accessibility</a>

  - TBD
