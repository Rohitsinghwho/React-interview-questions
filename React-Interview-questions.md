# REACT-INTERVIEW QUESTIONS

### What is the differene between class based component and functional components?
- Class based components are created using Es6 class syntax
- functional components are just plain javascript functions that returns jsx
- To manange state in class based componets we have use this.state in the component and to set the state we use the.setState() method.
- The class based component must extend React.Component class and it must implement the render method of that class.
- Functional component does not have any render method and to manage state in functional component we use useState hook.


### What is component?
- Component is a resuable piece of ui which can be used at multiple parts of ui and is independent of other of ui.


### What is hook?
- A hook is a special function in react that lets you use features like state and lifecycle methods inside a functional component.

### What are the lifecycle methods?
- React's class based components had lifecycle method's for a component life journey.
- There were mainly three phases-:
    - 1. Mounting Phase-:Component is created and inserted in the DOM.
    - 2. Updating-:Components state or props changes,causing re-render.
    - 3. Unmounting-:Component is removed from the dom
 - Detailed:-
    - 1. Mounting Phase flow=> constructor->render->componentDidMount()(Componenet did mount runs only once when the component is inserted into     dom, Good for API calls timers etc)
    - 2. Updating Phase flow=> render()->componentDidUpdate(prevProps,prevStates)(Runs after changes are flushed to dom)
    - 3. Unomounting Phase flow=> componentWillMount()(called right before component is destroyed)Good for cleanup like removing listeners etc.

- Functional Components can handle the lifecycle methods using useEffect hook.
    - 1. Mounting Phase=> If we pass empty dependency array to a useEffet than the behaviour will be similar to componentDidMount()([] empty dep - array make sure that the effect runs only once)
    - 2. Updating Phase =>we know that it runs after states props are changed so => if we pass the state/props in the dependency array that it will behave the same and effect will only run when that depenency array  has any changes.
    - 3. Unmounting Phase=>in useEffect , return a cleanup function to handle this


### What is state?
- State is an object which repersents the daynamic data about a component. Think of state as current situation of the component and how will be rendered.


### what is useState hook?
- 


