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

### What is " e " or sometimes referred as event inside the inside the callback above?
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

