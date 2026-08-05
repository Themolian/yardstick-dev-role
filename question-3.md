Given the following piece of erroring code:

```js
(function($) {
    ////////////////////////////////////
    ///// DOCUMENT READY FUNCTION /////
    //////////////////////////////////
    $(document).ready(function() {
        /////////////////////
        ///// ACCORDION /////
        /////////////////////
        $('.accordion__heading-wrapper').on('click', function{
            var parent = (this).closes('.accordion__item');
            var parentOpen = parent.hasClass('open');

            $(this).closes('.accordion').find('accordion__item').removeclass('open');

            if(!parentOpen) {
                parent.addClass('open');
            }
        })
    })
});
```

The fixes I would make would be:

```js
(function($) {
    ////////////////////////////////////
    ///// DOCUMENT READY FUNCTION /////
    //////////////////////////////////
    $(document).ready(function() {
        /////////////////////
        ///// ACCORDION /////
        /////////////////////
        $('accordion__heading-wrapper').on('click', function() {
            // I'd use let to keep these variables scoped
            
            // Add accordion variable - not an error per-say, just cleaner
            let accordion = $(this).closest('.accordion');

            let parent = $(this).closest('.accordion__item');
            let parentOpen = parent.hasClass('open');

            // add openItem variable for simpler toggling
            let openItem = accordion.find('.accordion__item.open');

            if(openItem !== undefined) {
                openItem.removeClass('open');
            }

            if(!parentOpen) {
                parent.addClass('open');
            }
        });
    });
});
```