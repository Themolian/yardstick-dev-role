The first thing I would do here is break each part of the section into its constituent parts.

In this case, that would be the heading and button in their own part, and the icons with text in another part.

In ACF, the content model would look something like:

**Banner w/ Icons (Group)(or if within a Flexible Content field, this would be in a layout)**
- Heading (Text field) (Required)
    - Instructions: This field will serve as a short introductory heading for the banner. 
- CTA (Link field)
    - Instructions: Optionally, use this field to send the user to a page / piece of content

- Icon List
    - The field that would be built for this would look something like:
        - **Icon List (Repeater)**
            - *List Item*
                - Icon (Either uploadable using an Image field, or selectable from a Button Group or some other option field)
                - Assuming the red type below the icon is always going to be a number, I would output this programmatically rather than having it as a field, but if the red type could contain any short content, I could make a field for that.
                - Heading (Text field) (Required)

    - Given that these icons with text are used within a separate timeline block on another section of the site, there are a few ways this could be done:
        1. **Scenario 1: The icon list will have the same content globally and can be inserted into other components**:
            - Build out the icons list in something like a "Global Content" tab within the Options Page set up with ACF.
            - In the Banner field, I would add a note in the instructions that the icon list would be pulled through from that global field and let the content editor know where to find it

        2. **Scenario 2: The icon list is built first in the timeline block**
            - In the Banner block, I would set up a Clone field mapped to the Icon List field in the timeline block. Further down the line, if the client wanted changes made to the field, depending on the context, I would keep in mind that I may need to ask them if they wanted that change done to the icon list in the timeline as well, or whether the change they requested was just for the banner block. If the changes they requested were local to the banner block, I would probably convert the Icons List field into its own instance by duplicating the field from the timeline block and moving the duplicate to the banner block, making sure to rename it / rehandle it appropriately (if nothing else, to remove the (clone) part from the field title and handle)

**Twig Code**

```php
<?php
    // page.php

    $context = Timber::context();
    /* ...template code */ 
    Timber::render('templates/blocks/banner-icons.twig', $context)
?>
```

```twig
{# templates/blocks/banner-icons.twig #}

<div class="block banner-icons">
    <div class="banner-icons-inner">
        <div class="banner__text">
            {# No checks needed as the heading is a required field #}
            <h2>{{ banner_heading }}</h2>
            {% if banner_cta and banner_cta['url'] is not empty %}
                <a href="{{ banner_cta['url'] }}">{{ banner_cta['title'] }}</a>
            {% endif %}
        </div>
        <div class="banner__icon-list">
            {% include("templates/blocks/icon-list.twig", {
                items: banner_icons
            }) %}
        </div>
    </div>
</div>
```

```twig
{# templates/blocks/icon-list.twig #}

<div class="icon-list">
    {% for item in items %}
        <div class="icon-list__item">
            <div class="item__icon">
                {# No alt text needed as icon is decorative #}
                <img src="{{ /path/to/icon.svg }}" alt="">
            </div>
            <span class="item_number">{{ "%02d"|format(loop.index) }}</span>
            <h3>{{ item.heading }}</h3>
        </div>
    {% endfor %}
</div>
```

