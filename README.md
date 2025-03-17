# Technical Writing Assignment

For guidance on setting up and submitting this assignment, refer to the Marcy lab School Docs How-To guide for [Working with Short Response and Coding Assignments](https://marcylabschool.gitbook.io/marcy-lab-school-docs/fullstack-curriculum/how-tos/working-with-assignments#how-to-work-on-assignments).

## Prompt 1

In your own words, explain why React is a popular choice for building user interfaces. Make sure to mention at least one benefit, such as how it simplifies development, supports reusable components, or helps optimize performance. Feel free to include any specific features you find particularly helpful.

### Response 1

React is a popular choice for building user interfaces because it simplifies development through reusable components. Components allow developers to create modular, self-contained UI elements that can be easily reused across an application, improving code organization, readability, and maintainability.

For example, consider a scenario where we need to create a list of video cards. In vanilla JavaScript, this process involves manually creating and appending elements:

```js
const videosList = document.createElement("ul");

const videoCard = (title, thumbnailLink) => {
  const videoContainer = document.createElement("div");
  const videoTitle = document.createTextNode(title);
  const thumbnail = document.createElement("img");
  thumbnail.src = thumbnailLink;

  videoContainer.append(thumbnail, videoTitle);
  return videoContainer;
};

const newVideo = videoCard("Hello World", "https://...");
videosList.append(newVideo);
```

This approach requires repeatedly manipulating the DOM and manually structuring the UI. In contrast, React simplifies this with reusable components:

```js
const VideoCard = ({ title, thumbnailLink }) => (
  return <>
    <h1>{title}</h1>
    <img src={thumbnailLink} alt={title} />
  </>
);

const VideoList = () => (
  return <ul>
    <VideoCard title="Hello World" thumbnailLink="https://..." />
  </ul>
);
```

With React, our code is more readable and maintainable. Each component clearly defines its purpose. The VideoCard component encapsulates a single video’s UI, while VideoList organizes multiple video cards. This modular approach reduces redundancy and improves development efficiency, making React a powerful tool for building scalable interfaces.

## Prompt 2

Explain how the useState hook is used in React to manage state within functional components. In your response, include an example of how useState might be used in a simple application and why managing state is important in building interactive user interfaces.

### Response 2

In order to give more control to rendering components, `useState` is one of many solutions that React came up with. What it does is that you can define variables using that same method which gives you access both to the value or reference itself in addition to a function that is meant to explicity dictate change that should accompany a re-render.

The simplest example of `useState`'s application would be a counter (with its corresponding 'update' button). It would look something like this:

```javascript
function App({}) {
  const [value, setValue] = useState(0);

  return (
    <div>
      <h1>{value}</h1>
      <button onClick={() => setValue(value+1)} />
    </div>
  )
}
```

If `useState` was not used, the value would still be updated, but it will not reflect on the render! This is because React has special hooks (`useState` included) which signal components when they should do so. This makes it so that the user can be guaranteed that they are getting the 'freshest' information available.

## Prompt 3

Describe the different ways the useEffect hook can be triggered in a React component. Include an explanation of how the dependency array influences its behavior. If possible, provide a code example for each scenario to illustrate your explanation.

### Response 3

In React, `useEffect` is a hook used to handle side effects in functional components. These side effects can include tasks such as fetching data from an API, setting up event listeners, or updating the document title.

The execution of `useEffect` depends on its dependency array, an optional argument that determines when the effect should run. The dependency array allows React to track specific values and re-run the effect only when those values change.

Here are the different ways `useEffect` can be triggered:

### 1. No Dependency Array (Runs on Every Render)

If `useEffect` is used **without** a dependency array, the effect runs **on every re-render** of the component. This can be inefficient, especially if the effect is computationally expensive.

```js
useEffect(() => {
  console.log("Effect runs on every render");
});
```

### 2. Empty Dependency Array (Runs Only on Mount)

When an **empty dependency array `[]`** is provided, the effect **runs only once**, when the component mounts. This is useful for one-time operations like initializing data or setting up subscriptions.

```js
useEffect(() => {
  console.log("Effect runs only on mount");
}, []);
```

### 3. Dependency Array with Values (Runs When Dependencies Change)

If specific values are included in the dependency array, the effect **runs only when those values change**. React monitors the dependencies and re-runs the effect whenever one of them updates.

```js
useEffect(() => {
  console.log("Effect runs when 'value' changes");
}, [value]);
```

This approach is commonly used for responding to state or prop changes, such as fetching new data when a selected item changes.

By understanding how the dependency array works, developers can optimize component behavior and avoid unnecessary re-renders or unwanted side effects.

## Prompt 4

The component below makes a mistake when using useEffect. When running this code, we will get an error from React! Please fix this code.

```js
const DogDisplay = () => {
  const [imgSrc, setImgSrc] = useState(
    "https://images.dog.ceo/breeds/hound-english/n02089973_612.jpg"
  );

  useEffect(async () => {
    try {
      const response = await fetch("https://dog.ceo/api/breeds/image/random");
      if (!response.ok) throw new Error(`Error: ${response.status}`);
      const data = await response.json();
      setImgSrc(data.message);
    } catch (error) {
      console.error(error);
    }
  }, []);

  return <img src={imgSrc} />;
};
```

After fixing the code provide and explanation to what you fixed and why it needed to be fixed.

### Response 4

The very first error is that `useEffect` is only allowed to return a function. This is implicit with the syntax, since any callback `() => {}` returns an anonymous function by default (unless it has a `return` statement inside). `useEffect` from a functional standpoint should never be asynchronous itself to prevent race conditions where it is a possibility that the component unmounts before `useEffect` finishes executing. Putting the `async` keyword to any function makes it so that it returns not just a regular `Function`, but an `AsyncFunction`.

So let's fix that:

```js
  useEffect(() => { // made the callback synchronous by removing `async`
    const fetchImg = async() => { // created an asynchronous function inside that is called immediately
      try {
        const response = await fetch('https://dog.ceo/api/breeds/image/random');
        if (!response.ok) throw new Error(`Error: ${response.status}`)
        const data = await response.json();
        setImgSrc(data.message);
      } catch (error) {
        console.error(error);
      }
    }
    fetchImg(); // the immediate call
  }, []);
}
```

There's nothing really functionally incorrect after that, but one improvement could be that `imgSrc` could be null or blank at the beginning, and only have the fallback render in case there's an error with the fetches. This implies a conditional loading render that we will render until the fetch completes. That entails these changes:

```javascript
const App = () => {
  const [imgSrc, setImgSrc] = useState(null);
  const [isLoading, setIsLoading] = useState(null); // a new load state invoke a re-render whenever it changes

  useEffect(() => {
    const fetchImg = async() => {
      setIsLoading(true); // set loading to true while in the asynchronous function
      try {
        const response = await fetch('https://dog.ceo/api/breeds/image/random');
        if (!response.ok) throw new Error(`Error: ${response.status}`)
        const data = await response.json();
        setImgSrc(data.message);
      } catch (error) {
        console.error(error);
        setImgSrc('https://images.dog.ceo/breeds/hound-english/n02089973_612.jpg');
      } finally {
        setIsLoading(false); // finally, set isLoading to false to re-render the component
      }
    }
    fetchImg();
  }, []);

  return (
    <>
      {/* We conditionally render either a loading message or the img once it's ready */}
      { isLoading ? <h1>Loading...</h1> : <img src={imgSrc} /> }
    </>
  )
}
```
