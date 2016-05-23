# SCSS Conventions [oldmen.digital]

Our team specialize in complex web and hybrid mobile applications. 99% of our projects are built with [AngularJS](https://angularjs.org). For last two years we have been working on conventions to simplify our development process. In this repository we try to gather different utilities which simplify bootstrapping, developing and maintaining scss part of all our projects. In addition to these utilities we use [Autoprefixer](https://github.com/postcss/autoprefixer)

We have reached stable version of our css conventions and ready to share them with community. It is not our aim to build another scss framework. That is why we are starting to migrate our conventions to [PostCSS](https://github.com/postcss/postcss) which we believe is the future of css and can replace 80% of our scss framework

At the same time we hope that publishing our small scss framework will help somebody to save their development time and that we will get some feedback from other developers about how we can improve our workflow

Thank you and enjoy!

## Overview

### Project structure
Inspired by RoR projects structure we selected the following structure for all our projects:

```
app/
    resources/
    	images/
        	favicon/
            	...
            ...
        fonts/
        	...
        ...
	html/
    	layouts/
        	...
        partials/
        	...
        pages/
        	...
        ...
    js/
    	...
    scss/
    	...
```
#### Resources

Here we store images, fonts and other assets

#### HTML

`html/layouts/` - Reusable wrappers a.k.a. general html components which wraps main html pages of the project

`html/partials` - Reusable content a.k.a. general html components which are being wrapped by main html pages of the project

`html/pages` - HTML pages for static content (like home, terms of use, privacy policy, etc...)

HTML pages for dynamic content (like sessions, users, etc...) are stored at the root of `html` folder and are named by the following convention:

```
ResourceNameFolder/
	SubResourceNameFolder/
    ...
    index.html
    show.html
    new.html
    edit.html
...
```

`index.html` - HTML page to show all resources

`show.html` - HTML page to show specific resource

`new.html` - HTML page to create specific resource

`edit.html` - HTML page to update specific resource

#### JS

JavaScript conventions is not the subject of this repository

### SCSS Structure

The main part is SCSS structure which is the following:

```
scss/
	utilities/
    	mixins/
        functions/
        classes/
    	_variables.scss
    vendor/
    	...
    root/
    	...
    _variables.scss
    _utilities.scss
    _vendor.scss
    _root.scss
    app.scss
	html/
    	layouts/
        	...
        partials/
        	...
        pages/
        	...
        ...
```

#### app.scss

`app.scss` is the entry point for Sass preprocessor. We do not define any rules here. Its only purpose is to import needed framework dependencies in the specific queue and then import all files from the `scss/html` folder:

```scss
@import 'utilities';
@import 'variables';
@import 'vendor';
@import 'root';

...
```

#### utilities.scss

`_utilities.scss` imports our default variables and all utilities which are divided into three main groups __mixins__, __functions__ and __classes__. They are located inside utilities folder and are imported in the following queue:

```scss
@import 'utilities/variables';
@import 'utilities/mixins/class';
@import 'utilities/mixins/offset';
@import 'utilities/mixins/size';
@import 'utilities/mixins/font-face';
@import 'utilities/mixins/font-size';
@import 'utilities/mixins/container';
@import 'utilities/mixins/background-image';
@import 'utilities/mixins/focus-hover';
@import 'utilities/functions/colors';
@import 'utilities/functions/inline-data-url';
@import 'utilities/classes/grid';
@import 'utilities/classes/rectangle';
@import 'utilities/classes/alignment';
@import 'utilities/classes/helpers';
```

##### utilities/variables.scss

`utilities/_variables.scss` stores following technical variables:

If user do not specify custom app title, application will be simply called 'app'

```scss
$app: 'app' !default;
```

If user do not specify custom breakpoint, application will have 3 main breakpoints

```scss
$sm: 40em !default;
$md: 60em !default;
$lg: 80em !default;
```

We have alias for Sass boolean variables

```scss
$f: false;
$t: true;
```

##### utilities/mixins/class.scss

`utilities/mixins/_class.scss` is the main tool to apply our convention and to keep our code clean and maintainable. It is the core of our framework. That is why we will talk about it at the end

##### utilities/mixins/offset.scss

`utilities/mixins/_offset.scss` simplifies work with offsets. Here are some things which you can do with mixins inside it:

```scss
.class {
  @include position((1: 12px), relative);
}

.class {
  @include position((a: 12px), absolute, 1);
}

.class {
  @include position((t: 12px, h: 0), fixed, -1);
}

.class {
  @include margin((!t: 12px));
}

.class {
  @include margin((h: auto));
}

.class {
  @include padding((1: 18px, 4: 6px));
}

.class {
  @include padding((v: 12px, l: 100%));
}
```

will be converted to

```css
.class {
  position: relative;
  top: 12px;
  left: 12px;
}

.class {
  position: absolute;
  top: 12px;
  right: 12px;
  bottom: 12px;
  left: 12px;
  z-index: 1;
}

.class {
  position: fixed;
  top: 12px;
  right: 0;
  bottom: 12px;
  left: 0;
  z-index: -1;
}

.class {
  margin-right: 12px;
  margin-bottom: 12px;
  margin-left: 12px;
}

.class {
  margin-right: auto;
  margin-left: auto;
}

.class {
  padding-top: 18px;
  padding-right: 6px;
  padding-bottom: 6px;
  padding-left: 18px;
}

.class {
  padding-top: 12px;
  padding-left: 100%;
}
```

Moreover, you can use `@include offset();` to build offsets with custom prefixes

```scss
.class {
  @include offset((v: 3px solid #000, h: 1px solid #fff), border);
}
```

will be converted to

```css
.class {
  border-top: 3px solid #000;
  border-right: 1px solid #fff;
  border-bottom: 3px solid #000;
  border-left: 1px solid #fff;
}
```

##### utilities/mixins/size.scss

`utilities/mixins/size.scss` allows you to declare container `width` and `height` and their limits in one line

```scss
.class {
  @include size(100%, 100%);
  @include size(300px, 600px, max);
  @include size(2em, 2em, min);
}
```

will be converted to

```css
.class {
  width: 100%;
  height: 100%;
  max-width: 300px;
  max-height: 600px;
  min-width: 2em;
  min-height: 2em;
}
```

##### utilities/mixins/font-face.scss

`utilities/mixins/font-face.scss` includes three mixins which allow you to include `ttf`, `otf` or `woff` fonts in one line

```scss
@include font-face-ttf('My TTF', 'path/to/example.ttf');
@include font-face-otf('My OTF', 'path/to/example.otf');
@include font-face-woff('MY WOFF', '$encoded-base64-string');
```

will be converted to

```css
@font-face {
  font-family: 'My TTF';
  src: url('path/to/example.ttf') format('truetype');
}

@font-face {
  font-family: 'My OTF';
  src: url('path/to/example.otf') format('opentype');
}

@font-face {
  font-family: 'My WOFF';
  src: url('data:application/font-woff;charset=utf-8;base64,...') format('woff');
}
```

##### utilities/mixins/font-size.scss

`utilities/mixins/font-size.scss` allows you to declare `font-size` and `line-height`. If you do not specify `line-height` parameter, by default it will be set to `1.2`

```scss
.class {
  @include font-size(1.2rem, 100%);
}

.class {
  @include font-size(1.2rem);
}
```

will be converted to

```css
.class {
  font-size: 1.2rem;
  line-height: 100%;
}

.class {
  font-size: 1.2rem;
  line-height: 1.2;
}
```

##### utilities/mixins/container.scss

If you want to split up window height in several parts, usually you will need to set up `height: 100%;` to `html` & `body` containers and to all containers generated by different frameworks (e.g. `<ui-view>` which is generated by `ui-router`). Another way is to create fixed container which will fill full screen size. `utilities/mixins/container.scss` allows you to create such container in one line. Moreover, this mixin allows you to enable/disable scrolling inside created container and enable/disable momentum scrolling inside it

```scss
.class {
  @include container;
}

.class {
  @include container($f);
}

.class {
  @include container($t, $f);
}
```

will be converted to

```css
.class {
  position: fixed;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  overflow: auto;
  -webkit-overflow-scrolling: touch;
}

.class {
  position: fixed;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  overflow: hidden;
}

.class {
  position: fixed;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  overflow: auto;
}
```

__NOTE#1:__ some JS libraries for scrolling or smooth scrolling will not with fixed containers

__NOTE#2:__ there is an [issue](https://remysharp.com/2012/05/24/issues-with-position-fixed-scrolling-on-ios#fixing-juddering) with fixed elements inside container with momentum scrolling being set. However, we think that keeping fixed elements inside fixed elements is a kind of bad practice and it should be avoided

##### utilities/mixins/background-image.scss

`utilities/mixins/background-image.scss` includes three mixins which allow you to set 3 `background-image` to class, before class or after class to represent their `default`, `focus & hover`, `active` states in one line

```scss
.class {
  @include background-image(url#1);
}

.class {
  @include background-image-before(url#1, url#2);
}

.class {
  @include background-image-after(url#1, url#2, url#3);
}
```

will be converted to

```css
.class {
  background-image: url#1;
}

.class::before {
  background-image: url#1;
}
.class::before:focus,
.class::before:hover {
  background-image: url#2;
}

.class::after {
  background-image: url#1;
}
.class::after:focus,
.class::after:hover {
  background-image: url#2;
}
.class::after.__active {
  background-image: url#3;
}
```

##### utilities/mixins/focus-hover.scss

`utilities/mixins/focus-hover.scss` is a simple alias for `&:focus,&:hover`

```scss
.class {
  @include focus-hover {
    color: white;
  }
}
```

will be converted to

```css
.class::before,
.class::after {
  color: white;
}
```

##### utilities/functions/inline-data-url.scss

`utilities/functions/inline-data-url.scss` module was created to gather helpers for inline resources like inline svg icons or inline base64 images. There is only one helper now and its name is `url-svg`

```scss
.class {
  background-image: url-svg(...);
}
```

will be converted to

```css
.class {
  background-image: url(data:image/svg+xml,...);
}
```

##### utilities/functions/colors.scss

`utilities/functions/colors.scss` module collects functions which work with colors

- `set-color` - creates map of shades of specific color
- `gray` - mixes black and white in proportion
- `shade` - mixes custom color and black in proportion
- `tint` - mixes custom color and white in proportion
- `hex-symbols` - excludes '#' symbol from hex colors

```scss
$var: set-color(red);

$var: set-color(red, 25%, 10%, 10%, 25%);

$var: gray(50%);

$var: shade(red, 25%);

$var: tint(red, 25%);

$var: hex-symbols(#fff);
```

we return to

```scss
$var: (darkest: darken(red, 15%), darker: darken(red, 5%), default: red, lighter: lighten(red, 5%), lightest: lighten(red, 15%));

$var: (darkest: darken(red, 25%), darker: darken(red, 10%), default: red, lighter: lighten(red, 10%), lightest: lighten(red, 25%));

$var: gray;

$var: #bf0000;

$var: #ff4040;

$var: fff;
```

##### utilities/classes/alignment.scss

`utilities/classes/alignment.scss` module was created to gather all css solutions to center elements vertically and horizontally. It includes only 'table' method to align content vertically now. The following code will do the magic:

```html
<div class="center_v_table>
  <div class="center_v_table_content>
    Vertically centered content
  </div>
</div>
```

##### utilities/classes/grid.scss

Inspired by [bootstrap grid](http://getbootstrap.com/css/#grid) we created its lite 24 columns version and placed it inside `utilities/classes/grid.scss` module. It includes only to main classes `.row` and `.col`. `.col` class has different states which represent its size and offset. [Bootstrap grid example](http://getbootstrap.com/css/#grid-example-basic) with our grid will look as following:


```html
<div class="row">
  <div class="col __md-2">col-md-1</div>
  <div class="col __md-2">col-md-1</div>
  <div class="col __md-2">col-md-1</div>
  <div class="col __md-2">col-md-1</div>
  <div class="col __md-2">col-md-1</div>
  <div class="col __md-2">col-md-1</div>
  <div class="col __md-2">col-md-1</div>
  <div class="col __md-2">col-md-1</div>
  <div class="col __md-2">col-md-1</div>
  <div class="col __md-2">col-md-1</div>
  <div class="col __md-2">col-md-1</div>
  <div class="col __md-2">col-md-1</div>
</div>
<div class="row">
  <div class="col __md-16">col-md-8</div>
  <div class="col __md-8">col-md-4</div>
</div>
<div class="row">
  <div class="col __md-8">col-md-4</div>
  <div class="col __md-8">col-md-4</div>
  <div class="col __md-8">col-md-4</div>
</div>
<div class="row">
  <div class="col __md-12">col-md-6</div>
  <div class="col __md-12">col-md-6</div>
</div>
```

__NOTE:__ Our grid does the same things as bootstrap grid or any other float grid does. However, we believe that we simplified it as much as possible and left inside only those thing which are needed to implement responsive 24 column grid

##### utilities/classes/rectangle.scss

`utilities/classes/rectangle.scss` allows you to create responsive rectangles and squares and even change their dimensions on breakpoints. Imagine that you need responsive rectangle which proportions will be 1:2 on mobile screens, 1:1 on tablet screens and 1:0.4 on desktop screens. The following code will create it for you:

```html
<div class="rectangle __xs-200 __sm-200 __md-100 __lg-40">
  <div class="rectangle_content">
    Responsive rectangle content
  </div>
</div>
```

##### utilities/classes/helpers.scss

`utilities/classes/helpers.scss` module were we collect small helpers to make our life easier

- `.clearfix` - [clearfix hack by Nicolas Gallagher](http://nicolasgallagher.com/micro-clearfix-hack/)
- `.ellipsis` - replaces long strings with ellipsis
- `.hidden` - adds `display: none !important` to element. It is used by our JS developers as a standard class to hide elements
- `.input-transparent` - we created to simplify inputs customisation. You can create relative container and create any layout for custom input inside it. The last element inside this container should be input with our class. Our class will make input invisible by adding `opacity: 0;` & `appearance: none;` and will force it to fill its relative parent by adding `@include position((a: 0), absolute, 1);

```html
  <div>
    ...
    <input type="file" class="input-transparent">
  <div>
```

#### variables.scss

`variables.scss` stores our custom variables. We believe that creating variables for everything is bad practice and it complicates maintains. In most cases we stores here color maps for our color pallets and inline svg icons only

#### vendor.scss

`vendor.scss` imports vendor stylesheets. By default only one library is included it is [sanitize.css](https://10up.github.io/sanitize.css/). We used to work with different `reset.css` and `normilize.css` libraries. However they were not enough for us. Only `sanitize.css` satisfies us in 100% and that is why we include it

#### root.scss

`root.scss` imports global project stylesheets like animations, fonts, etc. Also it includes some basic rules which we use in all project

- It disables default outline on focus
- It hides webkit scrollbar
- It sets root font size to 10px to simplify rem usage
- It sets default `font-family`, `font-size` and `line-height` to `body`

##### root/animations.scss

Inside `root/animations.scss` we define all css animations which will be used in project. By default it includes `fadeIn` and `fadeOut` effects

##### root/fonts.scss

Inside `root/fonts.scss` we define all font which will be used in project and create mixins to assign them easily. By default it does not import any font faces, but includes `font-sans-serif` mixin which sets up `sans-serif` as a font family. It is used by default in `root.scss` to assign default font family to `body`

##### root/icons.scss

We believe that the best way to use svg icons is the following:

1. Optimise it with [svgo](https://github.com/svg/svgo)
2. Encode it to url encoded format. We use [urlencoder.org](http://www.urlencoder.org) to do it
3. Create function which will take as parameters parts of your svg which possibly will be changed a lot and will return encoded svg string. In most cases we separate as parameters svg icon colors

Inside  `root/icons.scss` we define all those svg functions. It is the only root module which is imported inside `variables.scss` and not inside `root.scss`

##### root/angular.scss

Inside `root/angular.scss` we define all stylesheets which are relevant to `AngularJS` and its libraries. By default it three classes and do the following:

- disables `ng-animate` globally to use it with specific elements only
- imports styles for [ng-cloak](https://docs.angularjs.org/api/ng/directive/ngCloak)
- forces 'Angular Google Maps' to fill parent container

__NOTE:__ to use this module you need to uncomment its import inside `root.scss`

##### root/mobile.scss

Inside `root/mobile.scss` we define all stylesheets that helps hybrid mobile apps to look more natively. By default it includes three styles

- [-webkit-tap-highlight-color: rgba(0,0,0,0)](https://developer.mozilla.org/en-US/docs/Web/CSS/-webkit-tap-highlight-color)
- [-webkit-touch-callout: none](https://developer.mozilla.org/en-US/docs/Web/CSS/-webkit-touch-callout)
- [user-select: none](https://developer.mozilla.org/en-US/docs/Web/CSS/user-select)

__NOTE:__ to use this module you need to uncomment its import inside `root.scss`

### Our conventions

We believe that [DRY](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself) principles do not work in css. Everyone was in situation when you had just finished refactoring in your css files, had remove each duplication in your code and made your code super DRY. However, just after you revived feedback from your client were he asked you to change button on one page and leave it as now on the other. Nightmare

We love using WET principles inside our css code. Such approach allows you to easily edit any attribute. However, if you have one css file, it becomes huge and hard to maintain. That is why we created our conventions and implemented them with sass

#### scss/HTML

Inside `scss/html` folder we stores scss which is grouped by the same principle as html inside `html` folder

```
scss/
  html/
    layouts/
      ...
    partials/
      ...
    pages/
      ...
    ...
```

`scss/html/layouts/` - Scss to create reusable wrappers a.k.a. general html components which wraps main html pages of the project

`scss/html/partials` - Scss to create reusable content a.k.a. general html components which are being wrapped by main html pages of the project

`scss/html/pages` - Scss to create HTML pages for static content (like home, terms of use, privacy policy, etc...)

Scss to create HTML pages for dynamic content (like sessions, users, etc...) are stored at the root of `scss/html` folder and are named by the following convention:

```
ResourceNameFolder/
	SubResourceNameFolder/
    ...
    index.html
    show.html
    new.html
    edit.html
...
```

`index.html` - HTML page to show all resources

`show.html` - HTML page to show specific resource

`new.html` - HTML page to create specific resource

`edit.html` - HTML page to update specific resource

#### utilities/mixins/class.scss

Generally we do not use default '.' or '#' identifiers to create classes or ids inside our scss files. We use them only at edge cases. Instead, we created utility which allows us to follow our conventions. It consists of 3 main mixins

- `namespace` - allows you to define global (ids like #app) and local (classes like .form) namespaces
- `class` - allows you to create classes which names are related to their parents
- `state` - allows you to create class states (classes like .__selected)

In one raw our convention can be written as following:

`#<global-namespace-1>.__<state-1>.__<state-2>.__<...> #<global-namespace-2>.__<state-1>.__<state-2>.__<...> <...> .<local-namespace-1>.__<state-1>.__<state-2>.__<...> .<local-namespace-2>.__<state-1>.__<state-2>.__<...> <...> .<class-parent-1>_<class-parent-2>_<...>_<class>.__<state-1>.__<state-2>.__<...>`

We always use nested selectors syntax inside our scss, because it increase readability. Our utilities fix the biggest problem of nested selectors - generated code complexity

Instead of `TL;TR` guides we prepared an example which we believe explains everything

```scss
@include namespace('my-app', $t) {
  height: 100px;

  @include state('theme-1') {
    background: red;

    @include class('main') {
      background: green;
    }
  }

  @include state('theme-2') {
    background: blue;

    @include class('main') {
      background: yellow;
    }
  }

  @include class('header') {
    height: 20px;

    @include class('description') {
      font-size: 12px;
    }
  }

  @include class('main') {
    background: orange;
    height: 60px;
  }

  @include class('footer') {
    height: 20px;

    @include class('description') {
      font-size: 12px;
    }
  }

  @include namespace('navigation') {
    width: 100px;

    @include state('minified') {
      width: 10px;

      @include class('button') {
        height: 10px;
      }
    }

    @include class('button') {
      height: 20px;
    }
  }
}

```

will be converted to

```css
#my-app {
  height: 100px;
}

#my-app.__theme-1 {
  background: red;
}

#my-app.__theme-1 .main {
  background: green;
}

#my-app.__theme-2 {
  background: blue;
}

#my-app.__theme-2 .main {
  background: yellow;
}

#my-app .header {
  height: 20px;
}

#my-app .header_description {
  font-size: 12px;
}

#my-app .main {
  background: orange;
  height: 60px;
}

#my-app .footer {
  height: 20px;
}

#my-app .footer_description {
  font-size: 12px;
}

#my-app .navigation {
  width: 100px;
}

#my-app .navigation.__minified {
  width: 10px;
}

#my-app .navigation.__minified .button {
  height: 10px;
}

#my-app .navigation .button {
  height: 20px;
}
```

### Summary

We created this tool to optimize our workflow and published it to save other developers time. Feel free to use entire framework or its separate parts which you will find useful. We will appreciate any feedback which will help us to improve our development. Cheers!
