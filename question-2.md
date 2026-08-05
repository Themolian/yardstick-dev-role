Assuming the `posts` variable is the data returned by a `WP_QUERY` querying the `staff` (or something similar) post type:

The first thing I would check would be if the handle of the `job_title` field matched up with what was being called in the `post_meta` method - Occam's Razor.

The second thing I would check is whether or not it's not populated in the CMS. If that was the case, I would probably address this in one of two ways:

1. Make the `job_title` field required so that team member posts cannot be saved without populating the `job_title` field.

or

2. Add an if check to the code to make sure the code still outputs even if the `job_title` field isn't populated:
    ```twig
        {% for post in posts %}
            <h2>{{ post.title }}</h2>
            <p>{{ post.meta('job_title') }}</p>
        {% endfor %}
    ```

I would probably also check whether the correct data was being passed to the twig component. If `posts` was referring to the default `posts` rather than the `staff` post type, it would be unlikely to have a custom field of `job_title`!
```