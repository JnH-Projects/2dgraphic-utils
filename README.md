# 2D Graphics Util

> Created by: [Jaymar921](https://github.com/jaymar921)

A lightweight 2-Dimensional graphic utility that can be used for website or game development. Wraps all renderable images inside a `CanvasScreen` object.

## Installation

```bash
npm install @jaymar921/2dgraphic-utils
```

## Classes and Their Descriptions

| **Class**        | **Description**                                                                                                                                                                        |
| ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **CanvasScreen** | A wrapper class around the `<canvas>` element that manages sprites and their rendering. It handles screen updates, sprite registrations, and interactions like mouse clicks and drags. |
| **Sprite**       | Represents an image (or animation) that can be drawn onto the canvas. It has properties for position, size, animation frames, and more.                                                |
| **SpriteType**   | A predefined enum that categorizes different types of sprites, such as `OBJECT`, `PLAYER`, `BLOCK`, etc.                                                                               |

## CanvasScreen Class

| **Function**                                       | **Description**                                                                                                                                                                |
| -------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `constructor(canvasId, width, height, background)` | Initializes a `CanvasScreen` object, binding it to an existing canvas element in the DOM by its ID. Optional parameters allow for setting width, height, and background color. |
| `registerObject(spriteObj)`                        | Registers a `Sprite` object for rendering on the canvas. Order of registration matters, as later objects will overlap earlier ones.                                            |
| `unregisterObject(objID)`                          | Removes a `Sprite` object from the screen by its `objID`.                                                                                                                      |
| `getRegisteredObject(objID)`                       | Retrieves a `Sprite` object by its `objID`. Returns `null` if not found.                                                                                                       |
| `handleScreenClickedEvent(callbackFunc)`           | Registers a callback function to be triggered when the screen is clicked or touched.                                                                                           |
| `enableScreenDrag(arg)`                            | Enables or disables dragging (panning) of the canvas using mouse events.                                                                                                       |
| `static animate()`                                 | Responsible for continuously updating the canvas by clearing the screen and redrawing all registered sprites. This function runs on each frame to animate objects.             |

### CanvasScreen Class: Detailed Explanation

#### Constructor (`constructor`)

- Initializes a new `CanvasScreen` object by binding to an existing canvas element. If no canvas is found, it throws an error. It also accepts width, height, and background color, setting default values if not provided.

#### `registerObject(spriteObj)`

- Adds a sprite to the canvas. The sprite will be rendered on each animation frame. The order of registration matters, as new sprites will be drawn on top of old ones.

#### `unregisterObject(objID)`

- Removes a sprite from the canvas by its unique `objID`.

#### `getRegisteredObject(objID)`

- Searches for a sprite by its `objID` and returns it, or `null` if not found.

#### `handleScreenClickedEvent(callbackFunc)`

- Registers a callback to handle click or touch events. The callback receives the event and the current `CanvasScreen` object as arguments.

#### `enableScreenDrag(arg)`

- Enables or disables the ability to drag (move) the canvas. This could be used for creating a "camera" movement effect.

#### Static `animate()`

- A static function that continuously re-renders the screen. It clears the canvas and redraws all registered sprites in their current states.

## Sprite Class

| **Function**                                                                                                                                              | **Description**                                                                                                                                                                                                                                |
| --------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `constructor({objID, name, posX, posY, width, height, imageSource, animations, frames, frameBuffer, loop, autoPlay, scale, imageSmoothingEnabled, type})` | Initializes a `Sprite` object with properties like position, size, image source, and animation settings. Important Note: The image source that was tested is of file type `png` and `jpeg`. Other file types might cause some rendering issue. |
| `draw(context)`                                                                                                                                           | Draws the sprite on the canvas at its current position, using the current frame if it's an animated sprite.                                                                                                                                    |
| `update(context)`                                                                                                                                         | Calls `draw()` to render the sprite.                                                                                                                                                                                                           |
| `play()`                                                                                                                                                  | Starts the animation of the sprite if it was paused.                                                                                                                                                                                           |
| `updateFrames()`                                                                                                                                          | Updates the current frame of the sprite based on the frame buffer. Handles looping and auto-playing logic.                                                                                                                                     |

### Sprite Class: Detailed Explanation

#### Constructor (`constructor`)

- Creates a new sprite, loading an image and setting its properties such as position, size, animation settings, etc. The sprite can be an animated object or just a static image.

#### `draw(context)`

- Renders the sprite on the canvas at its current position. If the sprite has animations, it will draw the current frame of the animation.

#### `update(context)`

- Calls `draw()` to update the sprite's rendering on the canvas.

#### `play()`

- Starts the sprite's animation (if it was paused or stopped).

#### `updateFrames()`

- Handles the animation logic, updating the current frame based on the `frameBuffer` value. It also supports looping.

### Important Note: Sprite Class

- You'll only need to create an instance of the class and register it in the `CanvasScreen` as it will do all the rendering function.

## SpriteType Enum

| **SpriteType** | **Description**                                   |
| -------------- | ------------------------------------------------- |
| `OBJECT`       | Represents a general object.                      |
| `PLAYER`       | Represents a player character.                    |
| `BACKGROUND`   | Represents background elements in the game/scene. |
| `FLUID`        | Represents fluid objects, such as water.          |
| `PASSABLE`     | Represents objects that can be passed through.    |
| `ITEM`         | Represents collectible items in a game.           |
| `BLOCK`        | Represents solid, non-passable objects.           |
| `AIR`          | Represents air or an empty space.                 |

## Short Examples of Usage

### Example 1: Basic Setup and Sprite Registration

```javascript
import { CanvasScreen } from "@jaymar921/2dgraphic-utils";
import { Sprite } from "@jaymar921/2dgraphic-utils";

const canvas = new CanvasScreen("myCanvas"); // Assuming there's a <canvas id="myCanvas"></canvas> in the HTML

const playerSprite = new Sprite({
  objID: "player1",
  name: "Player 1",
  posX: 50,
  posY: 50,
  width: 32,
  height: 32,
  imageSource: "player-sprite.png", // Path to your image
  type: SpriteType.PLAYER, // Classify this Sprite object to a type "Player" | default: "Object"
  scale: 1,
});

canvas.registerObject(playerSprite); // It will automatically render the playerSprite
```

### Example 2: Handling Mouse Click Events

```javascript
canvas.handleScreenClickedEvent((e) => {
  const objID = e.objID; // objID of the Sprite that was clicked or touched
  const spriteType = e.type; // The SpriteType of the clicked Sprite | default: SpriteType.AIR
  const mouseX = e.mousePosition.x; // The offsetX of the mouse cursor
  const mouseY = e.mousePosition.y; // The offsetY of the mouse cursor

  console.log(`Mouse clicked at position: (${mouseX}, ${mouseY})`);

  // Custom logic such as moving the playerSprite by altering the posX and posY
});
```

### Example 3: Enabling Camera Movement via Dragging

```javascript
canvas.enableScreenDrag(true); // Allow camera movement by dragging the canvas
```

### Example 4: Canvas Rendered Objects

```javascript
// To keep track of all registered objects inside the canvas, you can call the getAllRegisteredObjects() function

const registeredObjects = canvas.getAllRegisteredObjects(); // Array of Sprites

// registeredObjects is passed by reference, you can alter the object's property and it will show the effect on screen. Example is by changing the sprite's posX and posY.

// To remove a sprite from being rendered inside the canvas, you can unregister the object by given objID
canvas.unregisterObject("player-1"); // Instant effect

// Get a single registered object by objID given.
const registeredObject = canvas.getRegisteredObject("sprite-id");
```

### Live Demo: Basic Implementation of the CanvasScreen

[2D-Graphics Implementation](https://jaymar921-2dgraphic-demo.vercel.app/)
