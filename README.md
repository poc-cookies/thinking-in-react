# Thinking in React

## Step 1: Break the UI into a component hierarchy

In order to decide what should be its own component just use the same techniques for deciding if you should create a new function or object.
One such technique is the [single responsibility principle](https://en.wikipedia.org/wiki/Single_responsibility_principle), that is, a component should ideally only do one thing.
If it ends up growing, it should be decomposed into smaller subcomponents.

![App mock](https://facebook.github.io/react/img/blog/thinking-in-react-components.png)

In regard to the current application:
1. **`FilterableProductTable` (orange):** contains the entirety of the example
2. **`SearchBar` (blue):** receives all *user input*
3. **`ProductTable` (green):** displays and filters the *data collection* based on *user input*
4. **`ProductCategoryRow` (turquoise):** displays a heading for each *category*
5. **`ProductRow` (red)**: displays a row for each *product*

Now that we've identified the components in our mock, let's arrange them into a hierarchy.
Components that appear within another component in the mock should appear as a child in the hierarchy:
* `FilterableProductTable`
  * `SearchBar`
  * `ProductTable`
    * `ProductCategoryRow`
    * `ProductRow`


## Step 2: Build a static version in React

The easiest way is to build a version that takes your data model and renders the UI but has no interactivity.

To build a static version of your app that renders your data model, you'll want to build components that reuse other components and pass data using *props*. *props* are a way of passing data from parent to child.
**Don't use state at all** to build this static version. State is reserved only for interactivity, that is, data that changes over time. Since this is a static version of the app, you don't need it.

You can build top-down or bottom-up.
In simpler examples, it's usually easier to go top-down, and on larger projects, it's easier to go bottom-up and write tests as you build.

At the end of this step, you'll have a library of reusable components that render your data model.
The components will only have `render()` methods since this is a static version of your app.


## Step 3: Identify the minimal (but complete) representation of UI state

To build your app correctly, you first need to think of the minimal set of mutable state that your app needs. The key here is DRY: *Don't Repeat Yourself*.
Figure out the absolute minimal representation of the state your application needs and compute everything else you need on-demand.

Think of all of the pieces of data in our example application. We have:
* The original list of products
* The search text the user has entered
* The value of the checkbox
* The filtered list of products

Let's go through each one and figure out which one is state. Simply ask three questions about each piece of data:
1. Is it passed in from a parent via props? If so, it probably isn't state.
2. Does it remain unchanged over time? If so, it probably isn't state.
3. Can you compute it based on any other state or props in your component? If so, it isn't state.

The original list of products is passed in as props, so that's not state.
The search text and the checkbox seem to be state since they change over time and can't be computed from anything.
And finally, the filtered list of products isn't state because it can be computed by combining the original list of products with the search text and value of the checkbox.

So finally, our state is:
* The search text the user has entered
* The value of the checkbox


## Step 4: Identify where your state should live

OK, so we've identified what the minimal set of app state is. Next, we need to identify which component mutates, or *owns*, this state.

Follow these steps to figure out which component should own what state:

For each piece of state in your application:
* Identify every component that renders something based on that state
* Find a common owner component (a single component above all the components that need the state in the hierarchy)
* Either the common owner or another component higher up in the hierarchy should own the state
* If you can't find a component where it makes sense to own the state, create a new component simply for holding the state and add it somewhere in the hierarchy above the common owner component

Let's run through this strategy for our application:
* `ProductTable` needs to filter the product list based on state and `SearchBar` needs to display the search text and checked state
* The common owner component is `FilterableProductTable`
* It conceptually makes sense for the filter text and checked value to live in `FilterableProductTable`

So we've decided that our state lives in `FilterableProductTable`.
First, add an instance property `this.state = {filterText: '', inStockOnly: false}` to `FilterableProductTable`'s `constructor` to reflect the initial state of your application.
Then, pass `filterText` and `inStockOnly` to `ProductTable` and `SearchBar` as a prop.
Finally, use these props to filter the rows in `ProductTable` and set the values of the form fields in `SearchBar`.

This project was bootstrapped with [Create React App](https://github.com/facebookincubator/create-react-app).
