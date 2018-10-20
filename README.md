# TetrisClock
#### [DEMO](https://m-butler.github.io/TetrisClock/) - See Tetris Clock in action
![Demo Gif](https://github.com/m-butler/TetrisClock/blob/master/images/tetris.gif "Demo Gif")
---
### What Is It
###### A fully working clock that presents time using tetris pieces in a gamelike fashion

### How To Use It
###### Open `index.html` in your browser. All code is contained in this single file

### How Does It Work?
##### 1. Rendering Blocks
###### When the page loads, the blocks needed to display the time are all created. There are 4 sections that will each hold a digit of the current time. Each of these digits are made up of a 20 X 36 grid or 720 individual blocks. These blocks are then positioned at -50px on the Y axis to hide them from view. At this point there are a total of 2,880 blocks on screen.
##### 2. Positioning Blocks
###### Once the time is acquired for the current date, it is now time to display the desired digit on screen. using the function `createElement(digit, spot)` we can place corresponding blocks by passing in the number to be displayed and the spot in which to place it.
###### There are multiple steps that occur when `createElement` is called:
###### A. Number Identification
###### The entered digit information is retrieved from the `numbers` object. Each property of the object corresponds to a digit 0 - 9 and contains an array of arrays. This data is used to identify "outline coordinates" that will be used to draw a number without knowing each and every one of its 720 coordinates. In the image below, you can see that we only identified 24 coordinates to draw the number zero.
![Number Outline Coordinates Image](https://github.com/m-butler/TetrisClock/blob/master/images/grid.PNG "Number Outline Coordinates")
###### B. Randomly Choosing Blocks
###### Now that we have our outline coordinates for the desired number, it is time to choose which blocks will populate our digit and in what order. The `sectionPatterns` array contains arrays of arrays that hold 4 tetris block patterns (16 blocks) that can be used. In the image below we can see 10 predefined 4 block squares that are chosen at random to fill a portion of the digit.
![Block Patterns Image](https://github.com/m-butler/TetrisClock/blob/master/images/patterns.PNG "Block Patterns")
###### C. Correctly Placing 4 Block Squares
###### Now that we have identified our outline coordinates and randomly chosen a block pattern, we can start to display the digit. We do this by looping over each 16 block square, using the outline coordinate as a reference. This is achieved by differentiating the offset in relation to the outline coordinate and corresponding the resulting index with a position in the chosen section pattern. In the image below, you can see the order in which the 16 block square is transversed.
![Square Grid Image](https://github.com/m-butler/TetrisClock/blob/master/images/grid.PNG "Square Grid")
###### D. Visually Defining Tetris Blocks
###### Now that we know where to place the 4 block squares, we can start to visually define the tetris blocks. Each tetris block (which consists of 4 blocks) is given the same color. The color of a tetris block is the result of stepping through the `colors` object. For instance, if the last color used was red, we would then select green, then blue and so on. Once the color list is exhausted, we start back at the beginning. This gives the overall digit a more uniform assortment of colors.
###### E. Animating Tetris Blocks
###### Now that we have visually defined our tetris blocks for the digit, it is time to animate them from -50px on the Y axis, down to their respective position within the digit as a whole. This operation is achieved through a recursive function that utilizes the `setTimeout()` feature in Javascript. We use recursion so that at the end of each call a block can be returned to the document and moved on its own without waiting for the entire digit to finish. The 4 blocks that make up a digit move in unison because we don't call `setTimeout()` until a group of 4 has been returned to the document. This results in the entire group of 4 moving as one since they were essentially all waiting for their turn to update within the document itself. Another feature to note is how smooth each tetris piece moves. This was achieved by utilizing the `-webkit-transition: all 0.25s ease` feature in CSS and apply the other respective vendor prefixes for non-webkit browsers. Lastly, the tetris blocks fall into place in a way that mimics actual gameplay. This is achieved by ordering the section patterns from bottom to top blocks. In other words, if a particular block in the pattern would logically have to be placed before another, it would be included towards the beginning of the array and any other blocks that is proceeds.
##### 3. Wave Of White Effect
###### After all 4 digits have been fully created, they turn to white in a wavelike motion. This is achieved in 2 steps. First, we detect that all have digits have finished building by incrementing a variable until it reaches the total number of digits we have on screen. Once this variable reaches its max, we can then loop through each and every block per digit (starting with the rightmost digit) and fade it to white. The blocks for each digit are changed starting with the top right for the respective digit, going down, and then across. This order creates the illusion of a wave.
##### 4. Wipe, Rinse, Repeat
###### Once the page detects a change in time, all of the blocks are then wiped from the screen. To avoid heavy lifting for the browser, we are reusing all of these blocks. First, we move each block (yes, all 2,880) to -50px on the Y axis. Second, we "rinse" them of their color and return it to black. This allows for a fading effect when transitioning. Lastly, we repeat the `createElement` call and display the time. 
