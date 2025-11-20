## Weekly Assignment Instructions

### Expected App Capabilities

After completing this week's assignment, your app should:

- [x] Contain a TodoList component that contains all todo code
- [x] Contain a TodoForm component with
  - [x] a non-functioning form with 1 input field and a submit button
- [x] Display a h1 heading, a todo entry form, and the todos we added last week

### Instructions Part 0: Pre-Work Version Control Tasks

_Before proceeding, make sure your week-01 lesson has been approved._

- [ ] Merge your PR in GitHub
- [ ] On your local environment, check out `main` and then pull down the latest changes:

```terminal
git checkout main
git pull
```

- [x] Create and checkout a working branch for this week: `git checkout -b week-02-components`
- [x] Publish the branch to GitHub: `git push origin week-02-components`
- [x] Start up your development server and open the app in your browser.

### Instructions Part 1: TodoList Component

- [x] Create a new file, `TodoList.jsx` inside the `src` directory
- [x] Scaffold a component inside that file with the name `TodoList`. This includes:
  - the function definition
  - an empty return statement
  - export default TodoList at the bottom of the file

At this point, the TodoList component should look like:

```jsx
{
  /*extract from TodoList.jsx*/
}
function TodoList() {
  return;
}

export default TodoList;
```

- [x] In the `App` component, import `TodoList`
- [x] Below the heading tag in the JSX, add an instance of the `TodoList`
- [x] Move the `todos` array over to `TodoList`
- [x] Observe the errors that appear in the browser console. You'll see something like:

![screen capture of ReferenceError in browser console](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/assignments/assets/week-02/reference-error.png)

Errors like list are common during development and help us out quite a bit. Make sure you understand what the "Uncaught ReferenceError" message means and where it comes from. You may not know what an "error boundary" in the second message. Take an opportunity to do some research into what that message may mean. Our next step will resolve this.

- Move the unordered list and the `todos.map...` to the return statement in `TodoList`. Remember that parens `()` can be used for multi-line return statements.
- Refresh the browser page and those error messages should be gone. The page should also look the same as it did before.

### Instructions Part 2: TodoForm Component

- [x] Create a new file, `TodoForm.jsx` in the `src` directory.
- [x] Create a component with the same name in that file.
- [x] Add an empty form to `TodoForm`'s return statement.
- In the form add
  - [x] a label with an `htmlFor` props set to `todoTitle`.
  - [x] between the label tags, insert the text, "Todo"
  - [x] a text input with an id of `todoTitle`
  - [x] a button with the text "Add Todo"
- [x] Import the `TodoForm` into `App`
- [x] Place an instance of `TodoForm` between the heading and `TodoList`

The app should now look like:

![screen capture of todo list with new todo form in the browser](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/assignments/assets/week-02/todo-list-with-form.png)

### Instructions Part 3: Final Steps

#### Version Control Closeout Tasks

- [x] Commit changes to your local working branch.
- [x] Push the changes up to GitHub.
- [x] In GitHub, create a PR (pull request) that compares the working branch to `main`.
- [x] Copy the PR link and submit assignment.

### Closing Notes

After this week, the instructions will start assuming some knowledge of the topics and tasks covered the previous weeks. As a result, fewer code examples will be given in instructions. You will also be responsible for remembering all basic version control tasks - merging, pulling, committing, publishing to GH, etc.

As a React student, it is important to gain practice in working through some tasks on your own and being able to reach out for assistance when you get stuck. We aim to make this course challenging but it won't be impossible either! Any areas that are particularly challenging or confusing will include tips or hints that can help out.

#### Assignment Mindset Questionaire

- **Q:** Have you ever used any AI tools (ex. chatGPT, Grammerly, Dall-E, etc.)? If yes, what did you use it for? If no, did you choose not to use AI, or you just haven't explored it/aren't aware of how to use it?
- **A:** I have used AI for a lot of different tasks over the past few years. I have used it to help improve my writing for blog posts. I also use it to brainstorm ideas to write about. I very often use Perplexity at work to help cross reference part numbers (I sell auto parts for a living). I started off heavily against it but eventually I found it to be incredibly useful. Sometimes I use it a bit too much, but I feel like it is a tool we have to learn to live with.

- **Q:** What did you think about your interaction with AI?
- **A:** As mentioned above, I was against it at first. But I was also against using React at first. I always focused on learning the fundamentals first. However I wish I had jumped on the AI bandwagon a little sooner. But overall I enjoy it. However I do long for the days before AI since there is now so much of it used everwhere it is hard to find the line between real and artificial.

- **Q:** What are ways you think AI can help you as you learn content throughout this class?
- **A:** I think that this course or all courses for that matter benefit from being able to ask AI to elaborate a little bit more, and to create study plans as necessary. 

- **Q:** What are ways that AI can cause frustration or negatively impact your learning?
- **A:** AI can make mistakes and often leads to bugs in software if it's overused (for example, vibe coding), but it can also give false information steering us wrong and leading to more confusion than necessary. 
