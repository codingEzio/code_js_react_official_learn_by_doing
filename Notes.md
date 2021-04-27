### :)
> Why the fuck I need to learn a new list of jargons?




### Naming
- these don't matter
    ```javascript
    this.state.KEY_BE_ANYTHING
    this.props.KEY_BE_ANYTHING
    ```
- these *should* follow the convention
    ```javascript
    // parent - passing down
    handleClick
    handle[EVENT]

    // child  - receiving props
    onClick
    on[EVENT]
    ```

### `React.Component`
> context: `.. render() { return ( <JSX_STUFF> ) }`
1. Building blocks of a *React* app
    - There are many [*other types*](https://levelup.gitconnected.com/types-of-react-components-a38ce18e35ab) of *Component*s
    - You write small fractions of *HTML*, then putting them together.
2. What can you do with it
    ```jsx
    class TodoList extends React.Component {
        render() {
            return (
                <div className="todo-list">
                    <h1>TODOs for {this.props.name}</h1>
                    <ul>
                        ...
                        ...
                    </ul>
                </div>
            )
        }
    }

    <TodoList name="Oliver" />
    ```
3. Dissectioning (assumption + [babeljs](https://babeljs.io/repl))
    - JSX are *merely* bunch of `createElement` with args
        - [`createElement` in DOM](https://developer.mozilla.org/en-US/docs/Web/API/Document/createElement): use code to generate HTMl code
        - [`createElement` in React](https://reactjs.org/docs/react-api.html#createelement): same thing but more sophisticated
    - `class .. extends ..`
        - *Give* functionality to this *small sections of code*
            - reuse or being reused by other *small sections of code*
            - ability to *manipulate whatever you want*
                - *It* empowers the *JSX* inside
                - *JSX* inside *use* stuff outside (*lifecycle* methods + our own)
    - `render() {}`
        - *Interacting* with the other [*lifecycle methods*](https://programmingwithmosh.com/javascript/react-lifecycle-methods/) (and *arguments*)
        - This by itself returns a *React element* (HTML, are you there?)




### Explanation with each commit 0x01
- `this.props`
    - my own words
        > mere an *object*, assign/access via `.` (e.g. `this.props.name`)
    - official jargon
        > Passing data from *parents* to *children* (component)
- events (`onClick` in this case)
    - naming convention (I assume): `onEVENT={..}`
    - either use the *full version* or *arrow function*
        > `function() { alert('ok') }`

### Explantion with each commit 0x02
- *Remembering* things
    ```jsx
    constructor(props) {
        super(props);

        this.state = {
            val: null,
        };
    }

    // only changed lines were listed here :)
    .. onClick={ () => this.setState({ val: 'X' }) }
    .. {this.state.val}
    ```
- Dissectioning (assumption + what the [*tutorial*](https://reactjs.org/tutorial/tutorial.html) says)
    > Nothing special, *JavaScript thing* (plus React's own concerns)
    - `constructor(..)` and `super(..)`
        - initializatin in ES6's `class` (*not* React's thing)
    - `this.state = { DICT }`
        - a private dict for storing *to-be-updated* values
    - `this.setState()`
        - line change: now we *update* it to make it *dynamic*
        - code syntax & the [*concerns*](https://stackoverflow.com/a/53098946) we've mentioned
            ```javascript
            // React need something to detect changes (in order to re-render)

            // do changes, but React cannot detect it
            this.state.color = 'red';

            // do changes, and React can detect it
            // why? because method `render()` was called
            this.setState({color: 'red'});
            ```
    - `this.state.val`
        - another normal JavaScript objects!
- Dissectioning (assumption)
    - two changes to `Square`
        - previously we let `Square` handle it by itself
            - `setState` and `this.state.OBJ_KEY`
        - now we pass values down from `Board` to `Square`
            - `this.props.ACTION()` and `this.props.OBJ_KEY`
            - and the `constructor` was *removed* from `Square`
    - on passing *ACTIONS* down to *child component*
        > Nothing different from simple *values*, only a bit more complicated
        1. a `Square` is clicked
        2. `onClick` inside the `Board` wasn't called at this point (its *existence* propels step3)
        3. *Component itself* <small>(`Board`)</small> tells React to *set up a event listener* (<small>to `Square` I assume</small>)
        4. `onClick` inside `Square` was called (immediately go back to `Board`)
        5. `this.handleClick(i)` inside `Board` was called
    - on `const .. = .. slice()` inside `Board`s `handleClick()`
        > each time we create a copy then update relevant value
        1. immutability is easier
            > *copy* is pretty *cheap* I assume?
            - no need to look at multiple places to see *who* changed *this variable*
            - cool features like *undo* and *redo* ("time travel" in our case)
        2. detecting changes
            - no need to compare each items one by one
            - only a `===` is enough (built-in `===` is definitely faster than our own implementation)
        3. when to re-render
            - combine with *function being called* like `setState`
    - some *jargons*
        - `Square`s are now called *controlled components* (fully, by `Board`)
