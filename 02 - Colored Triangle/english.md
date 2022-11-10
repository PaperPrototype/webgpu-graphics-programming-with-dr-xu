![](https://www.youtube.com/watch?v=QWh968pmsbg)

> Code Update! Make sure you `npm update @webgpu/types` to the latest version
> Since Dr Xu's recording, Chrome Canary made changes to the WebGPU API: Below is the updated code (taken from Dr Xu's github [here](https://github.com/jack1232/webgpu02/blob/main/src/main.ts))

```ts
    //...

    const swapChainFormat = "bgra8unorm";
    const swapChain = context.configure({
        device: device,
        format: swapChainFormat,
        alphaMode: "opaque",
    });

    const shader = Shaders(color);
    const pipeline = device.createRenderPipeline({
        layout:'auto',
        vertex: {

        },
    })

    //...
```

# Notes
I recommend you update `CheckWebGPU` to:

```js
export const CheckWebGPU = () => {
    let result = true;
    if (!navigator.gpu) {
        result = false;
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

    return {result, message};
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