---
title: Optimizations with React.memo, useCallback, and useReducer.
date: "2019-03-09"
path: "/react-component-optimization/"
image: "img/react-component-optimization.jpg"
description: "Research and findings on React.memo, useCallback, and useReducer. How to use these tools and optimize React components."
featured: true
imagewidth: 920
imageheight: 503
---


With the recent release of React Hooks, I've switched over to using more and more functional components in my React code. While reading the React docs, I kept seeing mentions of `useReducer` being "more performant" than `useState`. I was unclear on why so I did a deep dive into the topic. After much research and experimentation, these are my findings.

It's pretty hard to demonstrate these concepts without a video, but hopefully this content makes sense. I recommend using the React Dev Tools and turning on `Highlight Updates` in the settings to test things out. It's also helpful to put a console.log on the Child component, if you see the console log, then it's being re-rendered.

**Our Scenario: A Parent component with a Child component that receives props from the Parent. Assume that both are functional components and the Parent is using `useState` to manage state.**

If the props being passed to the Child component are simple, the Child should be wrapped with `React.memo` to prevent re-renders if the props don't change. `React.memo` will automatically optimize the functional component and acts like the `shouldComponentUpdate` lifecycle method. Read more about [React.memo](https://reactjs.org/docs/react-api.html#reactmemo).

If the Parent component is passing a function (specifically, a function that updates the state of the Parent) down to the Child component, only using `React.memo` will not work. The function in the Parent component will need to be wrapped with the `useCallback` hook. This is because the function will be "re-rendered" every time the Parent re-renders, so the Child will always consider that function a new prop. Read more about [useCallback](https://reactjs.org/docs/hooks-reference.html#usecallback).

If the `useReducer` hook is used in the Parent component to manage state, then we won't have to worry about `useCallback`. `useReducer` will return a `dispatch` method that can be passed down to the Child component. The `dispatch` method won't be "re-rendered" every time the Parent re-renders. Read more about [useReducer](https://reactjs.org/docs/hooks-reference.html#usereducer).

When working with deeply nested Child components, it is recommended to use the `useReducer` hook in conjunction with React Context. You can pass the `dispatch` method down the tree with Context, which prevents prop drilling. [Read more](https://reactjs.org/docs/hooks-faq.html#how-to-avoid-passing-callbacks-down) about this pattern.