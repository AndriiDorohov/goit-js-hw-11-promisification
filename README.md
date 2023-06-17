## Task 1

Write the `delay(ms)` function, which returns a promise that goes into the state
`"resolved"` in `ms` milliseconds. The value of the fulfilled promise should
be the number of milliseconds that was passed during the call to the `delay` function.

```js
const delay = ms => {
  // Your code
};

const logger = time => console.log(`Resolved after ${time}ms`);

// Function calls to check
delay(2000).then(logger); // Resolved after 2000ms
delay(1000).then(logger); // Resolved after 1000ms
delay(1500).then(logger); // Resolved after 1500ms
```

## Task 2

Rewrite the function `toggleUserState()` so that it does not use
the callback function `callback`, but accepts only two parameters `allUsers` and
`userName` and returned the promise.

```js
const users = [
  { name: 'Mango', active: true },
  { name: 'Poly', active: false },
  { name: 'Ajax', active: true },
  { name: 'Lux', active: false },
];

const toggleUserState = (allUsers, userName, callback) => {
  const updatedUsers = allUsers.map(user =>
    user.name === userName ? { ...user, active: !user.active } : user,
  );

  callback(updatedUsers);
};

const logger = updatedUsers => console.table(updatedUsers);

/*
* Currently works like this
 */
toggleUserState(users, 'Mango', logger);
toggleUserState(users, 'Lux', logger);

/*
* Should work like this
 */
toggleUserState(users, 'Mango').then(logger);
toggleUserState(users, 'Lux').then(logger);
```

## Task 3

Rewrite the function `makeTransaction()` so that it does not use
the callback functions `onSuccess` and `onError', but accepts only one parameter
`transaction` and returned a promise.

```js
const randomIntegerFromInterval = (min, max) => {
  return Math.floor(Math.random() * (max - min + 1) + min);
};

const makeTransaction = (transaction, onSuccess, onError) => {
  const delay = randomIntegerFromInterval(200, 500);

  setTimeout(() => {
    const canProcess = Math.random() > 0.3;

    if (canProcess) {
      onSuccess(transaction.id, delay);
    } else {
      onError(transaction.id);
    }
  }, delay);
};

const logSuccess = (id, time) => {
  console.log(`Transaction ${id} processed in ${time}ms`);
};

const logError = id => {
  console.warn(`Error processing transaction ${id}. Please try again later.`);
};

/*
 * Works like this
 */
makeTransaction({ id: 70, amount: 150 }, logSuccess, LogError);
makeTransaction({ id: 71, amount: 230 }, logSuccess, LogError);
makeTransaction({id: 72, amount: 75 }, logSuccess, LogError);
makeTransaction({ id: 73, amount: 100 }, logSuccess, LogError);
/*
* Should work like this
 */
makeTransaction({ id: 70, amount: 150 })
  .then(logSuccess)
  .catch(logError);

makeTransaction({ id: 71, amount: 230 })
  .then(logSuccess)
  .catch(logError);

makeTransaction({ id: 72, amount: 75 })
  .then(logSuccess)
  .catch(logError);

makeTransaction({ id: 73, amount: 100 })
  .then(logSuccess)
  .catch(logError);
```
