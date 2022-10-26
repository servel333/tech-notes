
[npm](https://www.npmjs.com/package/fastify), [github](https://github.com/fastify/fastify)

## Usage

- Start `node  ./app.js`

`./app.js`
```js
import Fastify from 'fastify';

const { PORT = 3000 } = process.env;
const fastify = Fastify();

fastify.get('/heartbeat', (request, reply) => 'OK');

fastify.listen(PORT, () => console.log(`Listening on port ${PORT}`));
```

## Example - small project

- Start `node  ./main/listener.js`

`./Makefile`
```Makefile
defulat: start
start: ; NODE_ENV=dev  node  ./main/listener.js
start-watcher: ; NODE_ENV=dev  npx fastify@4.9.2  start  -w  -l info  -P  ./main/server.js
```

`./main/app.js`
```js
import Fastify from 'fastify';

const app = Fastify({ logger: true });
export default app;

app.register(Autoload, {
  dir: join(__dirname, 'routes'),
});

// ----------------------------
// Load one specific route
app.register(import('../routes/heartbeat-GET));

// ----------------------------
// Use autoload to load all routes
import { dirname, join } from 'path';
import { fileURLToPath } from 'url';
import Autoload from 'fastify-autoload';

const __filename = fileURLToPath(import.meta.url);
const __dirname = dirname(__filename);
```

`./main/listener.js`
```js
import app from './app';

const { PORT = 3000 } = process.env;

app.listen(PORT, () => console.log(`Listening on port ${PORT}`));
```

`./main/server.js`
```js
import app from './app';

const { PORT = 3000 } = process.env;

// Wrap in function to make it work with the Fastify CLI.
export default async function (fastify, options) {
  app.listen(PORT, () => console.log(`Listening on port ${PORT}`));
}
```

`./routes/heartbeat-GET.js`
```js
/**
 * @param {import('fastify').FastifyInstance} fastify
 * @param {*} options
 */
export default function (fastify, options) {
  fastify.get('/heartbeat', (request, reply) => 'OK');
  fastify.get('/heartbeat2', (request, reply) => reply.code(200).send('OK'));
}
```
