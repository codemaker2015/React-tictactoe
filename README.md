# Using React in web games

## Prerequisites

* Node.js and NPM installed on your machine
* Basic knowledge of JavaScript and how React works
* Begin by installing the official React application scaffold tool, create-react-app:

`npm install -g create-react-app`

You can verify the installation by running create-react-app this should ask you to specify the directory name.

## Using React in our game
React allows us to create reusable components, and as a result, we can build units or components of our application that can be reused across various parts of our application. Leveraging this, we will be building the following components:

* Box component — this will be the box component for our game, it’ll be the unit that handles our tic-tac-toe objects
* Game component — this component handles the game logic such as the “game over” and “reset” to the initial state functions
* Layout component — this component handles the layout of the game. We will be using the CSS grid to build our layout

## Getting started
We will be using create-react-app to bootstrap our project, create-react-app is an open-source tool that is maintained by Facebook and the community to help developers start a react project in a very short time.
To create a new project using the create-react-app boilerplate, run the command in your preferred terminal:

`create-react-app react-tictactoe`

The name “react-tictactoe” is used as the project name for this tutorial, it can be replaced with whatever name you choose to use.
Next, change into your project directory and start your development server by running:

`cd react-tictactoe && npm start`

The command opens up a browser tap rendering the default boilerplate application.

## Building game components
We’ll start off with the box component.

## 1. Building the box component
In our src folder, let’s create a component folder, and inside the component folder, create a file called Box.js. Note that the filename can be whatever you want, however you see fit.

Next, we create a functional component with value and onClick properties, this component will return a button tag with these properties destructured:
```
import React from 'react';
function Box({ value, onClick }) {
    return (
        <button
            style={style}
            onClick={onClick}>
            {value}
        </button>
    );
}
export default Box;
```

In the code block above, our Box component accepts a value and onClick prop. This is because our application will have a box-like shape with 3 rows and 3 columns and each box will be clicked on to reveal a value. Before proceeding to refresh our browser, let’s write the style as we’ve included it in the Box component:
```
const style = {
    background: '#fff',
    border: '2px solid lightblue',
    fontSize: '30px',
    fontWeight: '800',
    cursor: 'pointer',
    outline: 'none'
}
```

## 2. Building the layout component
In this component, we will import our Box component and use it as the backbone to create a stable user interface for our game, for this we will need to create a functional component similar to the one we have in our Box component, after which we will pass boxes and onClick as props to the function, next, we will map through our Box component to create 3 rows and 3 columns:
```
import React from 'react';
import Box from './Box'

function Layout({boxes, onClick }) {
    return (
      <div style={style}>
        {boxes.map((box, i) => (
          <Box key={i} value={box} onClick={() => onClick(i)} />
      ))}
      </div>
    );
}
export default Layout;
```
In the code block above, we are mapping through the boxes in our Box component until we have 3 rows and columns for our apps, in the code above, we’ve not added our required number of boxes, to do that we add a style object to our component:
```
const style = {
  border: '4px solid lightblue',
  borderRadius: '10px',
  width: '320px',
  height: '320px',
  margin: '0 auto',
  display: 'grid',
  gridTemplate: 'repeat(3, 1fr) / repeat(3, 1fr)'
};
```
In our style above, we added a grid template to dictate how many rows and columns we want in our application, in our case we want to have three rows and columns.

## 3. The game component
This component is the principal component of our game application, here we are going to build most of the logic for our game, including declaring a winner.

We’ll start off by importing a function to check our Layout component, which we will be using later on, to handle clicks by a user. Next, we’ll add a div tag and inside of it we will add a paragraph to display our winners, soon we will add a function that helps us determine that automatically:
```
import React, { useState } from 'react';
import Layout from './Layout';

const styles = {
    width: '200px',
    margin: '20px auto',
};
const pStyle = {
    color: 'green'
}

function Game() {
    return (
        <React.Fragment>
            <Layout boxes={layout} />
            <div style={styles}>
                <p style={pStyle}> The winner goes here
              </p>        
            </div>
        </React.Fragment>
    )
}
export default Game;
```
In the code above, our Game component is wrapped in a React.Fragment. Next, we added our Layout component as props and a p tag that’d display winner.

The next thing for us is to add a state to our application and an event listener to our component. This tells us when we have a winner, then it displays the p tag to declare our winner:
```
function Game() {
    const [layout, setLayout] = useState(Array(9).fill(null));
    const [xIsNext, setXisNext] = useState(true);
    const winner = checkWinner(layout)

    const handleClick = (i) => {
        const layoutState = [...layout];
        if (winner || layoutState[i]) return;
        layoutState[i] = xIsNext ? 'X' : 'O';
        setLayout(layoutState);
        setXisNext(!xIsNext);
    }

    return (
            <React.Fragment>
            <Layout boxes={layout} onClick={handleClick} />
            <div style={styles}>
                <p style={pStyle}>
                    {winner ? 'Winner: ' + winner : 'Next Player '
                    + (xIsNext ? 'X' : 'O')}
                </p>

            </div>
        </React.Fragment>
            )
}
export default Game;
```
In the code block above, we are using the useState hook to create a state for our layout and using the array of boxes. Next, we set a state, xIsNext that tells us the next letter to click on during the game.

The last variable winner will prevent a click on a box once a winner is found in the game. The handleClick function takes in a copied version of our layoutState instead of directly mutating our state, the layoutState returns an empty set.

Next, if an empty box is clicked then the xIsNext state renders an X or O in the empty box and sets it as the layout state.

## 4. Building the game logic
This component is the last component we will work on before our game is complete. Here, we will be using an exported function that calculates the winner of the game:
```
export function checkWinner(boxes) {
 const lines = [
   [0, 1, 2],
   [3, 4, 5],
   [6, 7, 8],
   [0, 3, 6],
   [1, 4, 7],
   [2, 5, 8],
   [0, 4, 8],
   [2, 4, 6],
 ];
 for (let i = 0; i < lines.length; i++) {
   const [x, y, z] = lines[i];
   if (boxes[x] && boxes[x] === boxes[y] && boxes[x] === boxes[z]) {
     return boxes[x];
   }
 }
 return null;
}
const boxes = [null, null, null, "X", "X", "O", null, null, null];
console.log(checkWinner(boxes));
```
In the code block above, we write a function called checkWinner that checks for a winner, this function takes boxes as an argument. Next, we created a lookup array that contains all of the winning moves, counting from 0 to 8 so instead of looping through a box, we will loop through the lookup array. Next, we used the ES6 syntax to destructure the winning boxes and give it default letters x, y, z then we check if we have an X or an O in a roll. If we do, we continue on that path, else the next letter will be the opposite of the one we got. Otherwise, we check if the first value matches the second value and if they both match the third value then we have a winner, if none of this is done, then we return null — meaning we don’t have a winner.

At the end of this, our project should look like this:

![Screenshot](https://github.com/codemaker2015/React-tictactoe/blob/master/src/assets/Screenshot.png)