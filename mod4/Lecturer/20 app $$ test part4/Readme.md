# client --part1

Архитектура клиентской части

```
├── client
│   ├── public
│   │   └── ...
│   │   
│   │   
│   │
│   ├── src
│   │   ├── components
│   │   │   ├── Navbar.js
│   │   │   ├── Navbar.css
│   │   │   ├── Register.js
│   │   │   ├── Register.css
│   │   │   ├── Login.js
│   │   │   ├── Login.css
│   │   │   ├── PostList.css
│   │   │   ├── PostForm.js
│   │   │   ├── PostForm.css
│   │   │   ├── Post.js
│   │   │   └── Post.css
│   │   │
│   │   ├── App.js
│   │   ├── App.css
│   │   ├── index.js
│   │   └── index.css
│   │   
│   │   
│   │
│   ├── package.json
│   └── package-lock.json
│
├── server
│   ├── ...
```

`project`: корневая директория проекта.\
`client`: директория с клиентской частью приложения на React.js.\
`public`: статические файлы и index.html.\
`src`: исходный код клиентской части приложения.\
`components`: компоненты приложения.\
`Navbar.js`, `Navbar.css`: компонент навигационной панели и его стили.\
`Register.js`, `Register.css`: компонент регистрации и его стили.\
`Login.js`, `Login.css`: компонент авторизации и его стили.\
`PostList.js`, `PostList.css`: компонент списка постов и его стили.\
`PostForm.js`, `PostForm.css`: компонент формы создания поста и его стили.\
`Post.js`, `Post.css`: компонент отдельного поста и его стили.\
`App.js`, `App.css`: основной компонент приложения и его стили.\
`index.js`, `index.css`: точка входа в приложение и общие стили.\

____________________________
____________________________



`Navbar.js`, `Navbar.css`: компонент навигационной панели и его стили.

```js
import React from 'react';
import { Link } from 'react-router-dom';
import './Navbar.css';

const Navbar = () => {
  return (
    <nav className="navbar">
      <div className="navbar__brand navbar__links">
        <Link to="/">Блог</Link>
      </div>
      <div className="navbar__links">
        <Link to="/create-post">Создать пост</Link>
        <Link to="/register">Регистрация</Link>
        <Link to="/login">Вход</Link>
      </div>
    </nav>
  );
};

export default Navbar;

```

```css
.navbar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background-color: #333;
  padding: 1rem;
}

.navbar__brand,
.navbar__links a {
  color: #fff;
  text-decoration: none;
}

.navbar__links a {
  font-size: 20px;
}

.navbar__links {
  display: flex;
  gap: 1rem;
}

.navbar__links a:hover {
  color: #ccc;
}

```

протестируем

```js
import React from 'react';
import { BrowserRouter } from 'react-router-dom';
import { render, screen } from '@testing-library/react';
import Navbar from '../Navbar';

describe('Navbar', () => {
  test('содержит ссылку на главную страницу', () => {
    render(
      <BrowserRouter basename="/">
        <Navbar />
      </BrowserRouter>
    );
    const homeLink = screen.getByRole('link', { name: 'Блог' });
    expect(homeLink).toBeInTheDocument();
    expect(homeLink.getAttribute('href')).toBe('/');
  });

  test('содержит ссылку на страницу создания поста', () => {
    render(
      <BrowserRouter basename="/">
        <Navbar />
      </BrowserRouter>
    );
    const createPostLink = screen.getByRole('link', { name: 'Создать пост' });
    expect(createPostLink).toBeInTheDocument();
    expect(createPostLink.getAttribute('href')).toBe('/create-post');
  });

  test('содержит ссылку на страницу регистрации', () => {
    render(
      <BrowserRouter basename="/">
        <Navbar />
      </BrowserRouter>
    );
    const registerLink = screen.getByRole('link', { name: 'Регистрация' });
    expect(registerLink).toBeInTheDocument();
    expect(registerLink.getAttribute('href')).toBe('/register');
  });

  test('содержит ссылку на страницу входа', () => {
    render(
      <BrowserRouter basename="/">
        <Navbar />
      </BrowserRouter>
    );
    const loginLink = screen.getByRole('link', { name: 'Вход' });
    expect(loginLink).toBeInTheDocument();
    expect(loginLink.getAttribute('href')).toBe('/login');
  });
});

```

Реализуем компонент `PostList.js`, `PostList.css`: компонент списка постов и его стили.

```js
import React, { useState, useEffect } from 'react';
import { Link } from 'react-router-dom';
import './PostList.css';

const PostList = () => {
  const [posts, setPosts] = useState([]);

  useEffect(() => {
    const fetchPosts = async () => {
      try {
        const response = await fetch('http://localhost:5000/api/posts');
        const data = await response.json();
        if (response.ok) {
          setPosts(data.posts);
        } else {
          alert(`Ошибка загрузки постов: ${data.message}`);
        }
      } catch (error) {
        console.error('Ошибка при загрузке постов:', error);
        alert('Ошибка загрузки постов. Пожалуйста, попробуйте еще раз.');
      }
    };

    fetchPosts();
  }, []);

  return (
    <div className="post-list">
      <h2>Список постов</h2>
      {posts.map((post) => (
        <div className="post-list__item" key={post.id}>
          <h3 className="post-list__title">{post.title}</h3>
          <p className="post-list__content">{post.content.substring(0, 100)}...</p>
          <Link className="post-list__read-more" to={`/posts/${post.id}`}>
            Читать далее
          </Link>
        </div>
      ))}
    </div>
  );
};

export default PostList;
```

```css
.post-list {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
  max-width: 800px;
  margin: 0 auto;
}

.post-list__item {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  background-color: #f8f9fa;
  padding: 1rem;
  border-radius: 4px;
  margin-bottom: 1rem;
}

.post-list__title {
  margin: 0 0 0.5rem;
}

.post-list__content {
  margin: 0 0 1rem;
}

.post-list__read-more {
  text-decoration: none;
  color: #007bff;
}

.post-list__read-more:hover {
  text-decoration: underline;
}

```

`PostForm.js`, `PostForm.css`: компонент формы создания поста и его стили.\

```js
import React, { useState } from 'react';
import { useNavigate } from 'react-router-dom';
import './PostForm.css';

const PostForm = () => {
  const [title, setTitle] = useState('');
  const [content, setContent] = useState('');
  const navigate = useNavigate();

  const handleSubmit = async (event) => {
    event.preventDefault();

    try {
      const response = await fetch('http://localhost:5000/api/posts', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
          // Здесь необходимо добавить заголовок для авторизации, если требуется
          'Authorization': `Bearer ${localStorage.getItem('accessToken')}`,
        },
        body: JSON.stringify({
          title,
          content,
        }),
      });

      const data = await response.json();

      if (response.ok) {
        alert('Пост успешно создан!');
        setTitle('');
        setContent('');
        navigate('/');
      } else {
        alert(`Ошибка создания поста: ${data.message}`);
      }
    } catch (error) {
      console.error('Ошибка при создании поста:', error);
      alert('Ошибка создания поста. Пожалуйста, попробуйте еще раз.');
    }
  };

  return (
    <div className="post-form">
      <h2>Создать новый пост</h2>
      <form onSubmit={handleSubmit}>
        <div className="post-form__group">
          <label className="post-form__label">Заголовок: </label>
          <input
            className="post-form__input"
            type="text"
            value={title}
            onChange={(e) => setTitle(e.target.value)}
            required
          />
        </div>
        <div className="post-form__group">
          <label className="post-form__label">Контент: </label>
          <textarea
            className="post-form__textarea"
            value={content}
            onChange={(e) => setContent(e.target.value)}
            required
          />
        </div>
        <button className="post-form__submit" type="submit">Создать пост</button>
      </form>
    </div>
  );
};

export default PostForm;

```

```css
.post-list {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
  max-width: 800px;
  margin: 0 auto;
}

.post-list__item {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  background-color: #f8f9fa;
  padding: 1rem;
  border-radius: 4px;
  margin-bottom: 1rem;
}

.post-list__title {
  margin: 0 0 0.5rem;
}

.post-list__content {
  margin: 0 0 1rem;
}

.post-list__read-more {
  text-decoration: none;
  color: #007bff;
}

.post-list__read-more:hover {
  text-decoration: underline;
}

```

_____________
_____________
_____________
# Домашнее задание


# Задача 1

`Post.js`, `Post.css`: компонент отдельного поста и его стили.\


```js
import React, { useState, useEffect } from 'react';
import { useParams, useNavigate } from 'react-router-dom';
import './Post.css';

const Post = () => {
  const [post, setPost] = useState(null);
  const { postId } = useParams();
  const navigate = useNavigate();

  useEffect(() => {
    const fetchPost = async () => {
      try {
        const response = await fetch(`http://localhost:5000/api/posts/${postId}`);
        const data = await response.json();

        if (response.ok) {
          setPost(data.post);
        } else {
          alert(`Ошибка загрузки поста: ${data.message}`);
        }
      } catch (error) {
        console.error('Ошибка при загрузке поста:', error);
        alert('Ошибка загрузки поста. Пожалуйста, попробуйте еще раз.');
      }
    };

    fetchPost();
  }, [postId]);

  if (!post) {
    return <div>Загрузка поста...</div>;
  }

  return (
    <div className="post">
      <h2 className="post__title">{post.title}</h2>
      <p className="post__content">{post.content}</p>
      <button className="post__back-btn" onClick={() => navigate(-1)}>Назад</button>
    </div>
  );
};

export default Post;

```

```css
.post {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
  max-width: 800px;
  margin: 0 auto;
}

.post__title {
  margin: 0 0 1rem;
}

.post__content {
  margin: 0 0 1rem;
  text-align: justify;
}

.post__back-btn {
  padding: 0.5rem;
  font-size: 1rem;
  background-color: #ccc;
  color: #000;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.2s;
}

.post__back-btn:hover {
  background-color: #999;
}

```