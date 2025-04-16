<context>







more_vert



more_vert





more_vert





more_vert



more_vert





more_vert

Okay, here is a basic example of an Agent-Based Model simulating ant-like agents moving within a circular arena, implemented in a single HTML file using plain JavaScript for the logic and PixiJS for WebGL rendering.

Key Features:

Agents (Ants): Simple objects with position (x, y), angle, and speed.

Behavior:

Move forward.

Wander: Slightly change direction randomly each step.

Boundary Check: Turn away when hitting the circular arena edge.

Rendering: Uses PixiJS (a 2D WebGL renderer) for efficient drawing of many agents.

Self-Contained: Everything is in one HTML file.

To Run:

Save the code below as an HTML file (e.g., ants_simulation.html).

Open the file in a modern web browser that supports WebGL (Chrome, Firefox, Edge, Safari).

<!DOCTYPE html><html><head><title>Ant Simulation (ABM + PixiJS)</title><meta charset="utf-8"><style>

body { margin: 0; overflow: hidden; background-color: #111; }

canvas { display: block; }</style><!-- Load PixiJS library from CDN --><script src="https://cdnjs.cloudflare.com/ajax/libs/pixi.js/7.4.0/pixi.min.js" integrity="sha512-xu+bF/3aJ25hY1v7CqABydx+NMh6Fz/jK7v/3x1r7r9oBCH+r+5d/DdPZqPZ8r9H/W8j27074Mp1V9i3G5D9g==" crossorigin="anonymous" referrerpolicy="no-referrer"></script></head><body><script>

// --- Simulation Parameters ---

const NUM_ANTS = 200; // Number of agents

const ANT_SPEED = 1.5; // Pixels per frame

const ANT_SIZE = 4; // Visual size of the ant (radius)

const WANDER_STRENGTH = 0.2; // How much the angle changes randomly (radians)

const BOUNDARY_TURN_STRENGTH = 0.5; // How sharply ants turn from the wall

const ARENA_PADDING = 20; // Padding from window edge



// --- Global Variables ---

let app;

let ants = [];

let arenaRadius;

let centerX, centerY;

let antContainer;



// --- Initialization ---

function init() {

// Create PixiJS Application

app = new PIXI.Application({

width: window.innerWidth,

height: window.innerHeight,

backgroundColor: 0x282c34, // Dark background

antialias: true,

resolution: window.devicePixelRatio || 1,

autoDensity: true

});

document.body.appendChild(app.view); // Add canvas to HTML



// Calculate Arena dimensions

calculateArena();



// Create a container for ants

antContainer = new PIXI.Container();

app.stage.addChild(antContainer);



// Create Ants

for (let i = 0; i < NUM_ANTS; i++) {

// Start ants randomly within the circle

const angle = Math.random() * Math.PI * 2;

const radius = Math.random() * arenaRadius * 0.9; // Start slightly away from edge

const startX = centerX + Math.cos(angle) * radius;

const startY = centerY + Math.sin(angle) * radius;



// Create the visual representation (a simple circle graphic)

const antGraphic = new PIXI.Graphics();

antGraphic.beginFill(0xFFFF00); // Yellow color for ants

antGraphic.drawCircle(0, 0, ANT_SIZE);

antGraphic.endFill();

antGraphic.x = startX;

antGraphic.y = startY;

antContainer.addChild(antGraphic);



// Create the agent data object

ants.push({

x: startX,

y: startY,

angle: Math.random() * Math.PI * 2, // Random initial direction

speed: ANT_SPEED * (0.8 + Math.random() * 0.4), // Slight speed variation

graphics: antGraphic // Link to the visual part

});

}



// Start the simulation loop

app.ticker.add(update);



// Handle window resizing

window.addEventListener('resize', onResize);

}



// --- Simulation Update Loop ---

function update(delta) { // delta is time elapsed since last frame

for (let i = 0; i < ants.length; i++) {

const ant = ants[i];



// 1. Wander Behavior: Add small random change to angle

ant.angle += (Math.random() - 0.5) * WANDER_STRENGTH;



// 2. Move Forward

const vx = Math.cos(ant.angle) * ant.speed * delta;

const vy = Math.sin(ant.angle) * ant.speed * delta;

ant.x += vx;

ant.y += vy;



// 3. Boundary Check

const dx = ant.x - centerX;

const dy = ant.y - centerY;

const distSq = dx * dx + dy * dy; // Use squared distance for efficiency



if (distSq > (arenaRadius - ANT_SIZE) * (arenaRadius - ANT_SIZE)) {

const dist = Math.sqrt(distSq);

const overlap = dist - (arenaRadius - ANT_SIZE);



// Push back slightly

ant.x -= dx / dist * overlap;

ant.y -= dy / dist * overlap;



// Turn away from the wall

const angleToCenter = Math.atan2(dy, dx);

// Aim roughly opposite to the wall normal (angleToCenter)

ant.angle = angleToCenter + Math.PI + (Math.random() - 0.5) * BOUNDARY_TURN_STRENGTH;

}



// 4. Update Visual Position

ant.graphics.x = ant.x;

ant.graphics.y = ant.y;

// Optional: Update rotation if using a non-circular sprite

// ant.graphics.rotation = ant.angle;

}

}



// --- Utility Functions ---

function calculateArena() {

centerX = window.innerWidth / 2;

centerY = window.innerHeight / 2;

arenaRadius = Math.min(centerX, centerY) - ARENA_PADDING;

}



function onResize() {

// Resize the renderer

app.renderer.resize(window.innerWidth, window.innerHeight);



// Recalculate arena dimensions

calculateArena();



// Optional: You might want to reposition ants if the aspect ratio changes significantly

// or just let them adapt to the new boundary. Currently, they just adapt.

}



// --- Start the simulation ---

init();</script></body></html>

content_copy

download

Use code with caution.

Html

Explanation:

HTML Structure: Basic HTML with CSS to make the canvas fill the screen.

PixiJS Load: Loads the PixiJS library from a CDN.

Parameters: Constants define the simulation's behavior (number of ants, speed, size, etc.).

 Function:

Sets up the PixiJS application (PIXI.Application) which automatically creates a <canvas> element and uses WebGL if available.

Calculates the circular arena boundaries based on window size.

Creates a PIXI.Container to hold all ant graphics efficiently.

Loops NUM_ANTS times:

Creates a data object for each ant storing its state (x, y, angle, speed).

Creates a PIXI.Graphics object (a yellow circle) for the ant's visual representation.

Adds the graphic to the container and links it to the data object.

Adds the ant data object to the ants array.

Starts the main loop using app.ticker.add(update). The ticker uses requestAnimationFrame internally for smooth animation.

Adds a resize listener.

 Function: This is the core simulation loop, executed every frame.

It iterates through all ant objects in the ants array.

Wander: Modifies the ant.angle slightly and randomly.

Move: Calculates the change in x and y based on the current angle and speed, and updates the ant.x and ant.y. The delta value helps keep movement speed consistent across different frame rates.

Boundary Check: Calculates the distance from the center. If the ant is outside the arenaRadius, it's pushed back inside, and its angle is changed to point away from the wall.

Update Visuals: The x and y properties of the corresponding ant.graphics object are updated to match the simulation state, making the visual representation move on the screen.

 &  Helper functions to set up and update dimensions when the window is resized.

Potential Improvements/Next Steps:

Trails: Modify the update loop to draw short line segments behind each ant or use a PIXI.Graphics object per ant to draw its trail (can become performance-intensive). A more advanced technique involves rendering trails to a texture.

Ant-Ant Interactions: Implement collision avoidance (e.g., check distance to nearby ants and turn away). This requires an efficient way to find nearby ants, like the Spatial Hashing technique mentioned previously, especially for larger NUM_ANTS.

Pheromones: Add a layer (perhaps another texture or grid) for pheromones that ants deposit and react to.

Sprites: Replace the simple PIXI.Graphics circles with actual ant images (PIXI.Sprite) for better visuals.

Performance: For thousands of ants, investigate using PIXI.ParticleContainer (less flexible but faster) or optimizing interaction checks heavily (Spatial Hashing, Quadtrees). Moving simulation logic to a Web Worker would prevent UI freezes.





</context>



Write the JS code on canvas and run it