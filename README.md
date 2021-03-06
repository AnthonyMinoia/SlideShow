SlideShow
=========

![SlideShow](https://github.com/rpflorence/SlideShow/raw/master/logo.png)

Extensible mid-level class that manages transitions of elements that share the same space, typically for slideshows, tabs, and galleries.

View the [demo site](http://ryanflorence.com/slideshow) for real world examples.

View the [Docs](https://github.com/rpflorence/SlideShow/tree/develop/Docs/SlideShow) for all methods and details.

Features:
---------

- Custom transitions
- Autoplay
- Reverse
- Show next slide
- Show previous slide
- Show arbitrary slide
- Transitions by slide
- Durations by slide
- Default transitions
- Default durations
- On-the-fly transitions

19 Transitions Out-of-the-Box
-----------------------------

- fade
- crossFade
- fadeThroughBackground
- pushLeft, pushRight, pushUp, pushDown
- blindLeft, blindRight, blindUp, blindDown
- slideLeft, slideRight, slideUp, slideDown
- blindLeftFade, blindRightFade, blindUpFade, blindDownFade

Events
------

- `onShow`
- `onShowComplete`
- `onPlay`
- `onPause`
- `onReverse`

CSS Transitions!
----------------

New in 2.0, `SlideShow.CSS` has a method to override JS animations with CSS3 animations.  Still experimental.

How to use
----------

### Simple Example

_HTML_

SlideShow naturally grabs all children elements of a parent as the slides.

    #HTML
    <div id=slideshow>
      <div>Slide 1</div>
      <img src=image.jpg>
      <div>Slide 3</div>
    </div>

_CSS_

Most transitions require absolutely positioned slides **with top and left set to 0**.  SlideShow doesn't do this for you, keeping it more flexible for all types of transitions:

    #CSS
    #slideshow {
      width: 500px;
      height: 300px;
      overflow: hidden;
      position: relative; /* not required, slideshow will set this for you */
    }
    
    #slideshow > * {
      position: absolute; /* required for most transitions */
      top: 0;             /* ditto */
      left: 0;            /* ditto */
      width: 100%;        /* usually required */
      height: 100%;       /* same */
    }

_JavaScript_

    #JS
    var slideshow = new SlideShow('slideshow', {
      transition: 'fadeThroughBackground',
      delay: 5000,
      duration: 400,
      autoplay: true
    });

### Declarative Example

_HTML_

SlideShow can get all of the information it needs from the `data-slideshow` attribute in your HTML, and allows you to have different transitions for each slide.

    #HTML
    <div id=slideshow data-slideshow="transition:fade delay:4000 duration:750">
      <div data-slideshow="transition:pushUp">Slide 1</div>
      <img data-slideshow="transition:slideRight" src=image.jpg>
      <div data-slideshow="transition:blindDown duration:1000">Slide 3</div>
    </div>


_JavaScript_

    #JS
    var slideshow = new SlideShow('slideshow');

### CSS Transitions

First include the `SlideShow.CSS.js` file.  You'll then want to verify if the browser supports CSS transitions and transforms, I typically use Modernizr:

    #JS
    if (Modernizr.csstransitions && Modernizr.csstransforms){
        SlideShow.useCSS();
    }

Browsers that support transitions and transforms will use the new CSS transitions instead of JavaScript.

### Controlling a SlideShow

    slideshow.show('next');
    slideshow.show('previous');
    slideshow.show(2); // shows the third slide
    slideshow.play();

### Creating a navigation interface

    $$('.some-elements-in-the-order-of-the-slides').each(function(item, index){
      item.addEvent('click', function(){
        slideshow.show(index);
      });
    });

### Extending SlideShow with your own transitions

You can create whatever transitions you need with the SlideShow `defineTransition` function.

    SlideShow.defineTransition('flash', function(data){
      data.previous.setStyle('display', 'none');
      data.next.setStyle('opacity', 0);
      new Fx.Tween(data.next, {
        duration: data.duration,
        property: 'opacity'
      }).start(1);
    });
