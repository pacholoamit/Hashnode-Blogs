# Use over 140+ amazing ChatGPT prompts in 10 minutes 🚀

We're going to be exploring this amazing NodeJS library called [chatgpt-prompts](https://github.com/pacholoamit/chatgpt-prompts). This library allows you to use over 140+ amazing community built prompts compiled over at [awesome-chatgpt-prompts](https://github.com/f/awesome-chatgpt-prompts) instantly.

# 🚀 Quick Start!

If you are impatient person (like me!), here's a quick start on what the code would look like for a basic example but I recommend you follow along just to ensure that your (hopefully typescript) project gets set up correctly as this package is ESM-only.

```typescript
import { ChatGPTAPI } from "chatgpt";
import { createChatGPTPrompt } from "chatgpt-prompts";

const run = async () => {
  const instance = new ChatGPTAPI({
    apiKey: "OPEN_AI_API_KEY",
  });

  const prompt = createChatGPTPrompt(instance);

  let res = await prompt.linuxTerminal("touch hello.txt");
  console.log(res.text);

  res = await prompt.linuxTerminal("echo hello world > hello.txt");
  console.log(res.text);

  res = await prompt.linuxTerminal("cat hello.txt");

  console.log(res.text);
};

run().catch((err) => console.log("Something went wrong"));
```

# 💻 Project Setup

This section details how we would set up our typescript & NodeJS project with the `chatgpt` and `chatgpt-prompts` package.

To start setting up our project let's create a directory to house all of the code for our project

```sh
mkdir chatgpt-app
cd chatgpt-app
```

Now we have a directory, let's create a `package.json` file by running

```sh
npm init -y
```

Now let's install all of our dependencies and dev dependencies

```sh
npm install --save-dev nodemon ts-node typescript 
npm install chatgpt chatgpt-prompts
```

Your `package.json` file should look something like this

```json
{
  "name": "chatgpt-app",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "nodemon": "^2.0.20",
    "ts-node": "^10.9.1",
    "typescript": "^4.9.5"
  }
}
```

We're going to be making some absolutely important changes to our `package.json` file since `chatgpt` and `chatgpt-prompts` are ESM-only packages.

Modify your package.json so that it looks something like this

```json
{
  "name": "chatgpt-app",
  "version": "1.0.0",
  "description": "",
  "main": "dist/index.js",
  "types": "dist/index.d.ts",
  "type": "module",
  "scripts": {
    "start": "nodemon --ext js,ts,json,env --exec 'node --experimental-specifier-resolution=node --loader ts-node/esm' src/index.ts"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "nodemon": "^2.0.20",
    "ts-node": "^10.9.1",
    "typescript": "^4.9.5"
  },
  "dependencies": {
    "chatgpt": "^4.1.1",
    "chatgpt-prompts": "^1.1.1"
  }
}
```

Here were the changes that we've made for extra clarity:

* We've changed the `main` field from `index.js` to `dist/index.js`
    
* We've added the `types` field with a value of `dist/index.d.ts`
    
* We've added the `type` field with a value of `module`; and finally
    
* We've modified the `start` script to be really long.
    

After this is completed, we're going to set up our lengthy but absolutely necessary `tsconfig.json` file. Feel free to copy the one below

```json
{
  "compilerOptions": {
    "target": "ESNext",
    "module": "ESNext",
    "moduleResolution": "Node",
    "allowSyntheticDefaultImports": true,
    "esModuleInterop": true,
    "importHelpers": true,
    "resolveJsonModule": true,
    "sourceMap": true,
    "declaration": true,
    "alwaysStrict": true,
    "forceConsistentCasingInFileNames": true,
    "removeComments": true,
    "noImplicitAny": true,
    "strictNullChecks": true,
    "strictFunctionTypes": true,
    "noImplicitThis": true,
    "noUnusedLocals": false,
    "noUnusedParameters": false,
    "noImplicitReturns": true,
    "strict": true,
    "noFallthroughCasesInSwitch": true,
    "emitDecoratorMetadata": true,
    "experimentalDecorators": true,
    "composite": true,
    "skipLibCheck": true,
    "noEmitHelpers": false,
    "outDir": "dist",
    "rootDir": "src",
    "baseUrl": "."
  },
  "include": ["src"],
  "exclude": ["dist", "node_modules"]
}
```

Awesome! After that is set up, we're now going to be writing some actual code. Please run the commands below to create a `src` directory and an `index.ts` file.

```plaintext
mkdir src
cd src
touch index.ts
```

Now we have created our `index.ts` file, let's actually write and run our code. Here's a code snippet to get you started

```typescript
import { ChatGPTAPI } from "chatgpt";
import { createChatGPTPrompt } from "chatgpt-prompts";

const run = async () => {
  const instance = new ChatGPTAPI({
    apiKey: "OPEN_AI_API_KEY",
  });

  const prompt = createChatGPTPrompt(instance);

  let res = await prompt.linuxTerminal("touch hello.txt");
  console.log(res.text);

  res = await prompt.linuxTerminal("echo hello world > hello.txt");
  console.log(res.text);

  res = await prompt.linuxTerminal("cat hello.txt");

  console.log(res.text);
};

run().catch((err) => console.log("Something went wrong"));
```

You should have an output similar to that one below

![Success message](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/64mfka692ad4adprtfkp.png align="left")

# 💯 Congratulations

You now have access to over 140+ amazing prompts in NodeJS. I'd also like to credit the following people for the awesome work they've done:

* A big thank you to [Travis Fischer](https://github.com/transitive-bullshit) for making an amazing [NodeJS Client](https://github.com/transitive-bullshit/chatgpt-api) of the ChatGPT API.
    
* All of the prompts featured in this package comes from [awesome-chatgpt-prompts](https://github.com/f/awesome-chatgpt-prompts) maintained by [Fatih Kadir Akın](https://github.com/f)