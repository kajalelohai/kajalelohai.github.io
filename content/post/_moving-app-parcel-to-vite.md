+++
author = "Kajal Negi"
title = "Move your app from Parcel to Vite"
date = "2020-02-27"
description = ""
tags = [
    "security",
]
draft = false
+++

Below are the list of changes needed to switch from Parcel to Vite:

- changes in package.json

	* empty out the script object
	* Remove all things related to parcel such as for me its:

		* "@parcel/transformer-sass"
		* "parcel": "^2.8.2",

- Run `npm i` to remove all the packages.
- Remove *.parcel-cache*
- I had one project setup with vite in my machine so I copied the packages needed and vite scripts object to package.json as below:
	```json
	"scripts": {
	"dev": "vite",
	"build": "tsc && vite build",
	"preview": "vite preview"
	},
	"devDependencies": {
	"@vitejs/plugin-react": "^3.1.0",
	"typescript": "^4.9.3",
	"vite": "^4.1.0"
	}
```

*NOTE:* I need to make above changes to the existing package.json

- Now run `npm i` to install the new dependencies:

```json
{
	"compileOnSave": true,
	"compilerOptions": {
	"jsx": "preserve",
	"experimentalDecorators": true,
	"jsxImportSource": "react",
	"isolatedModules": true,
	"esModuleInterop": true,
	"strictNullChecks": true,
	"skipLibCheck": true,
	"lib": ["ES2021", "DOM"],
	"baseUrl": "./",
	"paths": {
	"~src/*": ["src/*"]
	}
	},
	"include": ["src/**/*"],
	"exclude": ["node_modules"]
}
```
- Replaced above `tsconfig.json` with below code :
```json
{
	"compilerOptions": {
	"target": "ESNext",
	"useDefineForClassFields": true,
	"lib": ["DOM", "DOM.Iterable", "ESNext"],
	"allowJs": false,
	"skipLibCheck": true,
	"esModuleInterop": false,
	"allowSyntheticDefaultImports": true,
	"strict": true,
	"forceConsistentCasingInFileNames": true,
	"module": "ESNext",
	"moduleResolution": "Node",
	"resolveJsonModule": true,
	"isolatedModules": true,
	"noEmit": true,
	"jsx": "react-jsx",
	"baseUrl": "./",
	"paths": {
		"~src/*": ["src/*"]
	}
	},
	"include": ["src/**/*"],
	"exclude": ["node_modules"],
	"references": [{ "path": "./tsconfig.node.json" }]
}
```
- Add tsconfig.node.json with below code in it:
```json
{
	"compilerOptions": {
	"composite": true,
	"module": "ESNext",
	"moduleResolution": "Node",
	"allowSyntheticDefaultImports": true
	},
	"include": ["vite.config.ts"]
}
```
- Install `vite-tsconfig-paths` by `npm i -D vite-tsconfig-paths`
- The index.html for me was inside src so I also moved it out to root of my ui project.
- Also, inside index.html changed the src path to `src='src/index.tsx'`
- Define a `vite.config.ts` file at root of the project with below lines in it:
```ts
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'

// https://vitejs.dev/config/
export default defineConfig({

plugins: [react()],

})
```
- You are all set now to run the project using `npm run dev` also you can test it with `npm run build`.