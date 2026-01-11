## Weekly Assignment Instructions

1. assignment URL #1 - code updates to refactor project
2. assignment URL #2 - diagnose 3-4 failed tests, stretch goal: write a test for a component

### Assignment #1 - Todo App

#### Expected App Capabilities

After completing this week's assignment, your app should:

- be organized by feature and shared components
- use the same label and input on new todo form and edit todo
- allow the user to edit their todo's title

#### Instructions Part 1: Re-organize Project

_While running the Vite development server, these changes will throw errors about non-existent files until all of the import statements are corrected._

![error fail to load file](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/assignments/assets/week-06/does-file-exist.png)

- [x] Create two new directories in `src`: `features` and `shared`
- [x] Inside of `src/features`, create a new directory `TodoList`
- [x] Move `TodoForm.jsx` into `src/features`
  - [x] Update the import statement in `App.jsx`
- [x] Move `TodoList.jsx` and `TodoListItem.jsx` into `src/features/TodoList`
  - [x] Update the import statement in `App.jsx`
- [x] Troubleshoot any remaining import errors before proceeding.
- [x] Add all changes to the stage and then commit to the current working branch.

Committing periodically allows us to set up a safe recovery point that we can get back to without undoing all changes to the working branch.

#### Refactor Input and Label into a Re-usable Component

You will be refactoring out the label and the input from the `TodoForm` so that we can re-use it to allow users to edit existing todos.

- [x] Inside of `src/shared` create a new file, `TextInputWithLabel.jsx`
- [x] Create a component `TextInputWithLabel`
  - [x] Add a label and an input that are wrapped in a React fragment to the return statement. Their props don't matter for now.
  - [x] Add a default export to the bottom of the file.

##### Determining Props

We next have to determine which props we have to pass into the new component. In the TodoForm label and input, you'll see:

for the label:

- `htmlFor`

for the input:

- `type`
- `id`
- `ref`
- `onChange`

We can reduce this list of props by combining `htmlFor` and `id` since they used together to associate a label with its input. We will call the new props `elementId`. We also do not need to pass in `type` since the component will only be for text inputs. This leaves us with `elementId`, `ref`, `onChange`. We will also want to dynamically update our label's text (call it `labelText`) and include the todo's title(call it `value`). With this list of five props identified, we can update the `TextInputWithLabel` component.

- [x] Destructure the props `elementId`, `label`, `onChange`, `ref`, and `value` from the props argument in `TextInputWithLabel`'s function definition.
- [x] Assign the destructured props to the matching label props and input props. You will end up with a component that looks like:

```jsx
{
  /*extract from TextInputWithLabel.jsx*/
}
function TextInputWithLabel({ elementId, label, onChange, ref, value }) {
  return (
    <>
      <label htmlFor={elementId}>{label}</label>
      <input
        type="text"
        id={elementId}
        ref={ref}
        value={value}
        onChange={onChange}
      />
    </>
  );
}

export default TextInputWithLabel;
```

##### Refactor TodoForm to use TextInputWithLabel

- [x] Over in `TodoForm.jsx`, import `TextInputWithLabel`.
- [x] Above the existing label, add an instance of that component.
- [x] Move the `ref`, `value`, and `onChange` props from the input to `TextInputWithLabel`
- [x] Add `elementId` props and set it to the same value as the input's `id`.
- [x] Add a `labelText` props and set it to the string "Todo"
- [x] Delete the existing label and input elements from the form.

The form should behave the same as it did previously but you can see that there's a new component in the component tree:

![TextInputWithLabel in react dev tools](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/assignments/assets/week-06/text-input-with-label-dev-tools.png)

#### Allow Users to Edit Existing Todos

##### Toggle Between Todo Display and Edit

In the `TodoListItem` component:

- [x] Add a state variable `isEditing` and its associated state update function. Use an `initialValue` of `false`;
- [x] Import `TextInputWithLabel` component.
- [x] In the return statement, create a ternary statement that evaluates `isEditing`
  - [x] If true, display an instance of `TextInputWithLabel` with its props `value` set to `todo.title`. We will update all props later. -[x] If false, display the existing form and its contents. The form will contain the checkbox input and the todo title.
- [x] Surround the `{todo.title}` with a span element.
- Add an `onClick` handler to the `span` that toggles the `isEditing` state value to `true`.

Your list item should resemble:

```jsx
{
  /*extract from TodoListItem.jsx*/
}
{
  /*...code*/
}
<li>
  <form>
    {isEditing ? (
      <TextInputWithLabel value={todo.title} />
    ) : (
      <>
        <label>
          <input
            type="checkbox"
            id={`checkbox${todo.id}`}
            checked={todo.isCompleted}
            onChange={() => onCompleteTodo(todo.id)}
          />
        </label>
        <span onClick={() => setIsEditing(true)}>{todo.title}</span>
      </>
    )}
  </form>
</li>;
{
  /*code continues...*/
}
```

At this point, when you click on the todo in the list, it will toggle between being displayed as text and showing in a form input:

![editing a todo](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/assignments/assets/week-06/edit-todo.gif)

- [x] Create a new state variable, `workingTitle` and its associated state update function. Set the `initialValue` to `todo.title`
- [x] Create a `handleCancel` event helper that:
  - [x] resets the `workingTitle` to `todo.title`
  - [x] sets `isEditing` state value to `false`
  - there is no need to include an argument
- [x] Add a button below the `TextInputWithLabel` component:
  - [x] Set its type to "button" to prevent the default behavior of submitting a form.
  - [x] Attach `handleCancel` to an `onClick` props
  - [x] Add the text "Cancel" in the button.

##### Create Local State and Controlled Form

- [x] Create a `handleEdit` event helper that:
  - [x] takes an `event` argument
  - [x] uses the `event.target.value` to update the `workingTitle` state value
- [x] Update `TextInputWithLabel`
  - [x] `value` should now use `workingTitle`
  - [x] add an `onChange` props to `TextInputWithLabel` that takes `handleEdit`

You should now be able to make changes to the todo in the input. We cannot save quite yet though!

![including a cancel button on edit-able todo](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/assignments/assets/week-06/edit-todo-cancel-button.gif)

##### Update Todo State from Local State

- [x] In the `App` component, create an `updateTodo` handler that:
  - [x] takes an `editedTodo` argument.
  - [x] maps through the todos, comparing each `todo.id` with the updated todo's id
    - [x] if it matches - return a new object that destructures the `editedTodo`
    - [x] if not a match - return the current todo.
    - [x] saves the array returned by `map` to `updatedTodos`
  - [x] updates the `todoList` state value with `updatedTodos`.
  - Hint: this function closely resembles the existing `completeTodo` helper function.
- [x] Pass `updateTodo` to a new props in `TodoList` instance and name it `onUpdateTodo`.
- [x] In `TodoList`, destructure that helper out of the component's props then pass it to `TodoListItem` in the same manner.
- [x] In `TodoListItem`:
  - [x] destructure `onUpdateTodo` out of props.
  - [x] Create a helper function, `handleUpdate` that
    - [x] takes event object
    - [x] if `isEditing` is false, return immediately to exit the function
    - [x] calls `event.preventDefault()`
    - [x] calls `onUpdateTodo` and pass in an object that destructures `todo` and sets the `title` equal to `workingTitle`.
    - [x] sets `isEditing` state value to false.
- [x] Add another button below the "Cancel" button:
  - [x] Give it the text "Update"
  - [x] Set its type to "button"
  - [x] add a click handler that takes `handleUpdate`
- [x] Add an `onSubmit` handler to the form element and pass it `handleUpdate`

![adding and completing multiple todos](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/assignments/assets/week-06/add-todos.gif)

#### Closing Notes

At this point, we've created a solid [MVP](https://www.geeksforgeeks.org/minimum-viable-product-mvp/) or "minimum viable product". Next week, we are going to work with Airtable to synchronize the todo list remotely so a user can save todos.

Also, don't forget that this week contains 2 coding assignments. Continue on down below to work with testing!

### Assignment #2 - Diagnose Failed Tests

For the second assignment, you'll fork a repo containing several failing tests. Your task will be to update the code so that the tests pass. The repo can be found here: [Code-the-Dream-School/multicalc-tests: RTL and Vitest testing for Intro to React v3](https://github.com/Code-the-Dream-School/multicalc-tests). The instructions to install the repo and run the tests can be found in the repo's readme file.

#### Submitting the Assignment

_Make sure that both the PR for your todo app and the testing assignment before submitting._

1. Work through the tests, fixing the code so that the tests fail. _Don’t make any changes to the test files!_
2. Once all your tests pass, publish your working branch to GitHub.
3. Open a PR on your own cloned repo, not CTD's.
4. Include the PR link for the tests as the second URL on the assignment submission form.

#### Stretch Goal for Testing (optional)

- Create a new test file for Form.
- Assert that the button is disabled when the operand contains no value. (hint: make sure you use `userEvent` from @testing-library/user-event to empty the field since it has a default value)
