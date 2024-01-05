# Jest Doc

## 1 : Test structure
```js
describe('makePoniesPink', () => {
  beforeAll(() => {
    /- Runs before all tests -/
  })
  afterAll(() => {
    /- Runs after all tests -/
  })
  beforeEach(() => {
    /- Runs before each test -/
  })
  afterEach(() => {
    /- Runs after each test -/
  })

  test('make each pony pink', () => {
    const actual = fn(['Alice', 'Bob', 'Eve'])
    expect(actual).toEqual(['Pink Alice', 'Pink Bob', 'Pink Eve'])
  })
})
```

- Describe (‘name of the group’,()=>{ tests}): Describe is use to create group of related unit test cases.
- BeforeAll(()=>{}):  Callback content is executed before any of the test cases in test file run. This also holds the compiler in case of promise 
- BeforeEach(()=>{}): Callback content is executed before running each test.
- AfterAll(()=>{}): Callback content is executed after all the test runs in a test file run.
- AfterEach(()=>{}): Callback content block is executed after each test in a file test run.


## 2: Matchers

## 2.1 : Basic matchers
```js
expect(42).toBe(42) // Strict equality (===)
expect(42).not.toBe(3) // Strict equality (!==)
expect([1, 2]).toEqual([1, 2]) // Deep equality
expect({ a: undefined, b: 2 }).toEqual({ b: 2 }) // Deep equality
expect({ a: undefined, b: 2 }).not.toStrictEqual({ b: 2 }) // Strict equality (Jest 23+)
```
- Not: For making matchers login complimentary.
- ToBe(): For comparing primitive values or to check referential identity of object instances.
- ToEqual():  To check and compare recursively all properties of object instances (Known as DEEP equality)
- ToStrictEqual(): To compare the object have same structure and same type.

## 2.2 : Truthiness
```js
// Matches anything that an if statement treats as true (true, 1, 'hello', {}, [], 5.3)
expect('foo').toBeTruthy()
// Matches anything that an if statement treats as false (false, 0, '', null, undefined, NaN)
expect('').toBeFalsy()
// Matches only null
expect(null).toBeNull()
// Matches only undefined
expect(undefined).toBeUndefined()
// The opposite of toBeUndefined
expect(7).toBeDefined()
// Matches true or false
expect(true).toEqual(expect.any(Boolean))
```
- ToBeTruthy(): For checking if True
- ToBeFalsy(): For checking if False
- ToBeNull(): For checking if Null
- ToBeUndefined(): For checking if undefined.
- ToBeNaN(): Not a number
- ToMatch(): For matching regexp | string

## 2.3 : Numbers
```js
expect(2).toBeGreaterThan(1)
expect(1).toBeGreaterThanOrEqual(1)
expect(1).toBeLessThan(2)
expect(1).toBeLessThanOrEqual(1)
expect(0.2 + 0.1).toBeCloseTo(0.3, 5)
expect(NaN).toEqual(expect.any(Number))
```
- ToBeClose(): Approximate equality
- All matchers are self explanatory 


## 2.4: Strings
```js
expect('long string').toMatch('str')
expect('string').toEqual(expect.any(String))
expect('coffee').toMatch(/ff/)
expect('pizza').not.toMatch('coffee')
expect(['pizza', 'coffee']).toEqual([expect.stringContaining('zz'), expect.stringMatching(/ff/)])
```
- Self explanatory 

## 2.5 Objects
```js
expect({ a: 1 }).toHaveProperty('a')
expect({ a: 1 }).toHaveProperty('a', 1)
expect({ a: { b: 1 } }).toHaveProperty('a.b')
expect({ a: 1, b: 2 }).toMatchObject({ a: 1 })
expect({ a: 1, b: 2 }).toMatchObject({
  a: expect.any(Number),
  b: expect.any(Number),
})
expect([{ a: 1 }, { b: 2 }]).toEqual([
  expect.objectContaining({ a: expect.any(Number) }),
  expect.anything(),
])
```
- ToHaveProperty(): To check the following:
    - If a particular key is present in an object,
    - We can also check with key value pair.
    - For checking nested object ‘a.b’ internal example.
- ToMatchObject(): For comparing the complete object.

## 2.6 Expectations
```js
// const fn = () => { throw new Error('Out of cheese!') }
expect(fn).toThrow()
expect(fn).toThrow('Out of cheese')
expect(fn).toThrowErrorMatchingSnapshot()
```
- ToThrow(): To check if that function throws anything
- ToThrowErrorMatchingSnapshot(): To check if the function throws a particular error.


## 2.7 Mock functions
```js
// const fn = jest.fn()
// const fn = jest.fn().mockName('Unicorn') -- named mock, Jest 22+
expect(fn).toBeCalled() // Function was called
expect(fn).not.toBeCalled() // Function was -not- called
expect(fn).toHaveBeenCalledTimes(1) // Function was called only once
expect(fn).toBeCalledWith(arg1, arg2) // Any of calls was with these arguments
expect(fn).toHaveBeenLastCalledWith(arg1, arg2) // Last call was with these arguments
expect(fn).toHaveBeenNthCalledWith(callNumber, args) // Nth call was with these arguments (Jest 23+)
expect(fn).toHaveReturnedTimes(2) // Function was returned without throwing an error (Jest 23+)
expect(fn).toHaveReturnedWith(value) // Function returned a value (Jest 23+)
expect(fn).toHaveLastReturnedWith(value) // Last function call returned a value (Jest 23+)
expect(fn).toHaveNthReturnedWith(value) // Nth function call returned a value (Jest 23+)
expect(fn.mock.calls).toEqual([
  ['first', 'call', 'args'],
  ['second', 'call', 'args'],
]) // Multiple calls
expect(fn.mock.calls[0][0]).toBe(2) // fn.mock.calls[0][0] — the first argument of the first call
```

- ToBeCalled(): To check if a mock function is called in a flow test or not.
- ToHaveBeenCalledTimes(number): To check if a function is called n number of times.
- ToBeCalledWith(args): To check is any of the calls was with these arguments.
- ToHaveBeenLastCalledWith(arg1,arg2): To check with last call with certain arguments.
- ToHaveBeenNthCalledWith(call number, args): To check if function called on nth times with certain arguments.
- ToHaveReturnedTimes(2): To check if function returns value n number of time without throwing an error.
- And  Other are selfexplanatory in above examples

## 2.8 Miscellaneous 
```js
expect(new A()).toBeInstanceOf(A)
expect(() => {}).toEqual(expect.any(Function))
expect('pizza').toEqual(expect.anything())
```

## 2.9 Promise matcher
```js
test('resolve to lemon', () => {
  expect.assertions(1)
  // Make sure to add a return statement
  return expect(Promise.resolve('lemon')).resolves.toBe('lemon')
  return expect(Promise.reject('octopus')).rejects.toBeDefined()
  return expect(Promise.reject(Error('pizza'))).rejects.toThrow()
})
```


# 3 Async test
Important  things to consider while writing test for the asynchronous test.
```js
test('async test', () => {
  expect.assertions(3) // Exactly three assertions are called during a test
  // OR
  expect.hasAssertions() // At least one assertion is called during a test

  // Your async tests
})
```
We should always mention the times of assertion need to be called in async test so the if a number of test failed to execute then test should get failed.

**Note** :  that you can also do this per file, outside any describe and test:
```js
beforeEach(expect.hasAssertions)	
```



## 3.1  Async/await test
```js
test('async test', async () => {
  expect.assertions(1)
  const result = await runAsyncOperation()
  expect(result).toBe(true)
})
```
**Note**: 
- For all sync operations we should always use await function if some value is returned.


## 3.2 Promise
```js
test('async test', () => {
  expect.assertions(1)
  return runAsyncOperation().then((result) => {
    expect(result).toBe(true)
  })
})
```

- There is an another way for testing the async/await by returning a test promise using the *return* so that assertion not get failed.

### or

```js
test('resolve to lemon', async () => {
  expect.assertions(2)
  await expect(Promise.resolve('lemon')).resolves.toBe('lemon')
  await expect(Promise.resolve('lemon')).resolves.not.toBe('octopus')
})
```

```js
test('resolve to lemon', async () => {
  expect.assertions(2)
  await expect(Promise.resolve('lemon')).resolves.toBe('lemon')
  await expect(Promise.resolve('lemon')).resolves.not.toBe('octopus')
})
```

## 3.3 Done callback **
Wrap your assertions in try/catch block, otherwise Jest will ignore failures:
```js
test('async test', (done) => {
  expect.assertions(1)
  runAsyncOperation()
  setTimeout(() => {
    try {
      const result = getAsyncOperationResult()
      expect(result).toBe(true)
      done()
    } catch (err) {
      done.fail(err)
    }
  })
})
```


# 4 Mock
Jest mocking have multiple use cases:
- Mocking function that is passed as arguments in some component or another function basically callbacks function. We can check if a particular function is called, called n times, called with x,y arguments.
- Another usecase is to mock a library function and checking if that function is called n time, or with x,y arguments.
- One of the important usecase of this is to mock a network request/ http request/promise await etc.

### Example out of the box:
- [Checkout this video incase below thing doesn't make sense](https://www.youtube.com/watch?v=gA-uNj2FgdM)
- Testing a function which makes a http request without using the mock.
    ```js
    import axios from “axios”;
    function abc(){
        return axio.get(url).then((res)=>res.data.name).catch((error)=>console.log(err));}
        export default abc;
    ```
    ```js
    describe(‘abc’, ()=>{
        test(‘Getting results correctly’, async ()=>{
            const result = await abc();
            console.log(result)
            expect(result).toBe(“Amit Raikwar”)
            })
        })
    ```
- Now mocking a function http request:
    - ###1 Mocking whole axios.
        ```js 
        import mockaxios from ”axios”jest.mock("axios") 
        mockaxios.mockImplementation(()=>Promise.resolve({data:{name:”Mock Jedi”}}))
        describe(‘abc’, ()=>{
            test(‘Getting results correctly’, async ()=>{
                const result = await abc();
                console.log(result)
                expect(result).toBe(“Mock Jedi”)
                expect(mockAxios.get).toHaveBeenCalledTimes(1)})
        })
        ```
    - ### 2 Mocking get function.
        ```js
        import mockaxios from ”axios”jest.mock("axios")
        mockAxios.get.mockResolvedvalue({data:{name:”Mock Jedi”}});
        
        describe(‘abc’, ()=>{
            test(‘Getting results correctly’, async ()=>{
            const result = await abc();
            console.log(result)expect(result).toBe(“Mock Jedi”)
            expect(mockAxios.get).toHaveBeenCalledTimes(1)
            })
        })
        ```
    - ### 3 : Always clears the mock after each test run.
        ```js
        afterEach(()=>{
            Jest.clearAllMocks();
            });
        ```
    - ### 4: Passing a callback without mocking any library:
        ```js
        jest.mock(“axios” ,()=> {
             __esModule: true,
             default:{
                get: jest.fn().mockResolvedValue({data:{name: “Mock jedi”}})
                }
            } 
        )
        ```
## 4.1 Mock function
For mocking a function that have been passed.
```js
    test('call the callback', () => {
  const callback = jest.fn()
  fn(callback)
  expect(callback).toBeCalled()
  expect(callback.mock.calls[0][1].baz).toBe('pizza') // Second argument of the first call
  // Match the first and the last arguments but ignore the second argument
  expect(callback).toHaveBeenLastCalledWith('meal', expect.anything(), 'margarita')
})
```

## 4.2 Returning, resolving and rejecting values

- Your mocks can return values:
```js
const callback = jest.fn().mockReturnValue(true)
const callbackOnce = jest.fn().mockReturnValueOnce(true)
```
- Or resolve values:
```js
const promise = jest.fn().mockResolvedValue(true)
const promiseOnce = jest.fn().mockResolvedValueOnce(true)
```
- Mock function even can reject values.
```js
const failedPromise = jest.fn().mockRejectedValue('error')
const failedPromiseOnce = jest.fn().mockRejectedValueOnce('error')
```
- We can even combine these:
```js
const callback = jest.fn().mockReturnValueOnce(false).mockReturnValue(true)

// ->
//  call 1: false
//  call 2+: true
```

## Mock module using Jest.mock method
```js
jest.mock('lodash/memoize', () => (a) => a) // The original lodash/memoize should exist
jest.mock('lodash/memoize', () => (a) => a, { virtual: true }) // The original lodash/memoize isn’t required
```


## Mock object methods
-
```js
const spy = jest.spyOn(console, 'log').mockImplementation(() => {})
expect(console.log.mock.calls).toEqual([['dope'], ['nope']])
spy.mockRestore()
```
- 
```js
const spy = jest.spyOn(ajax, 'request').mockImplementation(() => Promise.resolve({ success: true }))
expect(spy).toHaveBeenCalled()
spy.mockRestore()
```

## Clearing and restoring mocks

- For one mock:
```js
fn.mockClear() // Clears mock usage date (fn.mock.calls, fn.mock.instances)
fn.mockReset() // Clears and removes any mocked return values or implementations
fn.mockRestore() // Resets and restores the initial implementation
```
- For all mocks:
```js
jest.clearAllMocks()
jest.resetAllMocks()
jest.restoreAllMocks()
```

## Accessing the original module when using mocks
```js
jest.mock('fs')
const fs = require('fs') // Mocked module
const fs = require.requireActual('fs') // Original module
```

## Timer mocks
Write synchronous test for code that uses native timer functions (setTimeout, setInterval, clearTimeout, clearInterval).

```js
// Enable fake timers
jest.useFakeTimers()

test('kill the time', () => {
  const callback = jest.fn()

  // Run some code that uses setTimeout or setInterval
  const actual = someFunctionThatUseTimers(callback)

  // Fast-forward until all timers have been executed
  jest.runAllTimers()

  // Check the results synchronously
  expect(callback).toHaveBeenCalledTimes(1)
})
```

- Or adjust timers by time with advanceTimersByTime():

```js 
// Enable fake timers
jest.useFakeTimers()

test('kill the time', () => {
  const callback = jest.fn()

  // Run some code that uses setTimeout or setInterval
  const actual = someFunctionThatUseTimers(callback)

  // Fast-forward for 250 ms
  jest.advanceTimersByTime(250)

  // Check the results synchronously
  expect(callback).toHaveBeenCalledTimes(1)
})
```
Use jest.runOnlyPendingTimers() for special cases.

Note: you should call jest.useFakeTimers() in your test case to use other fake timer methods.


## Skipping tests
- Don’t run these tests:
```js
describe.skip('makePoniesPink'...
tests.skip('make each pony pink'...
```
- Run only these tests:
```js
describe.only('makePoniesPink'...
tests.only('make each pony pink'...
```



















