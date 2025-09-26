# REACT-INTERVIEW QUESTIONS

### What is the differene between class based component and functional components?
- Class based components are created using Es6 class syntax
- functional components are just plain javascript functions that returns jsx
- To manange state in class based components we have use this.state in the component and to set the state we use the.setState() method.
- The class based component must extend React.Component class and it must implement the render method of that class.
- Functional component does not have any render method and to manage state in functional component we use useState hook.


### What is component?
- Component is a resuable piece of ui which can be used at multiple parts of ui and is independent of other of ui Components.


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
- UseState is a React hook that lets us update and hold the state of a functional component during re-renders. 
- useState also provides us with the function which can update the state of a functional component.

- defination=> const [ stateVariable,setStateFunction ]= useState(InitialStateValue).
- stateVariable=> The current state of the component.
- setStateFunction=> Function for updating the state of the component.

### How to Update Differenent States?
- In Case of Normal Variables

```javascript
 const [On, setOn] = useState(true);
 const handleClick = () => {
    setOn((prev) => !prev);
  };
// The prev inside the callback is the reference to the current state of the variable inside the component.

```
- In Case of Objects Inside the state

    -  Key Point to Note=> We should never mutate the state directly instead we should return a new Object.
    -  Because React relies on state to track the changes in UI and then cause a re-render.

``` javascript
import { useState } from 'react';
export default function ProfileEditor() {
  const [ProfileDetail, setProfileDetail] = useState({
    firstName: '',
    lastName: '',
    email: '',
  });
  const handleFNameChange = (e) => {
    // if we do something like this
    // setProfileDetail((prev) => {
    //   prev.firstName= e.target.value  //We are mutating the directly which we should avoid
    // });

    setProfileDetail((prev) => ({
      ...prev, //copy the existing object
      firstName: e.target.value, //update only firstName
    }));
  };
  const handleFormSubmission = (e) => {
    e.preventDefault();
    if (
      ProfileDetail.firstName &&
      ProfileDetail.lastName &&
      ProfileDetail.email
    ) {
      alert(
        `Hey ${ProfileDetail.firstName + ProfileDetail.lastName}, How you doin!`
      );
      return;
    } else alert(`Please Fill All the details Champ!`);
  };

  return (
    <form onSubmit={handleFormSubmission}>
      <input
        type="text"
        placeholder="FirstName"
        value={ProfileDetail.firstName}
        onChange={handleFNameChange}
      />
      <input
        type="text"
        placeholder="LastName"
        value={ProfileDetail.lastName}
        onChange={(e) =>
          setProfileDetail((prev) => ({ ...prev, lastName: e.target.value }))
        }
      />
      <input
        type="email"
        placeholder="example@gmail.com"
        value={ProfileDetail.email}
        onChange={(e) =>
          setProfileDetail((p) => ({ ...p, email: e.target.value }))
        }
      />
      <button>Submit</button>
    </form>
  );
}
```
- In Case of Array Updates

```javascript
import { useState } from 'react';
export default function ProfileEditor() {
  const [ProfileAttributes, setProfileAttributes] = useState({
    firstName: '',
    lastName: '',
    email: '',
  });
  const [Profiles, setProfiles] = useState([
    { id: 1, name: 'Rohit Singh', email: 'devbyrohit@gmail.com' },
    { id: 2, name: 'John', email: 'johnbhai@gmail.com' },
  ]);
  const handleFormSubmission = (e) => {
    e.preventDefault();
    //Create a object that you want to insert in the array
    const newObj = {
      id: 3,
      name: ProfileAttributes.firstName + ' ' + ProfileAttributes.lastName,
      email: ProfileAttributes.email,
    };
    if (
      ProfileAttributes.firstName &&
      ProfileAttributes.lastName &&
      ProfileAttributes.email
    ) {
        // and do it like this
        //copy the previous elements and then add the new object
      setProfiles((prev) => [...prev, newObj]);
      return;
    } else alert(`Please Fill All the details Champ!`);
  };

  return (
    <div>
      <div>
        <table border="1">
          <thead>
            <tr>
              <th>Id</th>
              <th>Name</th>
              <th>Email</th>
            </tr>
          </thead>
          <tbody>
            {Profiles.map((item) => (
              <tr key={item.id}>
                <td>{item.id}</td>
                <td>{item.name}</td>
                <td>{item.email}</td>
              </tr>
            ))}
            <tr></tr>
          </tbody>
        </table>
      </div>
      <form onSubmit={handleFormSubmission}>
        <input
          type="text"
          placeholder="FirstName"
          value={ProfileAttributes.firstName}
          onChange={(e) =>
            setProfileAttributes((p) => ({ ...p, firstName: e.target.value }))
          }
        />
        <input
          type="text"
          placeholder="LastName"
          value={ProfileAttributes.lastName}
          onChange={(e) =>
            setProfileAttributes((prev) => ({ ...prev, lastName: e.target.value }))
          }
        />
        <input
          type="email"
          placeholder="example@gmail.com"
          value={ProfileAttributes.email}
          onChange={(e) =>
            setProfileAttributes((p) => ({ ...p, email: e.target.value }))
          }
        />
        <button>Submit</button>
      </form>
    </div>
  );
}

```

### What is " e " or sometimes referred as event inside the callback above?
- " e " is just a event object that represents the information about the event that just occured it can form submit , value change (onChange) or other events which are avaibable in javascript.
- Some methods and properties are as follows:-
    - e.type => Represents the type of event that occured.
    - e.target => Represents the element that triggered the event.(button,input fiels etc).
    - e.currentTarget => Represents the element that event handler is attached to.
    - e.preventDefault() => It prevents default browser behaviour such as form submission.
    - e.target.value => It represents the value of the element which triggered the event.

### What is UseEffect hook?
- UseEffect is special type of hook that lets us perform side effects in a functional component and also lets us hook to the lifecycle methods of react.
- Side Effects are anything that affects something outside the component like fetching the data, subscribing to events or timers, manuplating the dom directly, logging .local storage updates etc.
- It takes two arguments first is a callback function and second is dependency array.
- Callback function is the place where side effects are executed.
- dependency array is array of items that affect the execution of the callback back (means it will execute again if the item in dependency array changes).
- Empty dependency array means the side effect will only run once during the component insertion in DOM.

- Syntax
```javascript

useEffect(()=>{... sideEffects},[item1,item2]);

```
### What is Fetch and why is it used?
- Fetch is a built in javascript function that is used to make an Http request from the browser to a server. it's commonly used fro getting the data from the server or sending the dat to a server through an API.

- fetch returns us a promise that resolves to a response ,not the actual data.
- Response Object contains headers,status and body(in row form).
- The body of response is a ReadableStream.We cannot use it directly in javascript that is why we need to convert it explicitly to json format or whatever format we need.
- .json() it parses the stream to javascript object /arrays
- when we send the data from browser to server we stringify the data , because we cannot directly send the javascript object in the http request body so we convert it into JSON format.

- Syntax

```javascript
            fetch(API,{
            method:'POST/GET/PATCH/DELETE/'
            headers:{
                'Content-Type':'Application/json'
            },
            body:JSON.stringify({
                //data to be sent to server
            )}
            })
            .then((res)=>res.json())
            .then((data)=>console.log(data))

```

### How does fetch API and useEffect work together?

```javascript
import { useState, useEffect } from 'react';
const API = 'https://jsonplaceholder.typicode.com/todos';
export default function Todos() {
  const [todos, setTodos] = useState([]);

//This useEffect will only run once.
  useEffect(() => {
    async function fetchData() {
      const response = await fetch(API);
      const data = await response.json();
      setTodos(data);
    }
    fetchData();
  }, []);

  return todos ? (
    <table border="1">
      <thead>
        <tr>
          <th>Id</th>
          <th>title</th>
          <th>Completed</th>
        </tr>
      </thead>
      <tbody>
        {todos.map((item) => (
          <tr key={item.id}>
            <td>{item.id}</td>
            <td>{item.title}</td>
            <td>{item.completed}</td>
          </tr>
        ))}
      </tbody>
    </table>
  ) : (
    <h1>You don't have any todo </h1>
  );
}
```

### What is Debouncing and Provide a Example of Debouncing.
- Deboucing is a mechanism or concept through which we delay the exeution of function or task until a specified interval of time.
- In Debouncing we use the setTimeOut web api function to delay the execution of a function.
- Everytime the user interrupt the timer the previous timer is cleared and a new timer is created for same delay passed as argument.
- Example is Delaying the API Call for Seaching when doing search on a website.

- Check out the below image for debouncing

[Debouncing](https://miro.medium.com/v2/resize:fit:1400/1*sRRuxhp8rJK5fKDINFEL1Q.png)

#### Search Using Debounce

```javascript
import { useMemo, useState } from 'react';

export default function Search() {
  // our search box should give me some suggestions based on user typing....

  //Backend Mock Api response
  const suggestions = [
    {
      R: ['React', 'Ruby on rails', 'RollUp', 'React Dom'],
      e: ['React', 'React Dom', 'esBuild', 'es'],
      a: ['React', 'Reaction', 'Reactjs', 'Realtime', 'Reality'],
      c: [
        'React',
        'Reaction',
        'Reactjs',
        'Realtime',
        'Reality',
        'Reacon Stark',
      ],
      t: ['React', 'Reactjs', 'React interview Question', 'React in realtime'],
    },
  ];
  const [searchValue, setSearchValue] = useState('');
  const [SuggestionsDropDown, setSuggestionsDropDown] = useState(null);

  function getSuggestions(value) {
    let size = value.length;
    let char = value[size - 1];
    for (const [key, value] of Object.entries(suggestions[0])) {
      if (key === char) {
        return value;
      }
    }
    return [];
  }

  const Debouce = (fn, delay) => {
    let timerId;
    return function (...args) {
      if (timerId) clearTimeout(timerId);
      timerId = setTimeout(() => {
        fn(...args);
      }, delay);
    };
  };

  // debounced version of getSuggestions
  const DebouncedGetSuggestions = useMemo(() =>
    Debouce((val) => {
      const result = getSuggestions(val);
      setSuggestionsDropDown(result);
    }, 1000)
  );

  const handleInputChange = (e) => {
    setSearchValue(e.target.value);
    DebouncedGetSuggestions(e.target.value);
  };
  return (
    <div>
      <div
        style={{
          width: '100%',
          display: 'flex',
          flexDirection: 'column',
          alignItems: 'center',
          justifyContent: 'center',
          marginTop: '3rem',
        }}
      >
        <input
          style={{ width: '50vw', height: '2rem' }}
          type="text"
          placeholder="search"
          value={searchValue}
          onChange={handleInputChange}
        />
        {SuggestionsDropDown && (
          <ul style={{ border: '1px solid grey', width: '50vw' }}>
            {SuggestionsDropDown.map((suggestion) => (
              <li style={{ listStyle: 'none', cursor: 'pointer' }}>
                {suggestion}
              </li>
            ))}
          </ul>
        )}
      </div>
    </div>
  );
}

```


### What is useMemo hook?
- useMemo hook lets us cache the result of a calculation during re-renders.
- useMemo hook only cache the last result of calculation not all the results of a calculation.
- useMemo hook is also used for skipping the re-renders of a component.
- useMemo hook is also used for caching the dependency of other hook.
- Do not use useMemo for all the task use it if we have expensive function calls or heavy math to do.

- Syntax
  - let value=useMemo(()=>calculationFunc,[])
  - returns a value of the executed function and executes the function only if dependency changes.

#### Things that cause re-render
- A React component re-renders whenever React thinks its output (UI) might change due to:
  - State update
  - Prop update
  - Context update
  - Parent re-render

#####  function's value memoization.

- Before useMemo

```javascript
import { useState, useMemo, useEffect } from 'react';

export default function () {
  const [inputValue, setInputValue] = useState('');
  const [counter, setCounter] = useState(0);

  
  function calculate(val) {
    console.log('executing');
    return val * val;
  }
  // everytime the setCounter changes the state useState schedules a re-render so this function will be called again and this a expensive call just kidding.
  let double = calculate(4);
  return (
    <div style={{ display: 'flex' }}>
      <div>Counter: {counter}</div>
      <button onClick={(e) => setCounter(counter + 1)}>Increment</button>
      <div>{double}</div>
    </div>
  );
}

```
- After useMemo

```javascript
import { useState, useMemo, useEffect } from 'react';
export default function () {
  const [inputValue, setInputValue] = useState('');
  const [counter, setCounter] = useState(0);

  // Let's mimic this as expensive function call which should be happening again and again so let's memoize it.
  function calculate(val) {
    console.log('executing');
    return val * val;
  }
  // we use memoization here
  // if and only if the val changes then only useMemo hook will run again.
  //here val can be a state variable
  let val = 4;
  let double = useMemo(() => calculate(val), [val]);
  return (
    <div style={{ display: 'flex' }}>
      <div>Counter: {counter}</div>
      <button onClick={(e) => setCounter(counter + 1)}>Increment</button>
      <div>{double}</div>
    </div>
  );
}
```


### what is useCallback hook?
- In react with every re-render the function's refernces are created again.
- This may cause unnecssary re-render of the child component if the function is passed as a prop to them.
- So the rescuer here is useCallback which is used to cache such functions for which we do not want to create references again and again.
- Some might be thinking that this is what useMemo hook does too. Wrong, Cause it just caches the function's value not the entire function.
- It return's a function not the value.
- It caches only the function reference.

#### Usage:-
- When passing the function as prop to child component.
- when using functions inside useEffect or other hooks.
- Expensive function def where we do not want to re-calculate everything.

- Syntax:-
- useCallback(()=>fn,[])

```javascript
import { useState, useCallback, useEffect } from 'react';
export default function () {
  const [counter, setCounter] = useState(0);
  let val = 4;
  const double = useCallback((num) => {
    console.log('executing');
    return num * num;
  }, []);
  useEffect(() => {
    console.log('Function reference:', double);
  });
  const value = double(val);
  return (
    <div style={{ display: 'flex' }}>
      <div>Counter: {counter}</div>
      <button onClick={(e) => setCounter(counter + 1)}>Increment</button>
      <div>{value}</div>
    </div>
  );
}

```


### What is lifting State up?
- Sometimes, you want the state of two components to always change together. To do it, remove state from both of them, move it to their closest common parent, and then pass it down to them via props. This is known as lifting state up.


