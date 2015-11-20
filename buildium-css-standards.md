# Buildium CSS standards

*Our approach to writing simple, scalable CSS*

## Table of Contents

1. [Mission and goals](#mission)
2. [Our Tools](#our-tools)
3. [Organizing stylesheets](#organizing)

## Mission and goals

As developers as Buildium, we want to create simple ways to find styles that exist or create new styles, so we can modify pages with the confidence that we are not duplicating code or interfering with the styles on other pages.

### simplicity

Something simple has a clear purpose and direction, while something easy is that which is near at hand and familiar.  Using ID's as CSS selectors is easy, but not necessarily simple, because in the long run it will create complicated specificity issues.

### find styles that exist or create new styles

The names of classes should reflect the component they are providing styles for so that a developer can look some markup and have a reasonable guess where to look for a style (or what to search for). In addition, the file structure of our components and stylesheets should make finding relevant styles quick. 

### create or modify pages

By following the guidelines here, developers should be able to create new components and styles without worrying about conflicting with existing styles.

### with confidence

One of the biggest frustrations with tangled CSS is that it’s nearly impossible to know when a local change has an unwanted side-effect. Developers can prevent these headaches by sticking to stricter conventions.

## Our Tools

Our front-end code base at Buildium consists of a series of individual Angular.js applications, each serving a different part of our product.  Each of these apps consists of even smaller components (directives, views, and controllers).  And, we store common components that are used across multiple parts of the product in a directory called 'buildium-base'.

To help organize and optimize our stylesheets, we use LESS.


## Organizing stylesheets


A typical Angular component directory (a search bar directive, in this case) will look like this:

```
buildium-base/components/search

-search.html           //template
-search.js             //directive
-search.less           //styles
-search.test.js        //unit test

```

As you can see, this component contains it's own stylesheet.  This is imported into a file in the root components directory called main.less.

components/main.less is then imported into a styles directory within buildium-base.  This style directory contains common elements like mixins, variables, and other tools that we'd like access to when writing component specific styles.  This approach keeps the component specific stylesheets lean, and also reserves a special place to write styles that can be used globally.





