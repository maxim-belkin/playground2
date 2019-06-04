This is a Markdown file. Yes, this is a plain-text file. What makes it special?

Markdown is a writing format. It is not a replacement for HTML, which is a publishing format...
though, you <strong>can use HTML tags</strong> here!

Let's see what we can do with Markdown.

# Markdown basics

- Pound signs `#`, _a.k.a._ hashtags, are used for headings. The number of hashtags determines the level of this heading.
_(Don't worry if the previous sentence does not make sense to you -- this is 100% **normal**!)_
- Backticks are used for Monospaced text (usually used for `commands`)
- If you need to document a sequence of commands you used to generate a plot or start your analysis
  you can enclose it between a pair of triple backticks each appearing on a new line:
  
  ```
  echo "Hello, World!"
  python3 analyze_my_data.py
  ```
- Lists are super-easy: you can use `-`, `*`, or `*` for unordered lists...
- Or `1. `, `2. `, `3. `, ... for ordered ones. You can also use all `1.`s -- they will be automatically incremented as necessary.
- Links are super easy too: `[link](web-address)`, `[link2](web-address "Title")`, or
  ```
  Paragraph with a [link][url].

  [url]: web-address
  ```

To learn more about Markdown, see [this page](https://daringfireball.net/projects/markdown/syntax).

That's it!
