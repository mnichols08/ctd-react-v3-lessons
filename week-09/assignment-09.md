Closes #17 

## Weekly Assignment Instructions

### Expected App Capabilities

After completing this week's assignment, your app should:

- Use `useCallback` for URL string encoding.
- Pause API requests while the user is typing

### useCallback and encodeUrl

- [x] Import `useCallback` into App.jsx.
- [x] Create a variable `encodeUrl` (same name as helper function created last week) inside the App component and assign it an empty `useCallback`.
- [x] Define an empty arrow function that takes no arguments in the `useCallback`. It should now look like:

```jsx
{/*extract from App.jsx*/}
{/*...code*/}
const encodeUrl = useCallback(()=>{},[])
{/*code continues...*/}
```

- [x] Move the body of the `encodeUrl` utility function to this empty arrow function.
- [x] Delete the utility function from the top of the file.
-  If you're using ESLint, it should be giving you the following warning about `useCallback` dependencies:

![missing dependencies shown in eslint](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/assignments/assets/week-09/missing-deps.png)

- [x] Add these dependencies to the `useCallback` dependency array.
- [x] Search your App.jsx for all instances of `encodeUrl` then remove the arguments wherever the function is called. All calls to `encodeUrl` should look like: `encodeUrl()` since the `useCallback` handles all the dependencies now.

### Debouncing Filter Input

Sending a network request for every character typed can use up API and network resources.

![each keypress sends a network request](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/assignments/assets/week-09/undebounced.gif)

Earlier the lesson, we covered throttling as a means to control how rapid an event fires. Another approach to controlling network requests, especially well-suited for keyboard events is debouncing. This prevents any events from being processed until they go below a certain rate. In the case of the todo list, you'll prevent keyboard events from updating the query string until they stop typing for a half a second (500ms).

<details>
<summary>More about the term "debounce"</summary>
<p>This term "debounce" originally comes from an electrical engineering solution that deals with noise that gets introduced in a circuit when a mechanical switch closes. No matter how carefully two conductive surfaces come into contact (in a button, switch, dial, etc), there is a little bounciness as they come together. That bounce opens and closes the connection rapidly causing bad information to get transmitted.</p>
<p>The solution to this is to wait for the new circuit to stop "bouncing" and settles into an open or closed state before acting on a change. This is same thing that happens when you drop a basket ball onto cement - it'll bounce repeatedly, a little bit less each time, until it stops on the ground. It just happens on a different time and size scale.</p>
</details>

The best way to balance UI performance and keeping API calls down is to prevent any fetch from happening until a certain time has passed. We want it to pause long enough that our app doesn't send out a request for every keystroke but not force the user to wait a long period of time. With deeper research you may come up with a delay length, but 500ms is a good starting point.

#### Putting Debounce into Action with useEffect

You'll combine the use of a useEffect and a setTimeout with a 500ms delay. Remember that a useEffect's return value is used to clean up after the previous useEffect as the component re-renders. To take advantage of React's re-render process you can use the cleanup function to delete the previous timeout each time there's a change. When a user pauses typing long enough, the last called setTimeout will finally get a chance to execute it's callback function. That callback function will pass the locally managed state back up to the App component which finally kicks off a fetch request with updated query params.

In TodosViewForm.jsx:

- [x] Define a local state for the search input and set its `defaultValue` to `queryString`: `const [localQueryString, setLocalQueryString] = useState(queryString);`
- [x] Refactor the search input and the Clear button to use the local state instead of the `queryString` and `setQueryString,` from App.
- [x] Create a useEffect
  - [x] Add `localQueryString` and `setQueryString` to the dependency array since you'll be working with them.
  - [x] Call `setTimeout` and assign it to a constant, `debounce`.
    - [x] In the body of `setTimeout`'s callback, call `setQueryString(localQueryString)`
    - [x] Give it a delay of 500ms.
  - [x] In `useEffect`'s return statement, add an anonymous function that calls `clearTimeout` that takes in `debounce`.

![keystrokes debounced to 500 milliseconds](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/assignments/assets/week-09/debounced.gif)

### Closing Notes

Next week we'll talk about styling and using 2 prominent React styling libraries: CSS Modules and Styled Components. That will give us a chance to brighten up a bland interface and give it character!

### Mindset Assignment
Problem Solving

Though, at times, a solution may come together quickly first try, it more often than not takes some determination and thoughtful problem solving to see a new challenge over the finish line. (Pro tip: Those also end up being the truly satisfying wins!) But have you ever looked at a new problem and thought “I have no idea how to even get started!” …

Fear not! Problem solving is a learned skill too, and if you develop a solid process—ex. understanding the problem, planning, dividing the problem into subproblems—even the trickiest of challenges can be chipped away at.

This FreeCodeCamp article does a nice job of defining a problem solving process. Your preferred process may not end up mirroring this one exactly, but use this as inspiration to develop your own approach.
Please answer the below prompts in your assignment submission:

- Q: Do you already have a problem solving process in place that you’ve found effective? If so, tell us about it!
- A: My best problem solving approach that comes to mind is a process of elimination mixed with trial and error. I do not mind when things break, in fact I learn a lot from errors. When I figure out what does not work, I often come up with alternative solutions.
- Q: How does problem solving show up through other hobbies or activities in your life?
- A: I enjoy electronics and wiring and I leared a bunch over the summer about pinning, and more importantly, depenning wiring harnesses. I also got to dabble with capacitors, resistors, fuses, and relays to build a unique project inside of my car. Using a Raspberry Pi and a lot of components, came up with a way to accept 12v inputs from my vehicle, and convert them into 3v for the Pi to be able to read and respond appropriately. For example, if my Check Engine Light flashes, the screen flashes "Warning! Danger to Manifold!" in a Fast and the Furious Pun.
- Q: Have you observed others in the class solving problems differently than you do? What have you learned from their problem solving techniques?
- A: I don't get to see other students very often so it's hard to say. However I do recall in my college years that some of the students with the best grades would always take efficent notes and ask questions. I try and do the same especially when taking a class seriously