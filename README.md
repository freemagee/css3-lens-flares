**Published:** 2011-08-30

I sat down and wrote out some ideas recently about web development and the current boundaries of what is possible with CSS & HTML. Things have certainly moved on a long way in the last few years, and popular websites such as [Smashing Magazine](http://www.smashingmagazine.com) and [CSS-Tricks](http://css-tricks.com) consistently feature articles that feed the imagination.

On many occasions to learn something new you need to experiment. About 13 years ago I began to learn Photoshop. The first version I got my hands on was Photoshop 5. I used to read tutorials in [Computer Arts magazine](http://www.computerarts.co.uk/) on how to create metallic effect text, how to add gradients to images and how to use layers to create opacity effects. This learning period gave me a great foundation in the software and indirectly led to where I am today, working as a professional web designer.

What strikes me as slightly ironic is the web industry is beginning to create effects with CSS3 that I first learned about in Photoshop all those years ago. I fortunately have learned how to use these effects in Photoshop far more subtly nowadays but my imagination was inspired when I looked at the possibility of combining two CSS3 properties – border-radius & radial-gradients.

I present to you an effect that represents an almost humourous level when mentioned to design professionals, the humble Photoshop lens flare.

![Example of a Photoshop lens-flare](http://neilmagee.com/library/img/article-assets/lens-flare-example.jpg)

Example of a Photoshop lens-flare

The example above is the Photoshop lens flare in one of its basic forms. It was arguably over-used on images many years ago, and is probably still put to good use in dental advertisements to this day. So I have decided to take time to honour the lens flare and try to recreate the effect in CSS. It’s only fair as CSS matures that this effect is available in your toolbox.

*Please note that the code below is just for fun and is not semantic nor built for graceful degradation.*

### Part 1 – Where I use HTML to form the basic structure

In the above HTML I have created a class called ‘lens-flare’ and applied it to a DIV tag that contains a number of DIV’s that all have a different ‘circle’ class applied to them. Each of these circles will form the elements I can style to look like lens-flare ‘rings’. Pretty simple so far.

```HTML
<div class="lens-flare">

    <div class="circle-a"></div>
    <div class="circle-b"></div>
    <div class="circle-c"></div>
    <div class="circle-d"></div>
    <div class="circle-e"></div>
    <div class="circle-f"></div>
    <div class="circle-g"></div>
    <div class="circle-h"></div>
    <div class="circle-i"></div>
    <div class="circle-j"></div>
    <div class="circle-k"></div>

</div>
```

### Part 2 – Where I begin to style the HTML with CSS

```CSS
html, body {
    height: 100%;
}
body {
    background: #000;
    margin: 0;
    padding: 0;
}
.lens-flare {
    width: 100%;
    min-height: 100%;
    position: relative;
    overflow: hidden;
}
```

I wanted the lens-flare to display full width & height in my browser. So I set the html & body to a height of 100% and set my lens-flare class to have a minimum height of 100%. This means the demo will scale to various browser heights and widths. This produces an interesting effect. One I will briefly explain later on in this article in the [Bonus section](#bonus).

I also set the `position:relative` so that I can position my circles exactly and finally I set `overflow:hidden` so that the lens-flare could be embedded in a page and not exceed its boundaries. This setting is optional.

### Part 3 – In which CSS3 is used to transform a mundane square into a humble circle

```CSS
.circle-a {
    border-radius: 75px;
    width: 150px;
    height: 150px;
    background: red;
}
```

The basic technique here is to apply border-radius to the DIV with a radius equal to half the width/height of the box. As I am using exact squares the effect this produces is a circle. If you want to test this, give the class a background colour:

This will give you a red circle that is 150px wide by 150px tall i.e. 150px diameter. Pretty cool huh? Now I will move onto how I am going to position my circles to mimic the Photoshop lens-flare.

### Part 4 – Where a little positioning know how lines up my circles

If I return to the class ‘circle-a’ once more and show you the positioning CSS I am going to be using throughout this example.

```CSS
.circle-a {
    border-radius: 75px;
    width: 150px;
    height: 150px;
    margin: -75px 0 0 -75px;
    background: red;
    position: absolute;
    left: 12%;
    top: 12%;
    z-index: 1;
}
```

I will take you through my positioning logic. The first method is to apply a negative margin to the top and left of the DIV. This is extremely useful as it allows you to use the center point of this DIV for positioning, rather than the top-left corner which is the origin point used by default. I then position the box absolutely, setting it to exist at 12% down from the top and 12% in from the left. I set the container DIV ‘lens-flare’ to `position:relative` earlier therefore the circles will position themselves relative to it. Exactly what I want to achieve.

The 12% is arbitrary at the moment and any value can be used, as long as both ‘left’ and ‘top’ are the same value. This equal setting will allow all my circles to be positioned on the same diagonal as each other. This is important to maintain similarity to the original Photoshop effect.

Finally I give the DIV a z-index so that I can control the ‘layers’ that the circles appear on. I want them all to exist on their own layers to avoid any possible conflicts in their rendering.

### Part 5 – In which the radial-gradient magic is applied

Now that the shape and position of the circle are resolved, I can move onto making the circle appear more like a lens-flare effect. I achieve this by combing CSS3 radial gradients with RGBA values to control the opacity of the colours.

So I refer back to the original ‘circle-a’ class. I remove the `background: red` rule and replace it with this rule:

```CSS
background: radial-gradient(ellipse at center, rgba(34,46,24,0.2) 30%, rgba(80,201,32,0.6) 100%);
```

I am using the new radial-gradient syntax. More on the syntax and a greater explanation of how to use radial-gradients can be found over at [Web Directions](http://www.webdirections.org/blog/css3-radial-gradients/). Now I will explain the values I am using in my example.

The first pair of percentages after the `background: -webkit-radial-gradient(` are the location of the origin of the radial-gradient. I have chosen my radial-gradient to be centered within my DIV so I set it to be 50% & 50% respectively. The next pair of values cover the shape and the behaviour of the radial-gradient. I want a circlular shape, hence the explicit ‘circle’ declaration and I set the behaviour to ‘closest-corner’ because in my example the radial-gradient extends to the corners of my DIV. That provides optimal coverage within my DIV.

The final part of the style declaration is the fun part – setting the colour values and ‘stops’. I used my RGB colour picker in Photoshop to get some values for my first circle. I chose a dark green (R:34, G:45, B:24) with a low opacity of 0.2 (max is 1.0) for the first colour. The second colour in this gradient is a brighter green (R:80, G:201, B:32) with a opacity value of 0.6. What this produces is a gradient that goes from a dark green towards the center to a bright green towards the edges. The opacities are important because this allows the circles to interact with each other and for circles to be visible within each other.

![Example of gradient](http://neilmagee.com/library/img/article-assets/gradient-example.png)

Example of colour stops on a gradient

You may have noticed the percentage values after the RGBA declarations in the CSS rule. These are the colour ‘stops’ that the gradient adheres to. The above example shows a literal interpretation of colour stops on a linear-gradient. If you imagine a line from 0% to 100%, then the first colour begins at a point that represents 0% on this line, the second colour begins at 50% and finally blends into the third colour at 100%. The effect as seen in the example is a gradient that blends from blue to yellow and to blue once more.

Radial-gradients work the same way as linear-gradients, except they are repeated in a radial pattern. The gradient is drawn along the radius of the circle. In my CSS code for ‘circle-a’ this produces a circle that contains a dark green towards the center that slowly blends into a brighter green towards the edge. If you have used any of the gradient tools in Photoshop, then the concept is the same.

So the complete CSS code for my first circle is:

```CSS
.circle-a {
    background: radial-gradient(50% 50%, circle closest-corner, rgba(34,45,24,0.2) 30%, rgba(80,201,32,0.6) 100%);
    border-radius: 75px;
    width: 150px;
    height: 150px;
    margin: -75px 0 0 -75px;	
    position: absolute;
    left: 12%;
    top: 12%;
    z-index: 1;
}
```

### Part 6 – Rinse & repeat

Now it is just a case of repeating the steps we have taken so far for all the remaining circles. I have varied their position, size & colours to mimic the original Photoshop lens-flare effect.

Bonus Points
------------

View the demo and resize your browser for [special parallax scrolling effect](http://en.wikipedia.org/wiki/Parallax_scrolling). This is an interesting side-effect of the percentage based positioning, and its relationship to the browser window. Possible CSS3 animatable radial-gradient coming in the future.

- [View Demo](https://freemagee.github.io/css3-lens-flares/)
