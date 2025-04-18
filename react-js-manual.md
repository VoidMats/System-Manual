
## React

* [Use async functions in useEffects](#use-async-functions-in-useeffect)
* [Adding AbortController to async function in useEffect](#adding-abortcontroller-to-async-function-in-useeffect)
* [Display component according to a property](#display-component-according-to-a-property)

### npm 

Install a new React project outside 

### Use async functions in useEffect
Write the async function inside **useEffect** itself. Make sure to set the data *inside* the async function. 
```
useEffect(() => {
    // Declare the async function
    const fetchData = async () => {
        const response = await fetch("https://some/api.com");
        const data = await response.json();
        setData(data);
    }

    // Call the async function
    fetchData().catch(error => console.error(error));
}, []);
```
Don't write it like this. This will lead to a Promise {pending} answer, if the fetchData function will take to long time. 
```
useEffect(() => {
    // Define the async function
    const fetchData = async () => {
        const response = await fetch("https://some/api.com");
        return await response.json();
    }

    // Call the async function
    const result = fetchData().catch(error => console.error(error));
    // Set data
    setData(result);
}, []);
```
If you want to declare the async function outside the **useEffect** function, it has to be done by using useCallback. 
```
// Declare the async function somewhere else.
const fetchData = useCallback(async () => {
    const response = await fetch("https://some/api.com");
    const data = await response.json();
    setData(data);
}, []);

useEffect(() => {
    fetchData().catch(error => console.error(error));
}, [fetchData])
```
When using parameters, it becomes a littlebit more complicated. 
```
useEffect(() => {
    // Set state 
    let getData = true;

    // Define the async function
    const fetchData = async () => {
        const response = await fetch(`https://some/api.com?param=${param}`);
        const data = await response.json();
        // Set state if getData is true
        if (getData) {
            setData(data);
        }
    }

    // Call the async function
    fetchData().catch(error => console.error(error));
    // Cancel any future data request
    return () => getData = false;
}, [param]);
```
To read more [Devtrium - How to use async functions in useEffect](https://devtrium.com/posts/async-functions-useeffect)

### Adding AbortController to async function in useEffect
An *AbortController* is a mechanic where the user or event could abort an ongoing asynchronous task. This create a better resource management, where for example a request could be aborted if user navigate quickly through the page. It could also in some cases avoid race conditions. The interface consist of *AbortController* and *AbortSignal*. 
```
useEffect(() => {
    // Set state 
    const controller = new AbortController();
    const signal = controller.signal;
    let getData = true;

    // Define the async function
    const fetchData = async () => {
        const response = await fetch(`https://some/api.com?param=${param}`, { signal });
        const data = await response.json();
        // Set state if getData is true
        if (getData) {
            setData(data);
        }
    }

    // Call the async function
    fetchData().catch(error => {
        if (error.name === "AbortError") {
            console.log("Request aborted);
        } else {
            console.error(error);
        }
    });

    // Cancel any future data request and abort any fetch during unmount
    return () => {
        getData = false;
        controller.abort();
    }
}, [param]);

```
### Display component according to a property
A very common way to display a dialog is to use a open property. This could however be added to any component. Example
```
export default function SomeComponent({ open }) {

    return (
        <Box>
            {(open) && <TextField id="Some id" variant="Outlined" />}
        </Box>
    )
}
```

### Check performance 
performance.memory.jsHeapSizeLimit; // will give you the JS heap size
performance.memory.usedJSHeapSize; // how much you're currently using

## Javascript
Below there will be some small tips when programming in javascript. Most of the "tips" are general and probably only 
used under certain situations. 

* [Create an Array with increment numbers](#create-an-array-with-increment-numbers)
* [Local Storage](#local-storage)
* [Session Storage](#session-storage)
* [IndexDB](#indexdb)

### Create an Array with increment numbers
```
Array.from(Array(10).keys());                   # [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
[...Array(10).keys()]                           # [0, 1, 2, 3, 4, 5, 6, 7, 8, 9] Using spread operator
Array.from({length: 10}, (_, i) => i + 1)       # Possible to set the start index
```

### Local Storage
Local storage is a synchronous storage which is could be used on the client side for key-value solutions. The storage
limit is about 5Mb per domain. The data could persist in the browser even if it is closed. The ideal data to store is 
local user data which is only used by the browser, ex user preferences. 

### Session Storage
Session storage work the same way as for Local Storage, but with big difference that the data will be lost when the 
session is closed. This mean also that if a new browser is open session storage is not shared between difference sessions. 

### IndexDB
For storage of large amount of data IndexDB has to be used. It is asynchronous solution made to store structured data, 
files, blobs or other complex structured objects. 





