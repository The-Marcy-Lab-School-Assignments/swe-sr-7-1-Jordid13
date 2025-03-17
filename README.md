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
