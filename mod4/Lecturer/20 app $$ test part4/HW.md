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