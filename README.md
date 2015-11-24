# Buildium CSS standards

*Our approach to writing simple, scalable CSS*

## Table of Contents

1. [Mission and goals](#mission-and-goals)
2. [Our tools](#our-tools)
3. [Organizing stylesheets](#organizing-stylesheets)
4. [Common variables](#common-variables)
5. [Naming](#naming)
6. [Positioning and layout](#positioning-and-layout)
7. [Consistent order of properties](#consistent-order-of-properties)
8. [Specificity](#specificity)
9. [Shorthand vs. longhand](#shorthand-vs-longhand)
10. [Formatting](#formatting)
11. [Styleguide](#styleguide)
12. [Resources and further reading](#resources-and-further-reading)

## Mission and goals

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

```css
buildium-base/components/search

-search.html           //template
-search.js             //directive
-search.less           //styles
-search.test.js        //unit test

```

As you can see, this component contains it's own stylesheet.  This is imported into a file in the root components directory called main.less.

components/main.less is then imported into a styles directory within buildium-base.  This style directory contains common elements like mixins, variables, and other tools that we'd like access to when writing component specific styles.  This approach keeps the component specific stylesheets lean, and also reserves a special place to write styles that can be used globally.

##Common Variables

Use common mixins for visual behaviors that are likely repeated throughout the site such as colors, box-shadow, border-radius, etc...

All colors should be defined as common variables, and we store them in buildium-base/styles/variables/colors.less

##Naming

We use th Block Element Modifier (BEM) naming convention.  BEM-style names are, in general, very descriptive. They also provide a way of indicating hierarchy without the side-effects of nesting classes and causing specificity battles. Finally, BEM is very conducive to creating a component-based UI (which we strive for in our Angular-based code).

block__element-modifier

Always try to make class names meaningful -- "large" is better than 600, "primary" is better than blue.

```css
//less

.search__entity-list {
   .list-reset;
}

.search__entity-list-item {
   padding: 10px 5px 10px 38px;
   background-image: url(/Manager/static-images/search-sprite.png);
   background-repeat: no-repeat;
   font-size: 12px;
}

.search__entity-list-item--active {
   background-color: @focusHighlight;
}

//view

<ul class="search__entity-list">
  <li class="search__entity-list-item  
    search__entity-list-item--{{item.category}}"
    ng-repeat="item in entity.item"
    ng-class="{ 'search__entity-list-item--active': item === search.activeItem }"
    ng-mouseover="search.activeItem = item">
</ul>
```

It’s better to separate out what is essential to all form columns (positioning and gutters) and what is a specific modifier (width).

```css
//Avoid - does too many things

.formColumnSmall {
    float: left;
    margin: 0 @grid-gutter-width / 2;
    width: 90px;
}

//Instead, seperate it out...

.form__column {
   float: left;
   margin: 0 @grid-gutter-width / 2;
}

.form__column--small {
   width: 90px;
}

.form__column--default {
   width: 202.5px;
}
```

## Positioning/Layout

Seperate positioning and layout from reuseable components.  We use a grid system for positioning, making things predictable and preventing a specific component's styles from affecting others.

Lay out your pages using rows and columns.  For forms (the most common type of page in our application), we use a .form__column class with appropriate modifiers for width:

```html
<div class="row">
    <div class="form__column form__column--default" bd-form-element>
        <label class="form__label" for="required">Required</label>
        <input class="form-element form-element--default" name="required" id="required" required ng-model="vm.formModel.required" />
    </div>
</div>

<div class="form__vertical-spacer form__vertical-spacer--medium"></div>
```

You'll notice we handle vertical spacing using specific spacing elements.

### Individual controls

Individual blocks should not have positioning information or external margin/padding because this makes components difficult to adapt to varying circumstances. 
For example, if an input element includes a bottom margin, we are forced to create more and more overrides in cases where we want a different size gutter between elements. 

Instead, we strive to write components that take up the full size of their container (100% width) and do not have external margins or padding.

## Consistent order of properties

By keeping a consistent order of properties within a class, it’s easier to quickly read/write an existing class. Basically, it takes away the need to search through the entire class to see if a given property is set.

When writing a class, group different CSS properties together and place them in the following order (credit to Mark Otto @mdo):
<ul>
	<li>Positioning</li>
	<li>Display and box mode</li>
	<li>Typographic</li>
	<li>Visual</li>
</ul>

```css
.declaration-order {
    /* Positioning */
    position: absolute;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    z-index: 100;

    /* Display and box model */
    display: block;
    float: right;
    width: 100px;
    height: 100px;
    padding: 5px;

    /* Typography */
    font: normal 13px "Helvetica Neue", sans-serif;
    line-height: 1.5;
    color: @offBlack;
    text-align: center;

    /* Visual */
    background-color: @lightBlue;
    border: 1px solid @green;
    border-radius: 3px;
}

```

##Specificity
<ul>
	<li>Use classes.  No ID's!
	<li>Elements: Only use them for resetting or noramlizing styles between browsers.
	<li>If using JS to access a class, prefix it with js-, i.e. js-my-selector and use it only for JS
	<li>Try to only override a property at most 1 time: </li>
</ul>

```css
.my-class {
	font-size: 12px;
	text-align: center;
}

/** this class only overrides a single property. "text-align" remains the same **/
.my-class--left-text {
	text-align: left;
}

```

## Shorthand vs. longhand
<ul>
	<li>Define colors in their own less files using upper-case, long-hand Hex format. For example #FFFFFF;</li>
	<li>Define all sizes in properties that take 4 arguments:  border: 0 0 0 1px vs border-left: 1px;</li>
	<li>Overriding classes use more specific properties, if possible:  border-left: 1px vs border: 0 0 0 1px;</li>
</ul>

## Formatting

<ul>
	<li>Curly braces should start on the same line as the class name, with one space between.</li>
	<li>Each property should have it's own line, with semi-colons, and one space between.</li>
	<li>Avoid using 0px, 0em, etc.;  instead use 0.</li>
	<li>Each class should have a line between.</li>
	<li>Avoid nesting classes, if possible.</li>
	<li>Don’t use vendor-specific prefixes. We run the styles through <a href="https://github.com/postcss/autoprefixer">autoprefixer</a> which will automatically insert prefixes when needed.</li>
</ul>

##Common helpers

By separating some common behaviors - like those listed below - we can better keep track of variations within the app. This should keep the visual appearance and behavior of different parts of the site (text shadows, for example) more coherent.

These should be in common helpers:
<ul>
	<li>positioning</li>
	<li>transitions</li>
	<li>rounding edges</li>
	<li>shadows</li>
	<li>common fonts, i.e. for accounting</li>
</ul>

These are common helpers, but an element might have them.  As a general rule, you can use a few, but if we are adding more than 3 helpers, make a class.

## Styleguide

Documentation is great, but writing and reading it should be convenient and located near the code itself.

We use kss-node to generate docs from comments in styles.
<ul>
	<li>Spec is https://github.com/kss-node/kss/blob/spec/SPEC.md</li>
	<li>Comments should have a title and a description.</li>
	<li>Separate markup will be generated for each modifier class (.selected, in the example below the description text), using the Markup field.</li>
	<li>We’ve added a DocUrl field to the spec, that links to our living styleguide.</li>
	<li>The Styleguide variable at the end will put your comments in the right place.</li>
</ul>

Example:

```
/*
Tabs

Tabs are used to segment content for a single entity into logical groups.

.selected - Indicates tab is selected

Markup:
<nav id="tabbedNav">
   <ul>
       <li><a href>Summary</a></li>
       <li class="{{modifier_class}}"><a href>Application</a></li>
       <li><a href>Screening</a></li>
       <li><a href>Fee</a></li>
   </ul>
</nav>

DocUrl:
http://styleguide.buildium.com/dm01-1400-tabs/

Styleguide 3.1
*/
```

## Resources and further reading

<a href="http://codeguide.co/#css-declaration-order">CSS and HTML styleguide</a> by Mark Otto.

<a href="http://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/">MindBEMding – getting your head ’round BEM syntax</a>

<a href="https://css-tricks.com/bem-101/">BEM 101</a>





