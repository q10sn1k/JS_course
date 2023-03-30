# server --part3

Напишем тест userController.test.js

```js
const { expect } = require('chai');
const sinon = require('sinon');
const bcrypt = require('bcrypt');
const httpMocks = require('node-mocks-http');
const userModel = require('../models/user');
const userController = require('../controllers/userController');

describe('userController', function () {
  this.timeout(5000);

  afterEach(() => {
    sinon.restore();
  });

  it('should create a new user', async () => {
    const req = httpMocks.createRequest({
      method: 'POST',
      body: {
        username: 'testuser',
        email: 'test@example.com',
        password: 'testpassword',
      },
    });
    const res = httpMocks.createResponse();

    sinon.stub(userModel, 'createUser').resolves(1);

    await userController.createUser(req, res);
    const jsonResponse = res._getJSONData();
    expect(res.statusCode).to.equal(201);
    expect(jsonResponse).to.have.property('message').that.equals('Пользователь успешно создан');
    expect(jsonResponse).to.have.property('userId').that.equals(1);
  });

  it('should return a user by id', async () => {
    const req = httpMocks.createRequest({
      method: 'GET',
      params: { id: 1 },
    });
    const res = httpMocks.createResponse();

    const user = {
      id: 1,
      username: 'testuser',
      email: 'test@example.com',
    };

    sinon.stub(userModel, 'getUserById').resolves(user);

    await userController.getUserById(req, res);
    const jsonResponse = res._getJSONData();
    expect(res.statusCode).to.equal(200);
    expect(jsonResponse).to.deep.equal(user);
  });

  it('should return a user by email', async () => {
    const req = httpMocks.createRequest({
      method: 'GET',
      params: { email: 'test@example.com' },
    });
    const res = httpMocks.createResponse();

    const user = {
      id: 1,
      username: 'testuser',
      email: 'test@example.com',
    };

    sinon.stub(userModel, 'getUserByEmail').resolves(user);

    await userController.getUserByEmail(req, res);
    const jsonResponse = res._getJSONData();
    expect(res.statusCode).to.equal(200);
    expect(jsonResponse).to.deep.equal(user);
  });

  });


```

Напшем тест userRouter.test.js

```js
const request = require('supertest');
const expect = require('chai').expect;
const { app, startServer, stopServer } = require('../app');
const userRoutes = require('../routes/userRoutes');

const TIMEOUT = 2000;

const testUser = {
  username: 'TestUser',
  email: 'testuser@example.com',
  password: 'testpassword',
};

describe('User API tests', function () {
    this.timeout(TIMEOUT);

    before(function (done) {
        startServer();
        done();
    });

    after(function (done) {
        stopServer();
        done();
    });

    // Test for user registration
    describe('POST /api/users/register', function () {
        this.timeout(TIMEOUT);

        it('should register a new user', async () => {
            const response = await request(app)
                .post('/api/users/register')
                .send(testUser);

            expect(response.status).to.equal(201);
            expect(response.body.message).to.equal('Пользователь успешно создан');
            expect(response.body.userId).to.be.a('number');
        });
    });

    // Test for getting a user by ID
    describe('GET /api/users/:id', function () {
        this.timeout(TIMEOUT);

        let userId;
        before(async () => {
            const response = await request(app)
                .post('/api/users/register')
                .send(testUser);

            userId = response.body.userId;
        });

        it('should get a user by id', async () => {
            const response = await request(app).get(`/api/users/${userId}`);

            expect(response.status).to.equal(200);
            expect(response.body.id).to.equal(userId);
            expect(response.body.username).to.equal('TestUser');
            expect(response.body.email).to.equal('testuser@example.com');
        });

        it('should return 404 when user not found', async () => {
            const nonExistentUserId = 999;
            const response = await request(app).get(`/api/users/${nonExistentUserId}`);

            expect(response.status).to.equal(404);
            expect(response.body.message).to.equal('User not found');
        });
    });

    // Test for getting a user by email
    describe('GET /api/users/email/:email', function () {
        this.timeout(TIMEOUT);

        let userEmail;
        before(async () => {
            // Удаление существующего пользователя с тем же адресом электронной почты
            await request(app)
                .delete('/api/users/email')
                .send({email: testUser.email});

            // Создание нового пользователя
            await request(app)
                .post('/api/users/register')
                .send(testUser);

            userEmail = testUser.email;
        });

        it('should get a user by email', async () => {
            const response = await request(app).get(`/api/users/email/${userEmail}`);

            console.log(response.body);

            expect(response.status).to.equal(200);
            expect(response.body.username).to.equal('testuser');
            expect(response.body.email).to.equal('testuser@example.com');
        });

        it('should return 404 when user not found by email', async () => {
            const nonExistentEmail = 'nonexistent@example.com';
            const response = await request(app).get(`/api/users/email/${nonExistentEmail}`);

            expect(response.status).to.equal(404);
            expect(response.body.message).to.equal('User not found');
        });
    });
});
```
____________________________
____________________________
____________________________
## Домашнее задание

## Задача 1

Напишите тест postController.test.js

## решение

```js
const chai = require('chai');
const sinon = require('sinon');
const postController = require('../controllers/postController');
const postModel = require('../models/post');

const { expect } = chai;

describe('Post Controller', () => {
  afterEach(() => {
    sinon.restore();
  });

  it('should get all posts', async () => {
    const posts = [
      { id: 1, title: 'Post 1', content: 'Content 1' },
      { id: 2, title: 'Post 2', content: 'Content 2' },
    ];

    const req = {};
    const res = {
      status: sinon.stub().returnsThis(),
      json: sinon.stub(),
    };
    const next = sinon.stub();

    sinon.stub(postModel, 'getAllPosts').resolves(posts);

    await postController.getAllPosts(req, res, next);

    expect(res.status.calledOnceWith(200)).to.be.true;
    expect(res.json.calledOnceWith(posts)).to.be.true;
    expect(next.notCalled).to.be.true;
  });

  it('should get a post by id', async () => {
  const post = { id: 1, title: 'Post 1', content: 'Content 1' };

  const req = {
    params: {
      id: post.id,
    },
  };
  const res = {
    status: sinon.stub().returnsThis(),
    json: sinon.stub(),
  };
  const next = sinon.stub();

  sinon.stub(postModel, 'getPostById').resolves(post);

  await postController.getPostById(req, res, next);

  expect(res.status.calledOnceWith(200)).to.be.true;
  expect(res.json.calledOnceWith(post)).to.be.true;
  expect(next.notCalled).to.be.true;
});

it('should return 404 when post not found by id', async () => {
  const req = {
    params: {
      id: 1,
    },
  };
  const res = {
    status: sinon.stub().returnsThis(),
    json: sinon.stub(),
  };
  const next = sinon.stub();

  sinon.stub(postModel, 'getPostById').resolves(null);

  await postController.getPostById(req, res, next);

  expect(res.status.calledOnceWith(404)).to.be.true;
  expect(res.json.calledOnceWith({ message: 'Post not found' })).to.be.true;
  expect(next.notCalled).to.be.true;
});

it('should create a new post', async () => {
  const post = { title: 'New Post', content: 'New Content' };
  const postId = 1;

  const req = {
    body: post,
  };
  const res = {
    status: sinon.stub().returnsThis(),
    json: sinon.stub(),
  };
  const next = sinon.stub();

  sinon.stub(postModel, 'createPost').resolves(postId);

  await postController.createPost(req, res, next);

  expect(res.status.calledOnceWith(201)).to.be.true;
  expect(res.json.calledOnceWith({ message: 'Post created', postId })).to.be.true;
  expect(next.notCalled).to.be.true;
});

it('should update a post', async () => {
  const postId = 1;
  const updatedPost = { title: 'Updated Post', content: 'Updated Content' };

  const req = {
    params: {
      id: postId,
    },
    body: updatedPost,
  };
  const res = {
    status: sinon.stub().returnsThis(),
    json: sinon.stub(),
  };
  const next = sinon.stub();

  sinon.stub(postModel, 'updatePost').resolves(1);

  await postController.updatePost(req, res, next);

  expect(res.status.calledOnceWith(200)).to.be.true;
  expect(res.json.calledOnceWith({ message: 'Post updated', affectedRows: 1 })).to.be.true;
  expect(next.notCalled).to.be.true;
});

  it('should return 404 when updating a non-existent post', async () => {
  const postId = 1;
  const updatedPost = { title: 'Updated Post', content: 'Updated Content' };

  const req = {
    params: {
      id: postId,
    },
    body: updatedPost,
  };
  const res = {
    status: sinon.stub().returnsThis(),
    json: sinon.stub(),
  };
  const next = sinon.stub();

  sinon.stub(postModel, 'updatePost').resolves(0);

  await postController.updatePost(req, res, next);

  expect(res.status.calledOnceWith(404)).to.be.true;
  expect(res.json.calledOnceWith({ message: 'Post not found' })).to.be.true;
  expect(next.notCalled).to.be.true;
});

it('should delete a post', async () => {
  const postId = 1;

  const req = {
    params: {
      id: postId,
    },
  };
  const res = {
    status: sinon.stub().returnsThis(),
    json: sinon.stub(),
  };
  const next = sinon.stub();

  sinon.stub(postModel, 'deletePost').resolves(1);

  await postController.deletePost(req, res, next);

  expect(res.status.calledOnceWith(200)).to.be.true;
  expect(res.json.calledOnceWith({ message: 'Post deleted', affectedRows: 1 })).to.be.true;
  expect(next.notCalled).to.be.true;
});

it('should return 404 when deleting a non-existent post', async () => {
  const postId = 1;

  const req = {
    params: {
      id: postId,
    },
  };
  const res = {
    status: sinon.stub().returnsThis(),
    json: sinon.stub(),
  };
  const next = sinon.stub();

  sinon.stub(postModel, 'deletePost').resolves(0);

  await postController.deletePost(req, res, next);

  expect(res.status.calledOnceWith(404)).to.be.true;
  expect(res.json.calledOnceWith({ message: 'Post not found' })).to.be.true;
  expect(next.notCalled).to.be.true;
});
});

```

## Задача 2

Напшем тест postRouter.test.js

## решение

```js
const request = require('supertest');
const expect = require('chai').expect;
const { app, startServer, stopServer } = require('../app');
const postRoutes = require('../routes/postRoutes');

const TIMEOUT = 10000; // 10 seconds

describe('API tests', function () {
  this.timeout(TIMEOUT);

  before(function (done) {
    startServer();
    done();
  });

  after(function (done) {
    stopServer();
    done();
  });
  // app.use('/api/posts', postRoutes);

  describe('POST /api/posts', function () {
    this.timeout(TIMEOUT);

    it('should create a new post and return its id', async () => {
      const response = await request(app)
          .post('/api/posts')
          .send({title: 'Test Post', content: 'Test Content'});

      expect(response.status).to.equal(201);
      expect(response.body.message).to.equal('Post created');
      expect(response.body.postId).to.be.a('number');
    });
  });

  describe('GET /api/posts', function () {
    this.timeout(TIMEOUT);
    it('should return a list of posts', async () => {
      const response = await request(app).get('/api/posts');

      expect(response.status).to.equal(200);
      expect(response.body).to.be.an('array');
    });
  });

  describe('GET /api/posts/:id', function () {
  this.timeout(TIMEOUT);

  let postId;
  before(async () => {
    const response = await request(app)
      .post('/api/posts')
      .send({ title: 'Test Post', content: 'Test Content' });

    postId = response.body.postId;
  });

  it('should return a post by id', async () => {
    const response = await request(app).get(`/api/posts/${postId}`);

    expect(response.status).to.equal(200);
    expect(response.body.id).to.equal(postId);
  });

    it('should return 404 when post not found', async () => {
      const postId = 999;
      const response = await request(app).get(`/api/posts/${postId}`);

      expect(response.status).to.equal(404);
      expect(response.body.message).to.equal('Post not found');
    });
  });

  describe('PUT /api/posts/:id', function () {
  this.timeout(TIMEOUT);

  let postId;
  before(async () => {
    const response = await request(app)
      .post('/api/posts')
      .send({ title: 'Test Post', content: 'Test Content' });

    postId = response.body.postId;
  });

  it('should update a post and return the number of affected rows', async () => {
    const updatedPost = {title: 'Updated Post', content: 'Updated Content'};
    const response = await request(app)
      .put(`/api/posts/${postId}`)
      .send(updatedPost);

    expect(response.status).to.equal(200);
    expect(response.body.message).to.equal('Post updated');
    expect(response.body.affectedRows).to.equal(1);
  });

    it('should return 404 when updating a non-existent post', async () => {
      const postId = 999;
      const updatedPost = {title: 'Updated Post', content: 'Updated Content'};
      const response = await request(app)
          .put(`/api/posts/${postId}`)
          .send(updatedPost);

      expect(response.status).to.equal(404);
      expect(response.body.message).to.equal('Post not found');
    });
  });

  describe('DELETE /api/posts/:id', function () {
  this.timeout(TIMEOUT);

  let postId;
  before(async () => {
    const response = await request(app)
      .post('/api/posts')
      .send({ title: 'Test Post', content: 'Test Content' });

    postId = response.body.postId;
  });

  it('should delete a post and return the number of affected rows', async () => {
    const response = await request(app).delete(`/api/posts/${postId}`);

    expect(response.status).to.equal(200);
    expect(response.body.message).to.equal('Post deleted');
    expect(response.body.affectedRows).to.equal(1);
  });

    it('should return 404 when deleting a non-existent post', async () => {
      const postId = 999;
      const response = await request(app).delete(`/api/posts/${postId}`);
      expect(response.status).to.equal(404);
      expect(response.body.message).to.equal('Post not found');
    });
  });
});


```


