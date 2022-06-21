# ⮚	Google Authentication
## METHOD ONE

## ○	FirebaseConfiguration.js
``` javascript
import { initializeApp } from 'firebase/app';
import {getAuth} from "firebase/auth";
// your app's Firebase project configuration
const firebaseConfig = {
      //configuration, you will get these values from firebase once you create the project
      apiKey: "AIzaSyDazwdAUDlT9JligyjPHJrx3fvRskHLMe0",
      authDomain: "chatbox-d6e1b.firebaseapp.com",
      projectId: "chatbox-d6e1b",
      storageBucket: "chatbox-d6e1b.appspot.com",
      messagingSenderId: "148921123108",
      appId: "1:148921123108:web:8b097c140d4ed3e6ddf3b1",
      measurementId: "G-Z0LN4RM839"
};
const app = initializeApp(firebaseConfig);
export const firebase_authentication = getAuth(app);

```

## ○	App.js
``` javascript
import './App.css';
import {SignOut} from './authentication/SignOut';
import {SignIn} from './authentication/SignIn';
import Home from './components/Home';
import {ChatRoom} from './chatbox/ChatRoom';
import {BrowserRouter as Router, Routes, Route} from 'react-router-dom';

import {signInWithPopup, GoogleAuthProvider} from "firebase/auth";
import {firebase_authentication} from './authentication/FirebaseConfiguration';
function App() {
  const SignInWithGoogle = ()=>{
    const provider = new GoogleAuthProvider();
    signInWithPopup(firebase_authentication, provider)
    .then((user)=>{
      console.log(user);
    })
    .catch((err)=>{
      console.log(err);
    })
  }
  return (
    <div className="App">

            <button onClick={SignInWithGoogle}>
                  SignInWithGoogle
            </button>

      <Router>
        <Routes>
          <Route exact path='/' element={<Home />} />
        </Routes>
        <Routes>
          <Route exact path='/' element={<SignOut />} />
        </Routes>
        <Routes>
          <Route exact path='/' element={<SignIn />} />
        </Routes>
        <Routes>
          <Route exact path='/' element={<ChatRoom />} />
        </Routes>
      </Router>

    </div>
  );
}

```

## useReducer
``` javascript
import { useReducer } from "react";
import ReactDOM from "react-dom/client";

const initialTodos = [
  {
    id: 1,
    title: "Todo 1",
    complete: false,
  },
  {
    id: 2,
    title: "Todo 2",
    complete: false,
  },
];

const reducer = (state, action) => {
  switch (action.type) {
    case "COMPLETE":
      return state.map((todo) => {
        if (todo.id === action.id) {
          return { ...todo, complete: !todo.complete };
        } else {
          return todo;
        }
      });
    default:
      return state;
  }
};

function Todos() {
  const [todos, dispatch] = useReducer(reducer, initialTodos);

  const handleComplete = (todo) => {
    dispatch({ type: "COMPLETE", id: todo.id });
  };

  return (
    <>
      {todos.map((todo) => (
        <div key={todo.id}>
          <label>
            <input
              type="checkbox"
              checked={todo.complete}
              onChange={() => handleComplete(todo)}
            />
            {todo.title}
          </label>
        </div>
      ))}
    </>
  );
}

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(<Todos />);

```
