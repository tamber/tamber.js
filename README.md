# Tamber.js

Tamber.js makes it easy to put machine learning personalization into your app or website.

Just start tracking events (user-item interactions) and then get blazing fast, head-scratchingly accurate recommendations for your users.

[Get a free api key][homepage] to create your first project.

## Documentation

Checkout our [Quick Start][quickstart] guide for setup instructions, or our [full API documentation][docs].

## Installation

Just paste this snippet into your html header:

```html
<script type="text/javascript">
    var s=document.createElement("script");s.type="text/javascript",s.src="https://js.tamber.com/1.0.13/tamber.min.js",s.async=!0,document.getElementsByTagName("head")[0].appendChild(s),s.onload=s.onreadystatechange=function(){
        window.tamber = window.tamber("YOUR_PROJECT_KEY");
        window.tamber.trackGuests(true);
    };
</script>
```

Be sure to replace `YOUR_PROJECT_KEY` with your Tamber project's publishable key (available in the [dashboard][dashboard]).

## Usage

Tamber learns from user behaviors, so to get started all you need to do is track Events (user-item interactions) just as you would for any analytics service.

### Set User

If you are using a templating language to generate your pages, call this method from the footer of every page.

```js
// example syntax
<% if user_signed_in? %> 
    window.tamber.setUser(%=user_id%);
<% end %>
``` 

If you retrieve the user's unique ID from your backend on page load, add the method call wherever that retrieval is handled.

```js
window.tamber.setUser("user_id");
```


### Track Events

Track all events (user-item interactions in your app like 'clicked', 'shared', 'purchased', etc.) to your project in real time, just like you would for a data analytics service. Note that novel users and items will automatically be created.

```js
window.tamber.event.track({
    item: {
        id: "item_wmt4fn6o4zlk",
        properties: {
            type: "book",
            title: "The Moon is a Harsh Mistress",
            img: "https://img.domain.com/book/The_Moon_is_a_Harsh Mistress.jpg" 
        },
        tags: ["sci-fi", "bestseller"]
    },
    behavior: "shared",
    context: ["homepage", "for_you_section"]
});
```

Just start streaming events for the behaviors in your app, then kick back and wait for the data to accumulate (~1-2 weeks) before moving ahead with recommendations.

### Get Recommendations

Once you have tracked enough events and created your engine, it is time to put personalized recommendations in your app.

Often it is ideal to handle recommendation retrieval from the backend as part of normal page loading. If you would prefer to handle this in the backend, checkout our [other SDKs][sdks], including [Ruby][tamber-ruby], [golang][tamber-go], [python][tamber-python], and [Java][tamber-java].

Tamber.js provides full support for loading recommendations directly from Tamber.

#### For You

Put personalized recommendations on your homepage, or in any recommended section, by calling `forYou` with the number of recommendations you want to display.

```js
// supply the exact number of items to be displayed
window.tamber.forYou(10, function(err, discoveries) {
    err; // null if no error occurred 
    discoveries; // the items to display in the user's 'For You' section
});
```

#### Up Next

Keep users engaged by creating a path of discovery as they navigate from item to item. Just add the id of the item that the user is navigating to / looking at and the number of items to display.

```js
window.tamber.upNext("item_wmt4fn6o4zlk", 14, function(err, discoveries) {
    err; // null if no error occurred 
    discoveries; // the items to display in the 'Up Next' section of the item page
});
```

##### Infinite Scroll

Tamber's recommendations are optimized for the exact moment and context of the user at the time of request, so standard pagination is not possible. Instead, `forYou` and `upNext` use automatic continuation to allow you to 'show more' or implement infinite scrolling. 

When you want to add more recommendations to those currently displayed to the user, just call `moreForYou` or `moreUpNext`. Tamber will automatically generate the set of items that should be appended to the current user-session's list. The user-session is reset when either `forYou` or `upNext` is called.

```js
window.tamber.moreForYou(10, function(err, discoveries) {
    // handle discoveries
});

window.tamber.moreUpNext("item_wmt4fn6o4zlk", 20, function(err, discoveries) {
    // handle discoveries
});
```

#### Trending

Help your users keep their fingers on the pulse of your platform by showing them the hottest, most popular, newest, or most up-and-coming items.

```js
window.tamber.discover.hot({}, function(err, discoveries) {
    err; // null if no error occurred 
    discoveries; // the hottest (trending) items
});

window.tamber.discover.popular({}, function(err, discoveries) {
    err; // null if no error occurred 
    discoveries; // the most popular items
});

// BETA endpoints
window.tamber.discover.uac({}, function(err, discoveries) {
    err; // null if no error occurred 
    discoveries; // the most up-and-coming items
});

window.tamber.discover.new({}, function(err, discoveries) {
    err; // null if no error occurred 
    discoveries; // the newest items
});
```

## License

Released under the [MIT license][mit].

[homepage]: https://tamber.com/
[docs]: https://tamber.com/docs/
[dashboard]: https://dashboard.tamber.com/
[quickstart]: https://tamber.com/docs/start/
[sdks]: https://tamber.com/docs/libs/
[tamber-ruby]: https://github.com/tamber/tamber-ruby
[tamber-go]: https://github.com/tamber/tamber-go
[tamber-python]: https://github.com/tamber/tamber-python
[tamber-java]: https://github.com/tamber/tamber-java
[mit]: https://github.com/tamber/tamber.js/blob/master/LICENSE.md
