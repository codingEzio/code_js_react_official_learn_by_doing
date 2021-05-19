### :)
> Why the fuck do I need to learn a new list of jargons <small?>(***that's why we have this note***)</small>?




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
- *Functional Component*
    - WHAT
        > HTML with *superpowers* <small>(JSX)</small>, *with* exterval values and actions, *without* lifecycle methods
    - WHEN
        > It doesn't have any internal state to manage (or done by *parent* instead)
    - WHY
        > Easy to write and understand
    - HOW
        ```jsx
        function COMP_NAME(props) {
            return (
                // JSX Code
                //  data  : {props.OBJ_KEY}
                //  action: {props.ACTION}  (no parentheses!)
            )
        }
        ```

### Explantion with each commit 0x03
- Now `X` and `O` could take turns
    - functionally
        1. initialize a boolean variable in `this.state`
        2. do various things then run `this.setState` to update it
    - visually
        - access via `this.state.OBJ_KEY`

### Explanation with each commit 0x04
- What to do now
    - determine who's the winner
    - stop showing *who's next* when the game has already finished
    - stop accepting *button clicking* when the square is already filled or someone has won
- task1: winner
    1. what counts as winning
        > three consecutive lineup (horizontal, vertial and diagonal)
    2. who's on the lineup
        ```bash
        # e.g. X
        #   pos_a, pos_b, pos_c = winning_position
        #   sqr[pos_a]                  one 'X'
        #   sqr[pos_a] && sqr[pos_b]    two 'X' matching
        #   sqr[pos_a] && sqr[pos_c]    all three matched
        ```
    3. implement this feature
        - do it outside the lifecycle methods to make the component cleaner
        - then call it in method `render()` each time when `Board` is changed
- task2: status text
    - only a value that determine *who has won* is needed
        - initialize a variable called `status`
        - change its text based on the return value of `calculateWinner`
            > it doesn't have to be a boolean, enough for qualifying for `if (COND)`
- task3: button clicking
    - intuitively we need modify this in the `onClick` <small>(`handleClick`)</small>
        ```javascript
        if (
            calculateWinner(squares) // there HAS been a winner
            squares[i]               // OR this place has "taken"
        )
        ```

### Explanation with each commit 0x05
- What we'll do
    - Implementing the *time machine* feature (partially)
    - Lifting states *up*, again
        - previously: from `Square` to `Board`
        - things we'll do: from `Board` to the `Game` (*root*)
- Structural overview
    1. move the `constructor` up to `Game`
        ```javascript
        // 1. move from `Board` to `Game`
        // 2. create a nested array for the "time machine" feature
        this.state = {
            // BEFORE
            // > this.state.squares
            squares: Array(9).fill(null),

            // AFTER
            // > this.state.history[N].squares
            history: [{ squares: Array(9).fill(null) }],
        }
        ```
    2. move the *who's the winner* in the **`render` method** up to `Game`
        ```javascript
        // 1. move the JavaScript code from `Board` to `Game`
        // 2. move the HTML       code from `Board` to `Game`
        // 3. add two lines specifically caused by 'time machine feature'

        // used for the 'time machine feature'
        const history = this.state.history;          // local state, nested
        const current = history[history.length - 1]; // pick "the most recent"

        // BEFORE: <div className="status">       { status }    </div>
        // AFTER : <div className="game-info>  .. { status } .. </div>

        // these four lines of JavaScript are exactly the same
        const winner = calculateWinner();
        let status;
        if (winner) { ... }
        else        { ... }
        ```
    3. move the `handleClick` up to `Game`
        ```javascript
        // 1. accessing `squares`
        // 2. stopping Board changes from invalid conditions
        // 3. update relevant state

        // used for the 'time machine feature' (same as above in `render`)
        const history = this.state.history;          // local state, nested
        const current = history[history.length - 1]; // pick "the most recent"
        // make an copy before modifying, as usual
        const squares = current.squares.slice();

        // the part in the middle is exactly the same

        // the essential part for 'time travel' (saving board state)
        // expected structure of `history`: [ {s: s1}, {s: s2}, {s: s3} .. ]
        this.setState({
            history: history.concat( [ {squares: squares} ] ),
            xIsNext: !this.state.xIsNext,
        })
        ```
    4. passing down *values* and *methods*
        ```jsx
        // after the three steps, the structures of each should be like this:
        //  `Board`                         renderSquare(i), render()
        //  `Game`      constructor(props), handleClick(i),  render()

        // now we're gonna passing down those stuff, again

        // `Game`
        render() { return (
            // before
            <Board />

            // after
            <Board
                squares={ current.squares }
                onClick={ (i) => this.handleClick(i) }
            />
        )}

        // `Board`
        renderSquare(i) { return (
            // props from `Game`
            this.state.squares[i]   =>  this.props.squares[i]
            this.handleClick  (i)   =>  this.props.onClick(i)
        )}
        ```

### Explanation with each commit 0x06
> Finish the rest of the *time machine* feature
1. build the conceptual list using `.map(OBJ, INDEX)`
   ```jsx
   // JavaScript - `Game` :: render() :: just below the `current`
   const movse = history.map(
       // step returns maps        e.g. {squares: [null, null ..]}
       // move returns indices     e.g. 0
       (step, move) => {
           // the visual part inside the tag `li`
           const desc = move ?
               `Go to move #{move}` :
               `Go to game start`;

           return (
               <li>
                   // this does the actual "time travelling"
                   <button onClick={ () => this.jumpTo(move) }>
                       {desc}
                   </button>
               </li>
           )
       }
   )

   // JSX        - `Game` :: render() :: return :: div :: game-info
   <ol>{moves}</ol>
     ```
2. the actual *time travelling* part (**re-create based on saved states**)
    - interaction
        1. playing normally
        2. display all the steps being taken as a list <small>(`<li>`)</small>
        3. click one of them
        4. current states were discarded
        5. re-create the specific step <small>(*from the beginning to that step*)
    - initialize an varible `stepNumber` for grabbing the *part* we want
        > e.g. when we *travel* to the 3nd step, we only want the *current* plus the *two previous steps* from `this.state.history`
    - implement `jumpTo` to do the *travelling*
        ```javascript
        // `Game` :: jumpTo
        jumpTo(step) {
            // trigger update -> re-render based on changed `history`
            this.setState({
                stepNumber: step,           // jump to where (2, 0->1->2)
                xIsNext: step % 2 === 0,    // make the order weren't distorted
            });
        }

        // `Game` :: render() :: replaced `history[history.length - 1]`
        const history = history[this.state.stepNumber]
        ```
    - update `handleClick(i)` to do the *visual update*
        ```javascript
        // `Game` :: handleClick(i) :: at the top
        const history =
            this.state.history.slice(
                0,
                this.state.stepNumber + 1
            )

        // `Game` :: handleClick(i) :: at the bottom
        this.setState({
            ..
            stepNumber: history.length,
        })
        ```
