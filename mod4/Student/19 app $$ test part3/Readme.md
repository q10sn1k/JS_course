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
____________________________
____________________________
## Домашнее задание

## Задача 1

Напишите тест postController.test.js

## Задача 2

Напшем тест postRouter.test.js
