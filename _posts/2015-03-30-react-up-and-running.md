---
title: React - Up and Running
comments: true
series: React Data Grid Tutorials
---

##Fashionably Late
I always seem late to the party. I was late getting into Node.js, and then Ember & Angular. I played with ruby well after it was the hot topic, and I went straight to Gulp after only a short time with Grunt. And per the usual, just a couple weeks ago I started trying [React](https://facebook.github.io/react/). This party is just getting warmed up.

Learning frameworks for me has always been difficult.  I tend to lean towards the anti-framework models of straight PHP, or Rails-less ruby.  However, trying React, something clicked. I mean, I actually get what it's trying to do, and the understanding happened quickly.

Using the [getting started](https://facebook.github.io/react/docs/getting-started.html) tutorial provided by Facebook, I was able to instantly see the results:

{% highlight html %}
<!DOCTYPE html>
<html>
  <head>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/react/0.13.1/react.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/react/0.13.1/JSXTransformer.js"></script>
  </head>
  <body>
    <div id="example"></div>
    <script type="text/jsx">
      React.render(
        <h1>Hello, world!</h1>,
        document.getElementById('example')
      );
    </script>
  </body>
</html>
{% endhighlight %}

Open `index.html` in a browser, and it's already working.

##99 Data Problems
Rendering html in this way is super simple, and doesn't really get into the power of React. Then I started working with data, and that's where I started to really see things make sense.  Granted, it took a second to understand the one-directional data flow, but it was literally only a second. You just have to learn to [think in react](http://facebook.github.io/react/docs/thinking-in-react.html).

In order to not only better understand what's going on myself, but to also help other developers getting started, I'm going to write a few blog posts to dive into React piece by piece, and at the end, end up with a finished product that would be useful.

##The Plan
Let's build a data grid.  The concept isn't anything unusual, and it's useful in any UI project you might work on.  I'll break down components bit by bit, and at the end, we will have a finished, reusable component.  We'll laugh, cry, and learn together a bit more about React, and it's inner workings.  

The first step is, I want to think about how the finished product will work. For that, I turn to [Balsamiq](https://balsamiq.com/) to create a quick mockup that details exactly what I want.

<img src="../../../assets/images/data_grid_mockup.png" class="img-responsive" />

There are three components that will be working together here:
1. Title Bar - it will contain the data grid title, and a filter component to search the data.
2. Data Grid - this will display the data headers, allow for sorting, and display the data.
3. Pagination - we also need something to show a few options, and eventually page through data results.

It's a straight forward example, but I think it has enough going on to make it really useful.

##Initial Mockup
The first place to start is a static mockup.  I'm actually going to mock this up using components, so we can begin to think about how the pieces fit together. We will build several pieces that will ultimately be data driven - and we will use some build tools to make everything a bit easier to work with. For now - I will keep this in one file, `index.html`.

###Capitalization Counts!
When building out our mockup, we will be creating a few components.  It is important to note some syntax that is vital to getting these to work.

Each class needs to be a capitalized.  In other words, `<DataGrid />` is what we are looking for. `<dataGrid />` will not work properly.


###The Components
We will get to really splitting these components apart in a later tutorial, but first - there are three main components, and I will stub out those items so we can get the everything moving.

####Data Grid
First, let's make a component that houses everything together, and call it `DataGrid`.  

{% highlight js %}
var DataGrid = React.createClass({
  render: function() {
    return (
      <div>
        Hello, Data Grid!
      </div>
    );
  }
});
{% endhighlight %}

Notice the capitalization? The first character  must be a capital. I tend to like more camel casing, so I also capitalize the G, but it's not a requirement.  

Second item to notice - `React.createClass({})`.  `createClass()` is what tells React that we are building a component.  It takes an object as a property. That object contains various functions that will perform the different tasks we need later, like filtering or paging.  For now, it only contains the function `render`.  This indicates what will eventually be rendered into the DOM.  

I'm purposefully going to avoid what React is doing behind the scenes. Just know that the `JSXTransformer.js` file is required, and it's turning the XML markup in the render function into JSON objects. For now - we don't need to get any deeper than that.

One last item to note - the text is wrapped in a `<div>` tag.  React will error without a wrapper element. In other words, you have to have everything returned in between an open and close tag.  Anything else will cause your components to not render.

Now that I have my main component `DataGrid` set up, I render it into a `<div>` on the page.

{% highlight js %}
React.render(
  <DataGrid />,
  document.getElementById('dataGrid')
);
{% endhighlight %}

Now, let's make the `DataGrid` hold other components inside.

####Title Bar, Data Table, Pagination

I'm going to start out really simple here, and just render some text.  We can go back build out markup later.

{% highlight js %}
var TitleBar = React.createClass({
  render: function() {
    return (
      <div>
        Title Bar
      </div>
    );
  }
});

var DataTable = React.createClass({
  render: function() {
    return (
      <div>
        Data Table
      </div>
    );
  }
});

var Pagination = React.createClass({
  render: function() {
    return (
      <div>
        Pagination
      </div>
    );
  }
});
{% endhighlight %}

These will eventually hold all the markup we need to get our static mockup completed. What we need to do at this point, however, is add these to our `DataGrid` component, because just adding them doesn't do anything. Let's modify the `DataGrid` to look like so:

{% highlight js %}
var DataGrid = React.createClass({
  render: function() {
    return (
      <div>
        <TitleBar />
        <DataTable />
        <Pagination />
      </div>
    );
  }
});
{% endhighlight %}

Now our text from each component is rendered where the `<ComponentName />` is listed, and as before, wrapped in a container `<div>`.  

###Making it look real
I like using [Bootstrap](http://getbootstrap.com/), so I ended up using it here as well. There is a [React-Bootstrap](http://react-bootstrap.github.io/) library as well, and I may find a spot to use it. For now, I'll keep it simple. One item that has to be mentioned - [html properties](https://facebook.github.io/react/docs/tags-and-attributes.html) are not 1:1 transfers in React.  Most notably - `class` is actually `className`. This is important during the process of adding Bootstrap.  If your page doesn't render, open the console, and React does a great job of indicating where `class` should be `className`.

I took the liberty of building out the rest on my own to give it more of a final look and feel. Since this isn't really about designing with Bootstrap, I won't cover all of those steps here.

##Source Code and Next Steps
This is by all means not a perfect mock-up. There are a few items I'm just not happy with yet, but I'll continue to tweak in future posts, as we dive into each component directly.

I've created a [GitHub repo](https://github.com/kellyjandrews/react-tutorial/tree/static-mockup) with the source code, and I'll be adding to it as I create more tutorials.  You can clone what I have so far in your desktop, and run it in your browser.  

The next steps will be to set up a workflow to begin separating files, building out the data flow, making things more interactive, and eventually making this use live data and dropping it into a real application.
