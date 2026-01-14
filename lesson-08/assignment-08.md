## Weekly Assignment Instructions

### Expected App Capabilities

After completing this week's assignment, your app should:

- [ ] Use the API to sort todos by `title` or `createdTime`
- [ ] Use the API to search for todos based on title contents

### Instructions Part 1: Using Airtable's Query Parameters

#### Setup

- [x] Add field, `createdTime` for default sort to your Airbase table. This will end up being the default sort field. We are not be able to use the already existing `createdTime` on the `record` object because Airtable functions only work with `fields` properties which are those defined by the user in the table. Use the following options in the field creator:
- [x] Name the field "createdTime".
- [x] Set it to "Date".
- [x] Use the same timezones for everyone and set the timezone to GMT/UTC.
- [x] Set a default value of "Current date".

![configure createTime field](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/assignments/assets/week-08/created-time-config.png)

In App.jsx

#### Examine the APIs Query Parameters

You'll be using Airtable's query parameters to set the sort and filter options. Since we want to set up a default sort order we'll look at the Airtable's `sort`

- [x] Navigate to your table's help documents and select "List records" from the left-hand side navigation. If you don't recall the process to get to that, here's another way you can select your base to get customized documentation: [Introduction - Airtable Web API](https://airtable.com/developers/web/api/introduction)
- [x] Scroll down to `sort` and you'll be able to read about how to work with the `sort` URL query parameter.
  - [x] This will give you some sense about how Airtable's parameters get turned into these ugly URL strings:

```text
sort%5B0%5D%5Bfield%5D=title
sort%5B0%5D%5Bdirection%5D=desc

https://api.airtable.com/v0/airtable_table_id/Todos?sort%5B0%5D%5Bfield%5D=createdTime&sort%5B0%5D%5Bdirection%5D=desc
```

Each one of those character combinations that start with a "%" sign - as in `%5B`, `%5D`, and so on, are characters that have been re-encoded to be URL safe. To better read the string contents, you'll have to rely on a URI decoder or looking up the characters one at a time. Coincidentally, JavaScript comes with a build-in decoder: `decodeUri()` that you can run that in the browser console to see the original string:

```console
decodeURI('https://api.airtable.com/v0/airtable_table_id/Todos?sort%5B0%5D%5Bfield%5D=createdTime&sort%5B0%5D%5Bdirection%5D=desc')

"https://api.airtable.com/v0/airtable_table_id/Todos?sort[0][field]=createdTime&sort[0][direction]=desc"
```

From the decoded string, we can tell that the Airtable API takes string representations of the configuration that Airtable uses to process the request. We don't fully know how the API works in reality, but we can liken this string to setting 2 properties on an object using bracket notation, one at a time. Remember that APIs differ from each other, so other APIs may not follow this same approach. This is why developers rely so heavily on documentation to assist with their work.

2 config params also means we need to represent those values in state so a user can toggle them.

- Still in App, create new state variables for the params with associated update functions:
  - [x] `sortField`: initial value of "createdTime"
  - [x] `sortDirection`: initial value of "desc" as in descending

#### URL Encoding Utility Function

You already employ the `url` in four locations: the `useEffect`, `addTodo`, `updateTodo`, and `completeTodo`. All fetches need to use the same query parameters so that the todo list doesn't spontaneously re-order itself or show all items in a filtered view. You'll centralize the logic in a utility function that exists outside of the component.

- [x] Define a function `encodeUrl` above the `App` function definition:
  - [x] It takes an object that includes the properties: `sortField, sortDirection`.
  - - Inside the function, define a template literal that combines the 2 sort query parameters:
    - [x] ``let sortQuery = `sort[0][field]=${sortField}&sort[0][direction]=${sortDirection}`;``
    - [x] This compares to the console output above, but `createdTime` is replaced with a `${sortField}` and `desc` is replaced with `${sortDirection}`.
  - [x] It returns a method call to `encodeURI` that takes in a template literal that appends the query parameters to `url`. The function should resemble:

```jsx
{
  /*extract from App.jsx*/
}
{
  /*...code*/
}

const encodeUrl = ({ sortField, sortDirection }) => {
  let sortQuery = `sort[0][field]=${sortField}&sort[0][direction]=${sortDirection}`;
  return encodeURI(`${url}?${sortQuery}`);
};
{
  /*code continues...*/
}
```

- [x] Replace all references to `url` in App with `encodeUrl()`.
  - [x] At each location, pass in an object containing the state variables `sortDirection` and `sortField`.

If you are using ESLint, you should see a warning appear on your `useEffect:`

![missing sortDirection and sortField dependencies](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/assignments/assets/week-08/missing-deps.png)

- [x] Update the `useEffect`'s dependency array to include the state values, `sortDirection` and `sortField`. The `useEffect will now fire` each time a user sets the `sortDirection` and `sortField`.

### Instructions Part 2: View Control Forms

You now need to provide the user with a way to update the params.

- [x] Define a new component, `TodosViewForm` on the `src/features` directory.
- [x] Return a form that contains a div that will contain the form controls.
  - [x] Add a `label` containing the text "Sort by".
  - [x] Add a `select` element that is associated with that label
  - Give the `select` element 2 `option` elements:
    - [x] 1st: value="title" and includes the text, "Title"
    - [x] 2nd: value="createdTime and includes the text "Time added"
  - [x] Add another `label` containing "Direction" just after the previous label and select.
  - [x] Add another `select` with `option` elements for:
    - [x] "Ascending" using "asc" for the value
    - [x] "Descending" using "desc" for the value
- [x] Default export the component.
- [x] In `App`, import it and place a horizontal rule between TodoList instance and the div containing the error message display.
- [x] Below the `hr`, add an instance of `TodosViewForm`. The `hr` and `form` will show below the todos now:

![sort form on bottom of todos](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/assignments/assets/week-08/todos-hr-sort-form.png)

- [x] Into `TodosViewForm` instance, pass in the props `sortDirection`, `setSortDirection`, `sortField`, and `setSortField`.

Back over in `TodosViewForm` you'll wire up the form so that each time the user updates the options, the state updates will cause the `useEffect` to automatically fetch from Airtable.

- [x] Destructure `sortDirection`, `setSortDirection`, `sortField`, and `setSortField` out of the component's props.
- On `select`element for the sort by:
  - [x] Add an `onChange`handler:
    - Its callback is an anonymous function that:
      - [x] Takes the event object
      - [x] Calls `setSortField` with the event target's value.
  - [x] Add a `value` props and assign it to `sortField`.
  - [x] Ensure that each `option`'s value matches the target field names.
- On the `select` element for the sort direction:
  - Add an `onChange` handler that uses an anonymous function that:
    - [x] Takes the event object
    - [x] Calls `setSortDirection` with the event target's value.
- [x] Define a function `preventRefresh` whose only job is to prevent the page from refreshing if a user accidentally hits enter while working with this form:
- [x] pass it to an `onSubmit` props on the `form` element.

At this point, the form should allow you to select either "Title" or "Time Added" and the API will automatically return the sorted todos and update the UI.

![Time Added can be sorted ascending or descending](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/assignments/assets/week-08/time-sort.png)

### Instructions Part 3: Search Todo Titles

In this part, you'll add a search field an button to the form that was just created. It will take in a string and then added the url params before it's encoded.

#### Update Utility Function

- [x] In `App`, create the state value (and update function) for `queryString` with an empty string for an initial value.
- Update `encodeUrl` utility function:
  - [x] Add `queryString` to the argument object.
  - [x] Create an updatable variable (`let`) `searchQuery` set to an empty string.
  - [x]Add `${searchQuery}` to the end of the template literal that is used in the return value.
  - [x] Above the return statement, use an `if` block to assign a value to `searchQuery` is truthy.
    - [x] If true, update `searchQuery` in the block: ``searchQuery = `&filterByFormula=SEARCH("${queryString}",+title)`;``
- [x] Update each `encodeUrl` call in App to include `queryString` in its params object. Each of the 3 calls should now resemble: `encodeUrl({ sortDirection, sortField, queryString })`
- [x] Update the `useEffects` dependency array by adding `queryString`.

#### Update Form

- [x]In `App`, to the `TodosViewForm` instance, add props `queryString` and `setQueryString`,
- In `TodosViewForm`:
  - [x] Destructure these props out.
  - [x] Add a `div` above the previous one and include:
  - [x] A label containing the text "Search todos:"
  - An input with props:
  - [x] `type="text"`
  - [x] `value={queryString}`
  - [x] `onChange((e)=> {setQueryString(e.target.value)`
  - A button:
  - [x] Set to the type of button.
  - [x] Contains the text "Clear".
  - [x] And empties `queryString` whenever it's clicked.

Your user is now able to filter the todos based on an input that they then can also reset.

![demo of todo list functionality](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/assignments/assets/week-08/search-demo.gif)

### Closing Notes

This week you added in functionality to sort the todos by title or its creation date and filter the titles to match a string input. To minimize code edits across several fetches, you extracted the logic to build the URL string into a utility function. Also, as a result of how form state is managed, all updates happen live! Each time a user types or selects an option, the `useEffect` initiates a fetch using the updated url params. This produces so pretty snappy results but can eventually become a burden on the network and API services as the application grows or a user's todo's start growing in number. Next week' we will look at a few ways limit network traffic and perform some other optimizations to the codebase to keep the interface running smoothly.

### Mindset Assignment

#### Information Literacy

Have you ever gotten advice from someone, that turned out to be wrong, perhaps bad advice or just not applicable to you? The internet is a powerful tool to provide information to the masses, but there isn’t anything in place that regulates whether that information is accurate, and it isn’t always immediately apparent when information is or is not applicable to what you’re experiencing. As developers you’ll rely on the internet a lot, but that doesn’t mean all the answers you find will be correct, the best solution, or helpful.

Information literacy is the ability to identify, find, evaluate, and use information effectively. In technology, this often translates not only to assessing the accuracy of answers but also how to find a solution that aligns with your project specifications (ex. Did you find an article with a solution to your problem, but the solution is for someone using a different version of Ruby or NPM?) The specs may not always be a perfect match, but could still shed light on a shared root cause.

Some helpful tools that equip you to have stronger information literacy are:

ALWAYS check date of article or posting, especially with StackOverflow - versions and problems rapidly change in the world of tech. The first thing you should always verify is the WHEN, when was this information posted.
Know your own specifications (what tools + versions you’re using) so you can be critical about the answers you read. Is this information “from the source” (official developer documentation) or second hand (a tutorial or blog post)? The latter may warrant some additional skepticism—when in doubt start with the source, and then look to secondary resources to supplement. Is this a site you trust and has helped you before reliably? Navigating information sources takes time to get the hang of, so incorporate some of the tools we’ve covered earlier regarding asking for help, debugging, being curious and willing to try new things, and being ok with not knowing the answer at first!
Lastly, referring to Documentation (directly from whatever technology you are using) should always be your go to when applicable. Documentation will always provide you accurate information.
Please answer the below prompts in your assignment submission:

- Q: When was a time you got misinformation or bad advice? What happened? Also consider any time an artificial intelligence tool has given you bad/weird answers. Share about that experience here.
- Q: What are some ‘clues’ that you use to help you assess whether a resource is applicable/trustworthy or not? What are some ways you can verify the information you got from AI was accurate/current?
- Q: What are some ways you’ll build on your information literacy either in your personal life or as a developer?