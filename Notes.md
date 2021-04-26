### :)
> Why the fuck I need to learn a new list of jargons?

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
        - *Interacting* with the other *lifecycle methods* (and *arguments*)
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
