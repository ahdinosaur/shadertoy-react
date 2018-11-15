Shadertoy React
==============

Simple react component letting you easily add shaders you've been building on Shadertoy to your React page. I found myself using Shadertoy most of the time when I needed to create some shader for projects, since the live reload functionnality makes it really easy to start working on the visual quickly. 

 Can be really usefull when you want to add some interactive pieces in your web page or even just replace static images by interactive/generative shader.

### The way it works

I tried to stay as close as possible to the Shadertoy implementation. Basically it uses a WebGL context on a `<canvas/>` and render a material on a full viewport quad composed of 2 triangles. The canvas size matches the css size of your element, by default it it 100% 100% of your parent element size, this can be changed by passing a custom `style={}` prop to your component.

### React props

Here are a few built in react props you can pass to your component. Feel free to suggest more.

  * `textures` -- An array of textures objects following that structure `{url: ... , minFilter: , magFilter: , wrapS: ,wrapT: }` the format supported are (.jpg, .jpeg, .png, .bmp) for images, and (.mp4, .3gp, .webm, .ogv) for videos. 
  * `devicePixelRatio` -- A value passed to set the [pixel density](https://developer.mozilla.org/en-US/docs/Web/API/Window/devicePixelRatio) of the canvas. By default 1.
  * `fs` -- A string containing your fragment shader.
  * `style` -- Pass a [React Inline style](https://reactjs.org/docs/dom-elements.html#style) to customize the style of your canvas.
  * `contextOptions` -- To customize your [WebGL context attributes.](https://developer.mozilla.org/en-US/docs/Web/API/HTMLCanvasElement/getContext)
  * `onDoneLoadingTextures` -- Callback called once all textures are done being loaded, usefull when you want to wait for your shader to have all the needed texture before seeing it on screen. Using that callback you could for example simply fade the canvas in using css. 
  * `lerp` -- A value in between 0 - 1 used to lerp the mouse position in your fragment shader.
  

### Built in uniforms

Uniforms you can use in your shader.

  * `uniform float iTime` -- shader playback time (in seconds).
  * `uniform float iTimeDelta` -- Render time (in seconds).
  * `uniform int iFrame` -- Shader playback frame.
  * `uniform vec2 iResolution` -- viewport resolution (in pixels).
  * `uniform vec4 iDate` -- (year, month, day, time in seconds).
  * `uniform vec4 iMouse` -- mouse pixel coords. xy: current (if MLB down), zw: click.
  * `uniform sampler2D iChannel^n` -- The textures input channel you've passed; numbered in the same order as the textures passed as prop in your react component.
  * `uniform vec3 iChannelResolution[n]` -- An array containing the texture channel resolution (in pixels).

### How to use it

Example of React Component using ShadertoyReact:

    import React from  'react';
	import { render} from  'react-dom';
    import ShadertoyReact, { LinearFilter, NearestFilter, } from 'ShadertoyReact';

	const ExampleApp = () =>
		<Container>
			<ShadertoyReact fs={fs}/>
		</Container>;
	

Example of shader: 

    void mainImage( out vec4 fragColor, in vec2 fragCoord ) {
	    // Normalized pixel coordinates (from 0 to 1)
	    vec2 uv = gl_FragCoord.xy/iResolution.xy;
	    
	    // Time varying pixel color
	    vec3 col = 0.5 + 0.5*cos(iTime+uv.xyx+vec3(0,2,4));
	    
	    // Output to screen
	    gl_FragColor = vec4(col,1.0);
    }

### What's next

* Add support to multi passes as Shadertoy is doing.
* Add support for Cube texture.
* Add the rest of Shadertoy built in uniforms.
* Add support for WebGL2 and GLSL 3.0.
* Add uniform for phone device orientation / gyroscope based effects.
* Add support for custom uniforms.
* Add support for keyboard uniforms / inputs.
* Add support for iChannelTime.
* ~~Add props for optionnal mouse lerping.~~
* ~~Add support for iDate.~~
* ~~Add support for video textures.~~
* ~~Add support for iChannelResolution.~~
