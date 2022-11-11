![](https://www.youtube.com/watch?v=vmqx7rJk4uU)

# Trouble Shooting
When importing `"@webgpu/glslang/dist/web-devel-onefile/glslang"` if it fails copy paste the import string provided to make sure it is correct.

If importing is still failing, update the `tsconfig.json` file to the following:

```json
{
    "compilerOptions": {
        "rootDir": "src",
        "outDir": "dist",
        "target": "es6",
        "lib": [
            "es2017",
            "dom"
        ],
        "types": [
            "node",
            "@webgpu/types"
        ],
        "moduleResolution":"node",
        "module": "ES2015",
        "esModuleInterop": true,
        "strict": true,
        "sourceMap": true
    },
    "exclude": [
        "node_modules"
    ]
}
```