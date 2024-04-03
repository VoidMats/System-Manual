
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
