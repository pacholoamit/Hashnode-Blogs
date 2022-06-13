## 3 Essential tips for front-end developers to get started on the back-end

Most developers usually start on the front-end which is understandable, considering that front-end development is more visual. It's easier to understand front-end code from a beginner's perspective than back-end code but it gets to a point where most developers, even though they are really experienced on the front-end, have this horror or feeling of dread when approaching back-end development. 

It's okay, we've all been there and **I'm going to help you by give you 3 essential tips to help you transition your learning from front-end to the back-end**.

# What is back-end development? 🤔

 ![Thinking](https://media.giphy.com/media/lKXEBR8m1jWso/giphy.gif) 

I'm going to give you a brief summary of what back-end development is about. I'm sure the people reading have a rough idea of what it is but to formalize it:


> Back end development refers to the server side of an application. The back end usually consists of three parts: a server, an application, and a database.
- Lauren Stewart, Course Report

To help with communication, I'm pretty sure you've made GET requests with javascript (perhaps even with axios.) If you are not familiar on making GET requests here is a code snippet below on how it looks.

Vanilla Javascript Fetch:
```
fetch('https://jsonplaceholder.typicode.com/todos/1')
  .then(response => response.json())
  .then(json => console.log(json))
``` 
Axios Fetch:
```
axios.get('https://jsonplaceholder.typicode.com/todos/1')
   .then(response => console.log(response))
   .catch(error => console.log(error))
```

I much prefer axios since you get the same effect but you write less code but that's how  to make your first ever GET request.

## What happens on a GET request? 💡
When you perform a GET request you basically just retrieve a request from a server. In the example above, we are fetching from a URL from jsonplaceholder where it would return a set amount of todos based on the query string parameter (in this case, 1 todo.)

I'd really encourage everyone to use jsonplaceholder for testing making GET requests as you can customize the type of response you'd want returned without constraints.

So, now you understand how to make a GET request, here are 3 essential tips that would help you on your back-end journey:

## 1. Understand the basics: 🧐
 ![Studying](https://media.giphy.com/media/fhAwk4DnqNgw8/giphy.gif) 

The first thing you need to do is watch a 20 minute YouTube video on what are the components that are apart of the back-end. I'll give a brief description:

- The Server - This is the computer that receives requests. 
(Any computer can be a server, when you run "npm run serve" or "npm run start" you are, in effect, creating a dev server for your front-end application to run on)

- The Database - This is where data is organized and stored
(I.E. User information from you facebook profile is residing in the facebook database.)

- The App - This is the application running on the server that listens for requests, retrieves information from the database, and sends a response. (I.E. jsonplaceholder data is the app built to process request based on query string parameters)

Once you understand the basics, move on to:

## 2. Pick a back-end framework that has the most amount of learning resources OR is widely used in the job market around you: 👉
 ![Framework](https://media.giphy.com/media/ny7UCd6JETnmE/giphy.gif) 

This is the important part, Either pick a backend framework that relies on one of these two criteria. The first criteria is important but it won't matter if it won't get you hired which is why there is a second criteria. The ideal is to aim for both but I will leave that to your discretion.

### How do you learn?
To expand more on the first criteria, If you know yourself well enough, you should know the best ways for you to learn. Do you learn better through visual (YouTube, udemy, etc.) learning? Do you learn better on reading documentation? etc.

 Picking a back-end framework that has a lot of learning resources gives you a wider range of learning instruments that fits the way you learn in the way you are accustomed to and therefore speeds up the learning process. 

The second criteria focuses on the job market around you, If you're learning Express/NodeJS for the backend but most of the jobs around you are using Laravel PHP then you'll get beaten by another candidate that already knows Laravel. So, aside from considering the learning resources, also consider your job market.

## 3. If it gets boring, take a break and go back to it later: 😒
 ![stuck](https://media.giphy.com/media/uj8SgKD1Pzxjlwo4Sg/giphy.gif) 

The important thing to get out from this is just starting. Let me tell you, working on the back-end is the most unthrilling and boring thing I have ever done in my life BUT it is absolutely necessary to build application where you can store user data (or any sort of data) to a database. 

If you only made it far enough to install the NPM package or YARN package then good for you, you're one step closer to conquering your fear of back-end development.


> Eighty percent of success is showing up
- Woody Allen


I've heard myths and legends of developers having more fun in the back-end than the front-end and I haven't found any in my area so I guess it's still a myth or legend.

## Thanks for reading my post! Go make some back-end applications NOW🎉🎉🎉









