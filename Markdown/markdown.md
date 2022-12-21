# Markdown
> 간단한 마크다운 사용법

---

## 1. Headers

# This is an H1
```markdown
# This is an H1
```

## This is an H2
```markdown
## This is an H2
```

### This is an H3
```markdown
### This is an H3
```

#### This is an H4
```markdown
#### This is an H4
```

##### This is an H5
```markdown
##### This is an H5
```

###### This is an H6
```markdown
###### This is an H6
```

This is an H1
==
```markdown
This is an H1
=============
```

This is an H2
--
```markdown
This is an H2
-------------
```

## 2. Blockquotes

> This is a blockquote
```markdown
> This is a blockquote
```

> This is the first level of quoting.
>
> > This is nested blockquote.
>
> Back to the first level.
```markdown
> This is the first level of quoting.
>
> > This is nested blockquote.
>
> Back to the first level.
```

## 3. Lists

* Red
* Green
* Blue
```markdown
* Red
* Green
* Blue
```

+ Red
+ Green
+ Blue
```markdown
+ Red
+ Green
+ Blue
```

- Red
- Green
- Blue
```markdown
- Red
- Green
- Blue
```

1. Red
2. Green
3. Blue
```markdown
1. Red
2. Green
3. Blue
```

## 4. Code Blocks

This is a normal paragraph:

    This is a code block.
```markdown
This is a normal paragraph:

    This is a code block.
```

## 5. Horizontal Rules

* * *
```markdown
* * *
```

***
```markdown
***
```

*****
```markdown
*****
```

- - -
```markdown
- - -
```

---------------------------------------
```markdown
---------------------------------------
```

## 6. Links

This is [an example](http://example.com/ "Title") inline link.

[This link](http://example.net/) has no title attribute.
```markdown
This is [an example](http://example.com/ "Title") inline link.

[This link](http://example.net/) has no title attribute.
```

This is [an example][id] reference-style link.

[id]: http://example.com/  "Optional Title Here"
```markdown
This is [an example][id] reference-style link.

[id]: http://example.com/  "Optional Title Here"
```

## 7. Emphasis

*single asterisks*
```markdown
*single asterisks*
```
_single underscores_
```markdown
_single underscores_
```

**double asterisks**
```markdown
**double asterisks**
```

__double underscores__
```markdown
__double underscores__
```

## 8. Code

Use the `printf()` function.
```markdown
Use the `printf()` function.
```

``There is a literal backtick (`) here.``
```markdown
``There is a literal backtick (`) here.``
```

## 9. Images
![Alt text](./img/md.png)
```markdown
![Alt text](./img/md.png)
```

![Alt text](./img/md.png "Optional title")
```markdown
![Alt text](./img/md.png "Optional title")
```

## 10. Backslash Escapes

\*literal asterisks\*
```markdown
\*literal asterisks\*
```
```markdown
\   backslash
`   backtick
*   asterisk
_   underscore
{}  curly braces
[]  square brackets
()  parentheses
#   hash mark
+   plus sign
-   minus sign (hyphen)
.   dot
!   exclamation mark
```

---

# 참조 사이트
[공식 사이트](https://daringfireball.net/projects/markdown/)
