# glsl-hash-blur

[![stable](http://badges.github.io/stability-badges/dist/stable.svg)](http://github.com/badges/stability-badges)

![grain](http://i.imgur.com/uTGaC5t.jpg?1)

A fast blur effect that uses a random hash to compute the sample offset. This gives a snowy/grainy feel to the blur. When requiring, you must specify a `sample` function and the `iterations` (a `const` or `#define` integer).

```glsl
vec3 tex(vec2 uv);

#pragma glslify: blur = require('glsl-hash-blur', sample=tex, iterations=20)

vec3 tex(vec2 uv) {
  return texture2D(iChannel0, uv).rgb;
}

void main() {
  float aspect = resolution.x / resolution.y;
  gl_FragColor.rgb = blur(vUv, radius, aspect);
}
```

See [demo.frag](demo.frag) for an implementation of a vignette blur.

The effect was inspired by David Hoskins' [ShaderToy](https://www.shadertoy.com/view/XdjSRw).

## Demos

- [applied to a texture]() 
- [editable glslbin demo](http://glslb.in/s/c1a93844)

## Usage

[![NPM](https://nodei.co/npm/glsl-hash-blur.png)](https://www.npmjs.com/package/glsl-hash-blur)

#### `blur = require('glsl-hash-blur', sample=S, iterations=I)`

Requires the module with your desired sampling function `S` and iteration count `I`. The sample function has the following signature:

```glsl
vec3 sample(in vec2 uv);
```

#### `vec3 blur(vec2 uv, float radius[, float aspect[, float offset]])`

Using the sample function provided above, this will create a blur using the specified UV coordinates and `radius` strength. The radius is typically multiplied by texel size, e.g. `1.0 / resolution.x`. 

The `aspect` (defaults to 1.0) is recommended to produce a more accurate blur; e.g. `resolution.x / resolution.y`. 

The `offset` is optional (defaults to 0.0). It offsets the uvs during randomization, which can produce a "jitter" effect like moving film noise. 

## License

MIT, see [LICENSE.md](http://github.com/mattdesl/glsl-hash-blur/blob/master/LICENSE.md) for details.