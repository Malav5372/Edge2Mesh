# Edge2Mesh: Advanced 2D Line Projection and Silhouette mesh Generator
 

## Introduction:

Welcome to Edge2Mesh, my flagship computer graphics project developed in JavaScript. Edge2Mesh is a versatile tool designed for 3D projection and visualization. It offers two primary types of projections: geometry edge projection and silhouette projection. Each projection type is equipped with a set of configurable metrics and controls, allowing you to fine-tune and optimize your 3D projections for various computer graphics tasks.

### Type 1: Geometry Edge Projection Metrics

#### Geometry Edge Projection in action(video implementation):

1. Introduction-

https://github.com/Malav5372/Edge2Mesh/assets/144440737/e8b052c0-a9c2-4baf-80f4-37ab23848838









#### Display Model Color:

Description: This metric allows you to customize the colors used to visualize the 3D model during the projection process. You can specify distinct colors for different parts of the model, aiding in the clarity of your rendered projection.

Use Case: Useful when you want to differentiate between different components or surfaces of your 3D model visually. For example, you can use different colors for front-facing and back-facing surfaces.

#### Display Projection:

Description: Enabling this metric results in the visualization of the projection results. It allows you to see the projected geometry on your screen, which is vital for assessing the accuracy and quality of the projection.

Use Case: Essential for reviewing and refining the projection outcome. It helps you identify any issues or anomalies in the projected geometry.

#### SortEdges:

Description: This control influences the order in which edges are processed and displayed during the projection. Proper edge sorting is crucial for rendering accuracy, especially when dealing with complex 3D scenes.

Use Case: Helps ensure that edges are drawn in the correct order to avoid visual artifacts such as Z-fighting, where two surfaces appear to flicker or overlap incorrectly.

#### IncludeIntersectionEdges:

Description: When activated, this metric adds intersection edges to the projection. Intersection edges are edges that result from the intersection of two or more surfaces or objects within the 3D model.

Use Case: Enhances the precision of the projection by incorporating intersection edges, resulting in a more accurate representation of the 3D scene.

#### UseWorker:

Description: The "UseWorker" control enables the use of worker threads during the projection process. Worker threads run in the background and can significantly enhance the performance of the projection, making it faster and more efficient.
Use Case: Particularly beneficial when dealing with complex or computationally intensive 3D models, as it allows the projection to take advantage of multi-core processors for faster execution.

### Type 2: Silhouette Projection Metrics

#### Display Outline:

Description: The "Display Outline" control adds an outline around the silhouette of the 3D model. This outline enhances the visibility of the silhouette and provides a more striking visual effect.

Use Case: Useful when you want to make the silhouette projection more pronounced and visually appealing.

#### Display Wireframe:

Description: Enabling this metric renders the silhouette using wireframe lines instead of solid surfaces. This results in a distinctive visual style where the silhouette is represented by a network of lines.

Use Case: Suitable for achieving a unique, wireframe-style presentation of the silhouette, which can be artistically appealing.

Explore Edge2Mesh's comprehensive set of metrics and controls to tailor your 3D projections to your specific needs, whether you're focused on geometry edge projection or silhouette projection. This project is designed to elevate your computer graphics endeavors, providing you with the tools you need for accurate and visually appealing results.

# Use

**Generator**

More granular API with control over when edge trimming work happens.

```js
const generator = new ProjectionGenerator();
generator.generate( geometry );

let result = task.next();
while ( ! result.done ) {

	result = task.next();

}

const lines = new LineSegments( result.value, material );
scene.add( lines );
```

**Promise**

Simpler API with less control over when the work happens.

```js
const generator = new ProjectionGenerator();
const geometry = await generator.generateAsync( geometry );
const mesh = new Mesh( result.value, material );
scene.add( mesh );
```


# API

## ProjectionGenerator

### .sortEdges

```js
sortEdges = true : Boolean
```

Whether to sort edges along the Y axis before iterating over the edges.

### .iterationTime

```js
iterationTime = 30 : Number
```

How long to spend trimming edges before yielding.

### .angleThreshold

```js
angleThreshold = 50 : Number
```

The threshold angle in degrees at which edges are generated.

### .includeIntersectionEdges

```js
includeIntersectionEdges = false : Boolean
```

Whether to generate edges representing the intersections between triangles.

### .generate

```js
*generate(
	geometry : MeshBVH | BufferGeometry,
	options : {
		onProgress: ( percent : Number ) => void,
	}
) : BufferGeometry
```

Generate the edge geometry using a generator function.

### .generateAsync

```js
generateAsync(
	geometry : MeshBVH | BufferGeometry,
	options : {
		onProgress: ( percent : Number ) => void,
		signal: AbortSignal,
	}
) : Promise<BufferGeometry>
```

Generate the geometry with a promise-style API.

## SilhouetteGenerator

Used for generating a projected silhouette of a geometry using the [clipper2-js](https://www.npmjs.com/package/clipper2-js) project. Performing these operations can be extremely slow with more complex geometry and not always yield a stable result.

### .iterationTime

```js
iterationTime = 10 : Number
```

How long to spend trimming edges before yielding.

### .doubleSided

```js
doubleSided = false : Boolean
```

If `false` then only the triangles facing upwards are included in the silhouette.

### .generate

```js
*generate(
	geometry : BufferGeometry,
	options : {
		onProgress: ( percent : Number ) => void,
	}
) : BufferGeometry
```

Generate the geometry using a generator function.

### .generateAsync

```js
generateAsync(
	geometry : BufferGeometry,
	options : {
		onProgress: ( percent : Number ) => void,
		signal: AbortSignal,
	}
) : Promise<BufferGeometry>
```

Generate the silhouette geometry with a promise-style API.

