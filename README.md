# Reinforcement Learning to play Dino Run
I used reinforcement learning to teach the model to control the dino agent from Chrome's offline game, Dino Run, by detecting and jumping over obstacles.

## Dependencies
- Python 3.6
- Selenium
- OpenCV
- PIL
- Keras
- Chromium driver for Selenium

## Modifying the Original Game for Faster Learning on CPU-Only System
I made several modifications to the original game to facilitate faster learning on a CPU-only system. Here are the key modifications:
- **Acceleration**: The speed of the agent stays constant throughout the gameplay (ACCELERATION = 0).
- **Types of Obstacles**: Limited to 1, and I have trained the model only for a single type of obstacle (cactus) with a fixed size.
- **Game Start Time**: Reduced to around 400ms. Originally, each game would take 1 sec to start, but it was reduced for faster learning.
- **Jump Velocity**: Increased because when the dino is mid-air, there are no actions or features to be learned, so it stays mid-air for a shorter period of time.
- **Game Over Panel**: Removed as it is not a feature to be learned. Selenium interface is used to detect game over and restart.
- **High Score Panel**: Removed as we are not maintaining a high score on the game panel.

## Selenium as Interface Between the Model and Game
As the game is browser-based and the model is built in Python, we need an interface through which we can observe the game environment and send actions to the agent to play the game. We use Selenium to control the browser and send actions to the agent.

## OpenCV & PIL for Image Capturing and Pre-processing
To acquire the video of gameplay, I used PIL and OpenCV to capture and process sequential screenshots from the screen, which were then fed through the model for training as well as playing.

## Path Variables
We have two external dependencies:
1. The modified version of the game (web app).
2. The executable of chromedriver for Selenium. This is not included in the repo as it is system-dependent and needs to be placed one level above the project folder.

## Game Module
This is the main module that implements interfacing between Python and browser-JavaScript using Selenium.

## Agent Module
This module represents the agent (Dino) which the model controls for playing.

## Game State Module
The game state helps get the current state of the game environment as well as the agent. Actions are performed by this model before getting a new state.

## Utilities
- `save_obj()` and `load_obj()`: Used to preserve the state of the game in the file system.
- `grab_screen()`: Captures the screen and the bounding box for locating the region of interest.
- `process_img()`: Performs necessary image transformations before sending it to the model.
- `show_img()`: Coroutine implementation to observe the images sent to the model.

## Initialize Log Structures
Initialize log structures from files if they exist; otherwise, create new ones.

## Module Parameters
Define various parameters for the game and training.

## Building the Model
The input to our model is a tensor: 4 stacked images of dimensions 40x20 = 40x20x4. Each action has its own output (2 outputs): the Q-value for each action.

## Main Training Module
This module contains the main training loop and the Deep Q-Network algorithm.

## Main Function
Initialize the game module, launch the browser, initialize the agent module, initialize the game state module, build the Keras model, and start the training loop.

### Results
- Loss and Scores: Trained the model for around 2 million frames over a week. The first million steps were used for fine-tuning the game parameters and fixing bugs. Plotting the last million loss values and all scores recorded for all games. The last million training frames showed improvement in game scores, reaching a maximum score of 265.
- Action Distribution: The density distribution of the actions performed by the model.
- Plotting the game training logs.

## Working Demo
https://github.com/ishan-1010/PROJECT_2__T-Rex_Dino_Game_AI/assets/98383932/d3155b05-1aa5-47ed-9751-c4c5ddbb0075
