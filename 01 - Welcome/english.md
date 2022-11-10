# Welcome!

This series of youtube tutorials are by Dr Xu. All credit to him. I took it upon myself to take the tutorial videos and organize them into a course in sparker :)

All markdown files in this course are free and open source. If you find a mistake or want to suggest an edit you can do so [here!](https://github.com/PaperPrototype/webgpu-graphics-programming-with-dr-xu)

All project source code for the course can be found [here](https://github.com/jack1232/WebGPU-Step-By-Step)

# Setup

![](https://www.youtube.com/watch?v=-hXtt4ioH5A)

Settings file changes (replace `PATH_TO_CHROME_CANARY`)
```json
    "liveServer.settings.donotShowInfoMsg": true,
    "liveServer.settings.root": "/dist",
    "liveServer.settings.AdvanceCustomBrowserCmdLine": "PATH_TO_CHROME_CANARY",
```

# Trouble shooting
If `tsc --init` failed use `npx tsc --init`

Full `CheckWebGPU` function
```ts
export const CheckWebGPU = () => {
    let result = "Great, your current browser supports WebGPU!";
    if (!navigator.gpu) {
        result = `Your current browser does not support WebGPU! Make sure you are on a
        system with WebGPu enabled. Currently WebGPu is supported in 
        <a href="https://www.google.com/chrome/canary">Chome Canary</a>
        with the flag "enable-unsafe-webgpu" enabled (recommended). See the 
        <a href="https://github.com/gpuweb/gpuweb/wiki/Implementation-Status">
        Implementation Status</a> page for more details.
        You can also use your regular chrome to try a pre-releaseversion of WebGPU via
        <a href="https://developer.chrome.com/origintrials/#/view_trial/118219490218475521">Origina Trials</a>.
        `;
    }

    const canvas = document.getElementById('canvas-webgpu') as HTMLCanvasElement;
    if(canvas){
        const div = document.getElementsByClassName('item2')[0] as HTMLDivElement;
        if(div){
            canvas.width  = div.offsetWidth;
            canvas.height = div.offsetHeight;

            function windowResize() {
                canvas.width  = div.offsetWidth;
                canvas.height = div.offsetHeight;
            };
            window.addEventListener('resize', windowResize);
        }
    }

    return result;
}
```

Full `webpack.config.js` file
```ts
const path = require("path");
const bundleOutputDir = "./dist";

module.exports = {
    entry: {
        main: "./src/main"  
    },
    output: {
        filename: "[name].bundle.js",
        path: path.join(__dirname, bundleOutputDir),
        publicPath: 'public/dist/'
    },
    devtool: "source-map",
    resolve: {
        extensions: ['.js', '.ts']
    },
    module: {
        rules: [
            {
                test: /\.js$/,
                exclude: ['/node_modules/']
            },            
            { test: /\.tsx?$/, loader: "ts-loader" },        
            {
                test: /\.css$/,
                sideEffects: true,
                loader: "css-loader"
            }
        ]
    }
};
```