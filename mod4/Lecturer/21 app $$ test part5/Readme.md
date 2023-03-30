# client --part2

`Login.js`, `Login.css`: компонент авторизации и его стили.\

```js
import React, { useState } from 'react';
import { useNavigate } from 'react-router-dom';
import './Login.css';

const Login = () => {
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');
  const navigate = useNavigate();

  const handleSubmit = async (event) => {
    event.preventDefault();

    try {
      const response = await fetch('http://localhost:5000/api/users/login', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({
          email,
          password,
        }),
      });

      const data = await response.json();

      if (response.ok) {
        // Здесь можно сохранить токен доступа, полученный от сервера, в localStorage или другом хранилище.
        localStorage.setItem('accessToken', data.accessToken);
        alert('Успешный вход!');
        navigate('/');
      } else {
        alert(`Ошибка входа: ${data.message}`);
      }
    } catch (error) {
      console.error('Ошибка при входе:', error);
      alert('Ошибка входа. Пожалуйста, попробуйте еще раз.');
    }
  };

  return (
    <div className="login-container">
      <h2>Вход</h2>
      <form className="login-form" onSubmit={handleSubmit}>
        <div className="login-form__group">
          <label className="login-form__label" htmlFor="email">Email: </label>
          <input
            id="email"
            className="login-form__input"
            type="email"
            value={email}
            onChange={(e) => setEmail(e.target.value)}
            required
          />
        </div>
        <div className="login-form__group">
          <label className="login-form__label" htmlFor="password">Пароль: </label>
          <input
            id="password"
            className="login-form__input"
            type="password"
            value={password}
            onChange={(e) => setPassword(e.target.value)}
            required
          />
        </div>
        <button className="login-form__submit" type="submit">Войти</button>
      </form>
    </div>
  );
};

export default Login;

```

```css
.login-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
  max-width: 400px;
  margin: 0 auto;
}

.login-form {
  display: flex;
  flex-direction: column;
  width: 100%;
}

.login-form__group {
  display: flex;
  flex-direction: column;
  margin-bottom: 1rem;
}

.login-form__label {
  margin-bottom: 0.5rem;
}

.login-form__input {
  padding: 0.5rem;
  font-size: 1rem;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.login-form__submit {
  padding: 0.5rem;
  font-size: 1rem;
  background-color: #007bff;
  color: #fff;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.2s;
}

.login-form__submit:hover {
  background-color: #0056b3;
}

```

`Register.js`, `Register.css`: компонент регистрации и его стили.\

```js
import React, { useState } from 'react';
import { useNavigate } from 'react-router-dom';
import './Register.css';

const Register = () => {
  const [username, setUsername] = useState('');
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');
  const [passwordConfirmation, setPasswordConfirmation] = useState('');
  const navigate = useNavigate();

  const handleSubmit = async (event) => {
    event.preventDefault();

    if (password !== passwordConfirmation) {
      alert('Пароли не совпадают');
      return;
    }

    //....
   try {
      const response = await fetch('http://localhost:5000/api/users/register', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({
          username,
          email,
          password,
        }),
      });

      const data = await response.json();

      if (response.ok) {
        alert('Успешная регистрация! Пожалуйста, войдите.');
        setUsername('');
        setEmail('');
        setPassword('');
        setPasswordConfirmation('');
        window.location('/login');
        // navigate('/login');
      } else {
        alert(`Ошибка регистрации: ${data.message}`);
      }
    } catch (error) {
      console.error('Ошибка при регистрации: ', error);
      alert('Ошибка регистрации. Пожалуйста, попробуйте еще раз.');
    }

    //....

    setUsername('');
    setEmail('');
    setPassword('');
    setPasswordConfirmation('');
    navigate('/login');
  };

  return (
    <div className="register">
      <h2>Регистрация</h2>
      <form onSubmit={handleSubmit}>
        <label htmlFor="username">Имя пользователя</label>
        <input
          type="text"
          id="username"
          value={username}
          onChange={(e) => setUsername(e.target.value)}
          required
        />
        <label htmlFor="email">Email</label>
        <input
          type="email"
          id="email"
          value={email}
          onChange={(e) => setEmail(e.target.value)}
          required
        />
        <label htmlFor="password">Пароль</label>
        <input
          type="password"
          id="password"
          value={password}
          onChange={(e) => setPassword(e.target.value)}
          required
        />
        <label htmlFor="passwordConfirmation">Подтверждение пароля</label>
        <input
          type="password"
          id="passwordConfirmation"
          value={passwordConfirmation}
          onChange={(e) => setPasswordConfirmation(e.target.value)}
          required
        />
        <button type="submit">Зарегистрироваться</button>
      </form>
    </div>
  );
};

export default Register;

```

```css
.register {
  display: flex;
  flex-direction: column;
  align-items: center;
  max-width: 400px;
  margin: 0 auto;
  padding: 1rem;
  background-color: #f5f5f5;
  border-radius: 4px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.register h1 {
  margin-bottom: 1rem;
}

.register form {
  display: flex;
  flex-direction: column;
  width: 100%;
}

.register label {
  margin-bottom: 0.5rem;
}

.register input {
  margin-bottom: 1rem;
  padding: 0.5rem;
  font-size: 1rem;
  border: 1px solid #aaa;
  border-radius: 4px;
}

.register button {
  padding: 0.5rem 1rem;
  font-size: 1rem;
  color: #fff;
  background-color: #333;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.register button:hover {
  background-color: #555;
}
```

`App.js`, `App.css`: основной компонент приложения и его стили.\

```js
import React from 'react';
import { BrowserRouter as Router, Route, Routes } from 'react-router-dom';
import Navbar from './components/Navbar';
import Register from './components/Register';
import Login from './components/Login';
import PostList from './components/PostList';
import PostForm from './components/PostForm';
import Post from './components/Post';

const App = () => {
  return (
    <Router>
      <Navbar />
      <div className="container">
        <Routes>
          <Route path="/" element={<PostList />} />
          <Route path="/register" element={<Register />} />
          <Route path="/login" element={<Login />} />
          <Route path="/create-post" element={<PostForm />} />
          <Route path="/post/:postId" element={<Post />} />
        </Routes>
      </div>
    </Router>
  );
};

export default App;

```

```css
.App {
  text-align: center;
}

.App-logo {
  height: 40vmin;
  pointer-events: none;
}

@media (prefers-reduced-motion: no-preference) {
  .App-logo {
    animation: App-logo-spin infinite 20s linear;
  }
}

.App-header {
  background-color: #282c34;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  font-size: calc(10px + 2vmin);
  color: white;
}

.App-link {
  color: #61dafb;
}

@keyframes App-logo-spin {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(360deg);
  }
}

```

`index.js`, `index.css`: точка входа в приложение и общие стили.\

```js
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';


ReactDOM.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
  document.getElementById('root')
);
```

```css
* {
  box-sizing: border-box;
}

body {
  margin: 0;
  font-family: Arial, sans-serif;
  line-height: 1.6;
  background-color: #f5f5f5;
}

a {
  text-decoration: none;
  color: #333;
}

ul {
  padding: 0;
}

ul li {
  list-style: none;
}

img {
  width: 100%;
  display: block;
}

```


_______________
_______________

# Домашнее задание

Запустите клинет на 3000 порту и сервер на 5000 порту