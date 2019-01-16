# Beginner Sass Workshop

> This workshop (and everyone else, ever) actually uses **SCSS**, referred to on the website as 'Sassy CSS'. Sass was the original syntax, **SCSS** is the new version. 
> 
> The terms 'Sass' and 'SCSS' will be used interchangeably throughout this tutorial to refer to 'SCSS'. [More on this Stackoverflow post](https://stackoverflow.com/questions/5654447/whats-the-difference-between-scss-and-sass)

![sass logo](http://davidleger95.github.io/whitespace/assets/img/post/sass-math.svg)

## Why Bother?
- Your stylesheet for your project is larger and harder to organise as it gets larger
- You are getting tired of finding and replacing repeated css values, such as colours
- You like DRY code
- You know a few programming features (e.g. a constructor function, array, loop, conditional statement), and feel you could apply them to writing css in a more concise way
- You like to concept of splitting stuff up into partials (such as in handlebars.js)
- Lots of things depend on SCSS, as opposed to CSS to work. For example, [Material UI](https://codelabs.developers.google.com/codelabs/mdc-101-web)

### Caveat(s)
- This workshop comes from the [mr-bagglesworth](https://github.com/mr-bagglesworth), who is almost certainly isn't using SCSS to it's full potential.
- Therefore, you should see this tutorial as an overview of some of the **basics** of SCSS to get you started _and please, don't blame the author if things go wrong_.
- This contains no solutions files, just a few tips and code examples to help you along the way.

![rooting for you](https://media1.tenor.com/images/8a322e94bdb253a5fb42d010480d0163/tenor.gif?itemid=5104276)

---

# The Workshop
For this workshop, we will be taking a plain css file, and **refactoring it to be compiled with scss**.

Obviously this isn't how you will go about using sass in an actual project, it is more just to **demonstrate how and why it is useful**.

With each step, don't go too crazy, **just sample the feature**. Some are more useful than others.

**Before we proceed you should bear in mind the following points:**
> Sass is a css **preprocessor**. This means that styling is still controlled by a css file, which sass **compiles** as you edit your scss files. You still link your css file in your html as you always have done.
> 
> Sass **extends** the functionality of css. Valid css is valid sass.

---

## Topics Covered Include:
- [The Watch Command](#watch)
- [Partials (and imports)](#partials)
- [Comments in sass](#comments)
- [Nesting](#nesting)
- [Use of the ampersand (&)](#ampersand)
    - a) In referencing variations of the [same element](#ampersand-same) (or with a similar name)
    - b) In referencing the [parent element](#ampersand-parent)
- [Variables](#variables)
- [Extends and Placeholders](#extends)
- [Mixins](#mixins)
- [Interpolation](#interpolation)
- [For loops](#loops)


---

## Part 1
**1. Clone the project**
**2. [Install Sass](https://sass-lang.com/install)**. I personally run a standalone version of sass that I installed a long time ago. There is now a node version, which also may be of interest.
**3. Split up the css.** Looking at the starting css, decide which parts might logically split off into separate files. For example:
- reset
- typography
- forms
- header
- etc...

**4. <span id="partials">Partials and Imports:</span>**
**a)** Copy and paste the different parts of the css into new **partial** files. These are declared by starting the filename with an underscore, e.g. **_forms.scss**.

**A file structure might be:**

```vim
css/
    - style.css (this file is empty to start with)
scss/
    - style.scss (this file compiles to style.css)
    - _reset.scss
    - _variables.scss
    - _base.scss
    - _nav.scss
    ...
```
**b)** Create a file to import partial files, e.g. **style.scss**. This file will compile the partials to css in the next step.

**An import file might contain:**
    
```scss
@import "./reset.scss";
@import "./variables.scss";
@import "./base.scss";
@import "./nav.scss";
```


**5. <span id="watch">Watch Command:</span>**
Go to your terminal, and run the `sass --watch` command, to compile your partial files into one **style.css** file:
```ruby
sass --watch scss/style.scss:css/style.css
```
You will see the terminal respond to changes as you edit your scss, with <span style="color:green">green text</span> if css is compiling ok, and <span style="color:red">red text</span> if it is erroring. To stop compiling, press `ctrl + c`.

![tea break](https://www.telegraph.co.uk/content/dam/food-and-drink/2018/04/19/TELEMMGLPICT000135698809_trans_NvBQzQNjv4BqpVlberWd9EgFPZtcLiMQfyf2A9a6I9YchsjMeADBa08.jpeg?imwidth=450)
> Nice! Time for a tea break



## Part 2

With your css split up into logical partials, it's time to get sassy. First things first...

**1. <span id="comments">Comments:</span>**
To write comments in scss, you can also use double forward slashes, as you do in JavaScript. If it don't work, tell others what you are trying to achieve. If it does, still tell them!
```scss
\\ this is a one line comment

\*
this is a multi-
line
comment.
*\
```
<span style="color:red">**Your Task**:</span> Write some comments.

**2. <span id="nesting">Nesting:</span>**
In scss, you can nest child elements within parent elements. For example:
**before:**
```css
ul {
    list-style: none;
}
ul li {
    line-height: 2;
}
ul li a {
    font-size: 1.5rem;
    font-weight: bold;
    text-decoration: none;
    color: red;
}
```
**after:**
```scss
ul {
    list-style: none;
    li {
        line-height: 2;
    }
    a {
        font-size: 1.5rem;
        font-weight: bold;
        text-decoration: none;
        color: red;
    }
}
```
<span style="color:red">**Your Task**:</span> Nest some sass.


**3. <span id="ampersand">Using the Ampersand:</span>**

**a) <span id="ampersand-same">In referencing variations of the same element or similarly named classes</span>**
The Ampersand can be used to chain css selectors together, such as:
- multiple classes on one element
- psuedo classes
- classes that begin the same

**For example this:**
```css
.profile {
    background-color: white;
}
.profile-details {
    width: 60%;
    margin: 1rem 0 2rem;
    padding: 1rem;
    border-radius: 0.5rem;
    background-color: skyblue;
}
.profile-item {
    font-size: 1.5rem;
    color: white;
}
.profile-item:hover {
    border-bottom: 2px solid;
}
.profile-item.red {
    color: red;
}
.profile-item.red:hover {
    color: dark-red;
}
```

**Would become this** (with a bit of nesting):
```scss
.profile {
    background-color: white;
    &-details {
        width: 60%;
        margin: 1rem 0 2rem;
        padding: 1rem;
        border-radius: 0.5rem;
        background-color: skyblue;
    }
    &-item {
        font-size: 1.5rem;
        color: white;
        &:hover {
            border-bottom: 2px solid;
        }
        &.red {
            color: red;
            &:hover {
                color: dark-red;
            }
        }
    }
}

```
<span style="color:red">**Your Task**:</span> Use the ampersand to chain selectors together.


**b) <span id="ampersand-parent">In referencing the parent element</span>**
You can use the ampersand **after** the selector to style an element based on it's context. **For example this**:
```scss
.heading {
    .enter-details & {
        color: skyblue;
    }
}
```
**...would compile to this:**
```css
.enter-details .heading {
    color: skyblue;
}
```
<span style="color:red">**Your Task**:</span> Use the ampersand to style an element based on it's context / parent selector.

![go on](https://media3.giphy.com/media/W8Nte2s2kxxLy/giphy.gif?cid=3640f6095c3d23cc436839745574db95)
> Phew! Time for another tea break.

## Part 3

**1. <span id="variables">Variables:</span>**
Sass variables can make it easier to keep track of repeated css values, such as colours, fonts, gradients, icons, and shadows. A common way to organise variables is to group them together in a _partial file, then import the variables partial at the top of your file.

**You can declare and use sass variables like so:**
```scss
$light-black: #363636;
$font-main: 'Mukta', sans-serif;
```
```scss
h1 {
    color: $light-black;
    font-family: $font-main;
}

```
<span style="color:red">**Your Task**:</span> Make a new partial with some useful variables, and apply them to the rest of your scss partials.

> More useful things you can do with colour variables [can be found here](https://robots.thoughtbot.com/controlling-color-with-sass-color-functions#adding-alpha-transparency)


**2. <span id="extends">Extends and Placeholders:</span>**
An **extend** extends the styles (css property: value pairs) of an element like so:
```scss
.heading {
    font-size: 3rem;
    color: black;
    line-height: 1.5;
}
h2 {
    @extend .heading;
    // you can add styles specific to h2 element here...
}
```
...will compile to:
```css
.heading {
    font-size: 3rem;
    color: black;
    line-height: 1.5;
}
h2 {
    font-size: 3rem;
    color: black;
    line-height: 1.5;
    /* extra styles can be compiled to here */
}
```
A **placeholder** will work in exactly the same way, but not output the original css. A placeholder is generated with a **% sign** like so:
```scss
%heading {
    font-size: 3rem;
    color: black;
    line-height: 1.5;
}
h2 {
    @extend %heading;
}
```
...will compile to:
```css
h2 {
    font-size: 3rem;
    color: black;
    line-height: 1.5;
}
```
<span style="color:red">**Your Task**:</span> Give that a whirl. Obvs.

**3. <span id="extends">Mixins</span>**
Now we are talking. This is where sass begins to get sassy. Oh yeah. Imagine you have:
- **Multiple styles of heading:** different font sizes, weights, colours and line heights.
- **Three styles of button:** One which includes an icon, one which includes a funky hover transition, and one which is pretty plain.

**Mixins** are ideal for this situation. They are like a cookie-cutter, or constructor function for your css, taking in values (perhaps in the form of variables), and outputting css.

There are endless blog posts moaning about extends and placeholders (which do have their place), but mixins tend to have the edge most of the time.

**SCSS Mixin Example:**
```scss
@mixin header($sm-size, $lg-size, $line){
    font-size: $sm-size;
    font-weight: $bold;
    line-height: $line;
    margin: 0 0 1rem;
    @media only screen and (min-width: 600px) {
        font-size: $lg-size;
    }
    @content;
}
h1 {
    @include header(2rem, 3.25rem, 1.6);
    text-align: center;
}
```
> here, the [**@content** directive](https://robots.thoughtbot.com/sasss-content-directive) allows you to pass extra styles to an element when using a mixin

**Resulting CSS:**
```css
h1 {
  font-size: 2rem;
  font-weight: 700;
  line-height: 1.6;
  margin: 0 0 1rem;
  text-align: center;
}
@media only screen and (min-width: 600px) {
    h1 {
      font-size: 3.25rem;
    }
}
```
<span style="color:red">**Your Task**:</span> Take a wild guess...

![bertea](https://media1.giphy.com/media/LJRf8PxI30qWs/giphy.gif?cid=3640f6095c3d385876635975328bd7ec)
> Time for tea, Bertie!

## Part 4
**1. <span id="interpolation">Interpolation:</span>**
Interpo-whatnow???
Making selectors from things you enter into mixins and whatnot. Similar to the template literal / backtick syntax in vanilla JavaScript.

**For example, in a button mixin:**
```scss
// variables from another partial
$green: #1f7736;
$green-hover: #0ed642;
$yellow: #fcbc00;
$yellow-hover: #b98b00;
...etc

// the mixin
@mixin btnColour($name, $colour, $hover){
    &.#{$name} {
        background-color: $colour;
        &:hover,
        &:focus {
            background-color: $hover;
        }
    }
}

// and it's application:
.button-submit {
    /*
    ... default button styles to go here
    */ 
    @include btnColour('login', $green, $green-hover);
    @include btnColour('logout', $yellow, $yellow-hover);
    @include btnColour('date-sort', $green, $green-hover);
    @include btnColour('history', $blue, $blue-hover );
}

```

**And the resulting css:**
```css
.button-submit.login {
    background-color: #1f7736;
}
.button-submit.login:hover,
.button-submit.login:focus {
    background-color: #0ed642;
}
.button-submit.logout {
    background-color: #fcbc00;
}
.button-submit.logout:hover,
.button-submit.logout:focus {
    background-color: #b98b00;
}
... you get the idea
```

**2. <span id="loops">For loops:</span>**
So you like mixins, but think they are a bit OTT for the task at hand. Say you have a dashboard and have a load of labels, each with different background colours. With the mixin, you would end up doing an **@include** for each version of the label. Not very DRY. :umbrella: 

**Enter the for loop:**

```scss
// variables from another partial
$red: #e34c26;
$yellow: #f1e05a;
$blue: #40b1dc;
$green: #1f7736;

$languages : 'english', 'spanish', 'french', 'german';   
$colour_codes : $red, $yellow, $blue, $green;
@for $i from 1 through length($languages) {
    .label.#{nth($languages, $i)} {
        background-color : nth($colour_codes, $i);
        color: white;
        padding: 0.2rem;
    }
}
```
> Note: the above uses an array-like thing called a [Sass list](https://hugogiraudel.com/2013/07/15/understanding-sass-lists/)

**This will output the following css:**

```css
.label.english {
    background-color: #e34c26;
    color: white;
    padding: 0.2rem;
}
.label.spanish {
    background-color: #f1e05a;
    color: white;
    padding: 0.2rem;
}
...etc
```


---

# Well done! :trophy: 


![I got sass](https://media.giphy.com/media/FPl5jDN6WaY36/giphy.gif)
> You got the sass

## More Resources

- [Sass Cheatsheet](https://devhints.io/sass)
- [--watch Yo Sass](http://sassbreak.com/watch-your-sass/).... [Reminds me of this](https://www.youtube.com/watch?v=M7B5KwXHFtw)
- [Beware of Nesting](https://www.sitepoint.com/beware-selector-nesting-sass/)
- [The Ampersand](https://css-tricks.com/the-sass-ampersand/)
- [Colour and sass functions](https://robots.thoughtbot.com/controlling-color-with-sass-color-functions)
- [The Extend Concept](https://css-tricks.com/the-extend-concept/)
- [Mixins vs. Extends](https://csswizardry.com/2014/11/when-to-use-extend-when-to-use-a-mixin/)
- [How to use mixins](https://scotch.io/tutorials/how-to-use-sass-mixins)
- [Sass Interpolation](https://webdesign.tutsplus.com/tutorials/all-you-ever-need-to-know-about-sass-interpolation--cms-21375)
- [if / else statements](https://alligator.io/sass/if-else-statements/)
- [sass lists](https://hugogiraudel.com/2013/07/15/understanding-sass-lists/)

### Not covered in this workshop:
- [sass maps](https://www.sitepoint.com/using-sass-maps/)
