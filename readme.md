# Pico Pagination Plugin

Provides basic pagination for [Pico](http://pico.dev7studios.com).

## Changelog

**1.1** - Added ability to reverse the order in which the previous/next links are rendered.  
**1.0** - Initial release.

## How it works

It's pretty simple. The plugin creates a second array of pages identical to the default `pages` called `paged_pages`. This second array is filtered by date (so only blog-type posts will be returned) and limited by the number of pages you want per page. Listing your paged results, therefore, is just a matter of iterating through the `paged_pages` variable instead of the `pages` one.

Additional variables that can be used in themes are created to accompany the paged results. See below.

## Installation

1. Copy the pagination.php file to the plugins folder of your Pico site.
2. Update your theme to use the paginated pages generated by the plugin.
3. Set configuration variables if defaults are not suitable.
3. That's it!

## Configuration settings

You can configure a number of settings by adding values to your site's config.php file. The following are the options available and what they do.

*pagination_limit*  
Sets the how many items display on each page.  
**Default value: 5**

*pagination_next_text*  
Sets the text for the next link.  
**Default value: "next >"**

*pagination_prev_text*  
Sets the text for the previous links.  
**Default value: "< previous"**

*pagination_flip_links*  
Reverses the order the links are ouput. This is to aid in providing links in the format of older/newer as opposed to previous/next.  
**Default falue: false** | Options: true, false

*pagination_filter_date*  
Sets whether the posts returned should be filtered to only those with dates or not.  
**Default value: true** | Options: true, false

~~*pagination_page_indicator*  
Sets the word used in the URL that will indicate paged results. (i.e. http://yousite.com/page/2)  
**Default value: "page"**~~  
(This option is not currently available<sup>1</sup>.)

*pagination_output_format*  
Sets whether `{{ pagination_links }}` will output two `<a>` tags or an unordered list.  
**Default value: "links"** | Options: "links", "list"

For reference, these values are set in config.php using the following format:

	$config['pagination_limit'] = 10;
	$config['pagination_output_format'] = "list"

<sup>1</sup> _This option is not available because Pico processes the URL before it reads the configuration values. This makes it so that a configuration value cannot be used in the URL. If this gets changed it Pico, it will be made available._

## Setting up the theme

The plugin adds a couple new variables to the theme. They are:

- `{{ pagination_links }}` - The generated next and previous links (output as either two links or an unordered list).
- `{{ next_page_link }}` - The next link only. Returns empty string if there isn't one.
- `{{ prev_page_link }}` - The previous link only. Returns empty string if there isn't one.
- `{{ page_number }}` - The current page number
- `{{ page_of_page }}` - The page number out of the total number of pages. (i.e. "Page 1 of 3")
- `{{ paged_pages }}` - The new, paged array of pages for the theme.

To get a basic implemenation of the pagination plugin going, use something like the following:
	
	{% if is_front_page %} <!-- Front page lists all blog posts -->
			
		<div id="posts">
		{% for page in paged_pages %}
			<div class="post">
				<h3><a href="{{ page.url }}">{{ page.title }}</a></h3>
				<p class="meta">{{ page.date_formatted }}</p>
				<p class="excerpt">{{ page.excerpt }}</p>
			</div>
		{% endfor %}
		</div>
		{{ pagination_links }}
		
	{% else %} <!-- Single page shows individual blog post -->
	
		<div class="post">
			{% if meta.title %}<h2>{{ meta.title }}</h2>{% endif %}
			<p class="meta">{{ meta.date_formatted }}</p>
			{{ content }}
		</div>
	
	{% endif %}

**To note:** the key differences here are that the posts loop is iterating through *paged_pages* instead of *pages*. (This sample is taken [from the Pico](http://pico.dev7studios.com/docs.html#blogging) website and modified for the pagination plugin.)

### Your Site Navigation

If you're familiar with Pico, this may be obvious, but for your main site menu, you can filter it so that content with dates (i.e. your blog posts) don't get output along with your other pages. This is pretty easy to do just with a little if statement. Here's an example:

	<ul class="nav">
		{% for page in pages %}
		{% if not page.date %}
		<li><a href="{{ page.url }}">{{ page.title }}</a></li>
		{% endif %}
		{% endfor %}
	</ul>


### Using Older/Newer links instead of Previous/Next

A lot of blogs (or other chonologically listed content) will want pagination links that use chonological verbiage as opposed to the standard previous/next text. In order to do this, you can set the following config options in Pico's `config.php` file.

	$config['pagination_flip_links'] = true;
	$config['pagination_next_text'] = '< Older Posts';
	$config['pagination_prev_text'] = 'Newer Posts >';

Note the first option set. This changes the order in which the links display so that the older link is first and the newer link is second.

---

That's it. If you have questions, go ahead and ask. If you have issues, you can add them to the [issue tracker](https://github.com/rewdy/Pico-Pagination/issues).
