![](https://www.youtube.com/watch?v=QWh968pmsbg)

# Updates
> Since Dr Xu's recording, Chrome Canary made changes to the WebGPU API as well as changes to Web Graphics Shader Language (WGSL). Below is the updated code (taken from Dr Xu's github [here](https://github.com/jack1232/webgpu02))
>
> Also, make sure you do "npm update @webgpu/types" to update to the latest WebGPU types

(main.ts [source here](https://github.com/jack1232/webgpu02/blob/main/src/main.ts))
```ts
    //...

    // const context = canvas.getContext("gpupresent") as unknown as GPUCanvasContext;
    const context = canvas.getContext("webgpu") as unknown as GPUCanvasContext;

    const format = 'bgra8unorm';

    /*const swapChain = context.configureSwapChain({
        device: device,
        format: format,
    });*/    
    context.configure({
        device: device,
        format: format,
        alphaMode: 'opaque'
    });
    
    const shader = Shaders(color);
    const pipeline = device.createRenderPipeline({
        layout:'auto',
        vertex: {
            module: device.createShaderModule({                    
                code: shader.vertex
            }),
            entryPoint: "main"
        },
        fragment: {
            module: device.createShaderModule({                    
                code: shader.fragment
            }),
            entryPoint: "main",
            targets: [{
                format: format as GPUTextureFormat
            }]
        },
        primitive:{
           topology: "triangle-list",
        }
    });

    const commandEncoder = device.createCommandEncoder();
    const textureView = context.getCurrentTexture().createView();
    const renderPass = commandEncoder.beginRenderPass({
        colorAttachments: [{
            view: textureView,
            clearValue: { r: 0.5, g: 0.5, b: 0.8, a: 1.0 }, //background color
            loadOp: 'clear',
            storeOp: 'store'
        }]
    });
    renderPass.setPipeline(pipeline);
    renderPass.draw(3, 1, 0, 0);
    renderPass.end();

    device.queue.submit([commandEncoder.finish()]);

    //...
```

(shaders.ts [source here](https://github.com/jack1232/webgpu02/blob/main/src/shaders.ts))
```ts
export const Shaders = (color:string) => {
    const vertex = `
        @vertex
        fn main(@builtin(vertex_index) VertexIndex: u32) -> @builtin(position) vec4<f32> {
            var pos = array<vec2<f32>, 3>(
                vec2<f32>(0.0, 0.5),
                vec2<f32>(-0.5, -0.5),
                vec2<f32>(0.5, -0.5));
            return vec4<f32>(pos[VertexIndex], 0.0, 1.0);
        }
    `;

    const fragment = `
        @fragment
        fn main() -> @location(0) vec4<f32> {
            return vec4<f32>${color};
        }
    `;
    return {vertex, fragment};
}
```

# Notes
I recommend you update `CheckWebGPU` to:

```js
export const CheckWebGPU = () => {
    let success = true;
    if (!navigator.gpu) {
        success = false;
    }

    let message = "Great, your current browser supports WebGPU!";
    if (!navigator.gpu) {
        message = `Your current browser does not support WebGPU! Make sure you are on a
        system with WebGPu enabled. Currently WebGPu is supported in 
        <a href="https://www.google.com/chrome/canary">Chome Canary</a>
        with the flag "enable-unsafe-webgpu" enabled (recommended). See the 
        <a href="https://github.com/gpuweb/gpuweb/wiki/Implementation-Status">
        Implementation Status</a> page for more details.
        You can also use your regular chrome to try a pre-releaseversion of WebGPU via
        <a href="https://developer.chrome.com/origintrials/#/view_trial/118219490218475521">Origina Trials</a>.
        `;
    }

    return {success, message};
}
```

This way you can change the usage of `CheckWebGPU` in `main.ts` to the following:
```js
    //...

    const {success, message} = CheckWebGPU();

    if (!success) {
        console.log(message);
        throw('Your current browser does not support WebGPU!');
    }

    //...
```