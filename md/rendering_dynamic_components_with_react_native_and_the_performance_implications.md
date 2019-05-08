---
date: 2019-05-08T09:59:01-04:00
draft: false
title: Rendering dynamic components with React Native and the performance implications
---

I am currently working on a project that cries out for flexibility and dynamic functionality. Different builds require slightly different UI. 
Thankfully the same actions and business logic apply. 

My concern:

Because all builds are branched off of the same master branch, and each build could have slightly different UI any upstream bug fixes could 
potentially overwrite existing UI or cause conflicts. Both issues I would like to try to avoid.

I wanted to accomplish the above for developer frendliness while alsp keeping the app as performant as possible.

Aside: Generally when creating an app you want to aim for 60 frames per second. This means that the render time goal is 16ms or less.

My solution: 
Because my React Native app runs on Redux I could leverage the container / dumb component design pattern to ensure that no potential business 
logic was lost. I could also use this to potentially swap out the UI component based on a builds configuration.

i.e. The container would `connect` all the appropriate props and dispatch actions, then `render` the appropriate component for the build.

Step 1:
First I wanted to verify that it was possible to import a component outside of the standard `import component from ...` syntax with the 
`require` syntax, so that I could then verify that my desired pattern was even a possibility.

Assumption: Totally possible

```
render() {
  const listItemRowComponent = require('src/components/ListItemRowComponent1.tsx')
  return (<listItemRowComponent prop1={'value1'} prop2={'value2} />)
}
```

Step 2:
I refreshed my screen, and success! It worked! Whenever implementing a new pattern I always try to measure the performance implications. 

Now what?
The question I wanted to answer was what are the performance implications of requiring this component every time instead of simply importing 
it at the top of my class component.

Step 3:
Now I wanted to do was establish a performance baseline. How many milliseconds on average would it take to render my component when doing 
a standard `import`?

Step 4:
So I fired up Google Chrome's peformance tool, started the recording, navigated to the screen, waited for the load and scrolled down a bit.

I then investigated the results:

![Baseline](/images/base_line.png "Baseline")

At a glance, it looks as though the average rendering time is about 7ish ms. Fantastic! Under that desired 16ms we aim for.

I wonder what the [React Native Perf Monitor](https://facebook.github.io/react-native/docs/performance#what-you-need-to-know-about-frames) showed as well with regards to RAM usage and UI vs JS frames per second.

Before loading the list: 137ish MB, with 60 / 60 FPS.

Navigate to the List, load the 500 rows: 

 - Ram goes up to: 158ish MB
 - with scrolling, I see the UI thread go down to 58, but the JS stays consistent at 60. Great!

Step 5:
I then implemented the above `render()` function to see what, if any, performance implications 

Assumption: I won't see any dramatic differences.

![Require All The Time](/images/require_all_the_time.png "Require all the time")

There was a slight increase with this new method, but not dramatically so. I would say that it's acceptable.

One thing that I did notice however was that the Perf Monitor showed that the RAM usage went from about 158ish MB to 172ish MB. Not substantial, but not insigificant either.

Now I have two questions:

1. Obviously just requiring from a static string is no more flexible than doing an `import` from the top of the file. Can I use a variable?
1. Can I get that memory usage back down?

The answer to the first question seems pretty obvious at first blush. 

Simply add a configuration property and reference it.

``` 
// config.ts
export default {
  listItemRowComponent: 'src/components/ListItemRowComponent1.tsx'
}

// component.ts
render() {
  const listItemRowComponent = require(config.listItemRowComponent)
  return (<listItemRowComponent prop1={'value1'} prop2={'value2'} />)
}
```

Turns out RN isn't a big fan of this approach, and I got the Red Screen of Death.

Okay. I definitely wanted to be able to define which component to use in my config file. What do I do?

Step 6:
At this point I did what all developers do in this situation -- I Googled. It then pointed me to this wonderful [StackOverflow](https://stackoverflow.com/questions/33907218/react-native-use-variable-for-image-file) post.

Of course! Just `require` it directly into my config value:

``` config.ts
export default {
  listItemRowComponent: require('src/components/ListItemRowComponent1.tsx').default
}
```

Step 7:
Then simply render the `config.listItemRowComponent` as such:

``` component.ts
render() {
  return (<config.listItemRowComponent prop1={'value1'} prop2={'value2'} />)
}
```

And it works! Awesome. This makes me happy. It's configurable, extendable and manageable. Great 'ables' all around. And it's performant.

Taking a quick view of the performance snapshop I'm seeing render times of roughly 9ms for the component when referenced as such.

![Require in config](/images/require_config.png "Require in config")

Perf Monitor is also reporting that my RAM usage is comparable to the standard `import component from ...` syntax. 

Fantastic!

My conclusion: 

One big downside to this pattern is that you lose the ability to quickly navigate to a UI components implementation within VS code, directly linking between 
props and implementations.  You also lose the ability to refactor property types. Linters aren't going to catch any issues with the props either. 
This could be an issue if you use these tools as crutches. I know that I rely heavily on the refactor abilities (naming things is not easy).

Note that perf monitor, and the Google Chrome Performance recording isn't exact. The app was tested while in DEV mode, which means that the performance
will be worse than in prodiction, however my thoughts are that if I can get the app running well without any React optimizations, when those are in place
the app will run even smoother.