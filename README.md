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

This project was bootstrapped with [Create React App](https://github.com/facebookincubator/create-react-app).
