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


This project was bootstrapped with [Create React App](https://github.com/facebookincubator/create-react-app).
