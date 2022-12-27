# JS Boilerplate For 3D Rendering With Physics

## Getting Started
Run `npm i` to install dependencies.

## Development
A development server can be started by running `npm run dev`. The app is available at `http://localhost:1234`.

## Building
The app can be bundled for release by running `npm run build`. The output can be found in the `/dist` directory.

## Engine Usage
The "engine" (located at `app/lib/Engine.js`) facilitates rendering 3D objects with Three.js and simulating physics with Ammo.js (a direct port of Bullet).
Example usage of the engine's features as described here can be seen in `app/scene.js`.

To use the engine first import it, then create a new instance of it by providing a DOM element for the renderer to attach to.
Lastly execute the `init()` method (note this method is asynchronous). 
```javascript
import Engine from './lib/Engine'
import { $ } from './lib/util';

(async () => {
  const E = new Engine($('root'));
  await E.init();
})();
```

### Generating Rigid Bodies
Rigid bodies with physics properties can be generated by providing size, position, and rotation parameters as applicable.

#### Creating A Box

```javascript
E.createBox(
  {x: 1, y: 1, z: 1}, // x, y, z size parameters 
  {x: 1, y: 1, z: 1}, // x, y, z position parameters
  {x: 0, y: 0, z: 0, w: 1}, // x, y, z, z rotation quaternion
  1, // mass
  'name', // string name (optional)
  0xcccccc // hex color (optional)
);
```

#### Creating A Capsule

```javascript
E.capsule(
  1, // radius,
  1, // length
  {x: 1, y: 1, z: 1}, // x, y, z position parameters
  {x: 0, y: 0, z: 0, w: 1}, // x, y, z, z rotation quaternion
  1, // mass
  'name', // string name (optional)
  0xcccccc // hex color (optional)
);
```

#### Creating A Sphere

```javascript
E.sphere(
  1, // radius,
  {x: 1, y: 1, z: 1}, // x, y, z position parameters
  {x: 0, y: 0, z: 0, w: 1}, // x, y, z, z rotation quaternion
  1, // mass
  'name', // string name (optional)
  0xcccccc // hex color (optional)
);
```

### Creating Lights
Lights must be added to the scene to see what's going on.

Ambient lights will add light to the entire scene, but will not cause objects to cast shadows.
#### Creating An Ambient Light
```javascript
E.createAmbientLight(
  0xffffff, // hex color
  0.5 // intensity (0-1.0)
);
```

Spot lights are light which is focused in a certain direction and will cause objects to cast shadoes.
#### Creating A Spot Light
```javascript
E.createSpotLight(
  0xffffff,// hex color
  1, // intensity (0-1.0)
  {x: -10, y: 25, z: -10}, // x, y, z position parameters
  {x: 0, y: 0, z: 0} // x, y, z position to point to
);
```

### Physics
Rigid bodies are automatically generated with physics properties based on values set when created (like size and mass).

Gravity must be explicitly set to affect rigid bodies in the environment.
```javascript
E.setGravity(
  {x: 0, y: -150, z: 0} // x, y, z direction of gravity
);
```
Rigid bodies with a mass of `0` will not be affected by gravity.

### Diagnostics
There is an info panel which can be used to show dynamic or static diagnostic information.

#### Adding An Updatable Parameter
```javascript
E.addInfoReadoutParameterItem(
  'fps', // parameter name
  'FPS' // pretty name (optional)
);
```

#### Updating a Parameter Value
```javascript
E.updateInfoParameterValue(
  'fps', // parameter name to update
  Math.round(E.calcFPS()) // new parameter value
);
```

#### Adding Static Info Text
```javascript
E.addInfoReadoutTextItem('Some text here.');
```

### Utilities
The engine has some inbuilt utility methods, and there is also a more general utilities library at `app/lib/utils.js`.

#### Camera Position
A camera is generated automatically when the engine is initialized.
The camera can be manipulated programatically.
```javascript
E.setActiveCameraPosition(
  {x: 15, y: 20, z: -30}, // x, y, z position parameters
  {x: 0, y: 0, z: 0} // x, y, z position to point to
);
```

#### Render Callback
To execute code after every render, attach a function to `E.renderCallback`.
```javascript
E.renderCallback = () => {
  E.updateInfoParameterValue('fps', Math.round(E.calcFPS()));
}
```

___
&copy; 2022 [Michael Hamilton](https://miska.me)
