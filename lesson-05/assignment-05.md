## Weekly Assignment Instructions

## Expected App Capabilities

After completing this week's assignment, your app should:

- [x] conditionally render a message when the todo list is empty
- [x] disable the Add Todo button when the input is empty
- [x] allow users to complete a todo
- [x] utilize a controlled form

## Instructions Part 1: Conditional Rendering a Message

In TodoList.jsx:

- [x] create a [ternary statement](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Conditional_operator) that compares the todoList's length to zero.
  - [x] if `true`, render a paragraph tag that includes the text "Add todo above to get started"
  - [x] if `false`, render the unordered list includes the mapped todo list.

Your app should show the paragraph if the list is empty or the todos if the `todoList` contains any.

![hitting enter adds todo](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/assignments/assets/week-05/enter-todo.gif)

## Instructions Part 2: Mark Todo as Complete

Rather than deleting a todo item outright, we want to indicate it has been completed. We'll update the schema that the todos use and create a helper function that toggles the value when the user clicks a checkbox on the todo.

- [x] In `App`, update `newTodo` in the `addTodo` event handler with a new property called `isCompleted` and set it to `false`. Each todo should now consist of 3 properties: `title`, `id`, and `isCompleted`.
- [x] Create a helper function `completeTodo`. It:
  - [x] takes an `id`
  - [x] maps through the `todoList` and:
  - [x] if the current `todo.id` matches the `id`, return a new object that destructures the current todo and `isCompleted` set to `true`
  - [x] otherwise (if `todo.id` does not match the `id`) return the `todo`
  - [x] saves the resulting array to a const `updatedTodos`
  - [x] update the `todoList` state with `updatedTodos`

the function will use logic very similar to the example below:

```js
function favoriteColor(colorId) {
    const updatedColors = colors.map((color) => {
        if (color.id === colorId) {
        return { ...color, isFavorite: true };
    }
        return color;
    });
    //update `colors` state with `updatedColors`
}
```

- [x] pass your new helper function to a `TodoList` props named `onCompleteTodo`
- [x] in `TodoList`, destructure that out of the component's props.
- [x] add the same props to the `TodoListItem` instance and pass in that helper.
- in `TodoListItem`
  - [x] destructure the function out of props
  - [x] create a form element in the list item and move `{todo.title}` into it.
  - [x] above that, create an input
    - [x] give it the type of "checkbox"
    - [x] add the prop `checked` and set it equal to `todo.isCompleted`
    - [x] add an `onChange` event listener and then invoke an anonymous function that returns the helper function with the `todo` object's id.

> [!remember]
> You do not want to call the helper function directly in an event listener props. If you do this, the function will immediately fire off when the component renders, causing bugs!

To assist with the structure of the components elements, follow this layout:

```jsx
{/*extract from TodoListItem.jsx*/}
function TodoListItem({ todo, onCompleteTodo }) {
  return (
    <li>
      <form>
        <input
          {/* set the `type` prop to "checkbox" */}
          {/* add the `checked` props */}
          {/* add `onChange` event listener that uses the `onCompleteTodo` helper` */}
        />
        {todo.title}
      </form>
    </li>
  );
}

export default TodoListItem;
```

Each todo should now have a checkbox. When the checkbox is clicked, the helper function will set `isCompleted` to `true`. Our final step is to filter out the completed todos.

In `TodoList`, we need to filter out the completed todos before we map the todos to `TodoListItem` instances.

- [x] create a constant `filteredTodoList` that contains the `todoList` that has been filtered to remove any todo whose `.isCompleted` property is `true`
- [x] replace the `todoList` references in the JSX with the `filteredTodoList`

Now, when a user clicks on the checkbox beside an item, that todo disappears from the list.

![submit and complete a todo](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/assignments/assets/week-05/add-complete-todo.gif)

## Instructions Part 3: Convert Form to Controlled Component

### Create Local State

- [x] import `useState` into `TodoForm`
- [x] create a `workingTodoTitle` state variable with the accompanying state update function.
- [x] give the `useState` an initial value of an empty string.

### Connect Form Input to Component State

- [x] to the `input` element in the form, add a `value` props set to `workingTodoTitle`.
- [x] add an `onChange` listener to the `input`.
  - [x] it should call an anonymous arrow function that takes in the event object.
  - [x] the anonymous function should call `setWorkingTodo` and pass in the event's target value.

### Update `handleAddTodo`

- [x] remove the lines that access the title's value from the event object
  - hint: there's one that retrieves the value and one that sets the target's title `value` back to `""`
- [x] update `onAddTodo` so that it gets passed `workingTodoTitle` instead of `title`.
- [x] after `onAddTodo`, call the `setWorkingTodo` state update function with an empty string to reset the form input.

## Instructions Part 4: Disable Form Button when Input is Empty

We are pretty close to being complete for this week but there is one minor problem with the interface. A user is able to add empty todos to their list. Since the form was converted to a controlled component, we can do so based on the value of the working state in the component.

![user is able to hit add todo even when input is empty](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/assignments/assets/week-05/submit-enabled.gif)

- [x] In `TodoForm`'s button element, add a `disabled` prop that evaluates to true when `workingTodo` is an empty string.

![submission button disabled when input is empty](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/assignments/assets/week-05/submit-disabled.gif)

## Mindset Responses
- Q: So far in class, have you had any “aha” moments? What have you enjoyed the most? What has been the hardest?
- A: I have not really had many aha moments yet in this class. Most of this has been a refresher so far for me. The most enjoyable part of the class was setting up the structure for the project and tinkering with submodules (outside of the curriculum, but a part of my repository). The hardest part has been finding time to join a mentor session.
-Q: What were you excited/worried about before class started?
- A: I was pretty worried that I would struggle to find time for this course and it has proven to be extra difficult with my little ones arrival just before class started. Finding time to meet with any mentors is difficult due to a full time job and learning to be a father at home outside of work
- Q: How do you feel about what’s still to come in this class and in your journey ahead?
- A: I need to buckle down and figure out a proper time to meet with a mentor in a 1:1 or group setting. I am confident I will be able to keep up with the material but I keep procastinating the assignment too much lately.