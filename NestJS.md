# First Steps

```bash
# Start listening on given port
$ npm run start
```

```bash
# Equivalent to --watch
$ npm run start:dev
```

```bash
# Lint and autofix with eslint
$ npm run lint

# Format with prettier
$ npm run format
```

# Controllers

Handle requests and return responses.

<img src="https://docs.nestjs.com/assets/Controllers_1.png">

The routing mechanism decides which controller receives which requests. A controller can have more than one route.

```bash
# Generates all resources for an entity (e.g. User, or Product) - includes a controller with built-in validation
nest g resource [name]
```

```bash
# Generates a controller without built-in validation
nest g controller [name]
```

Define a controller using the `@Controller()` decorator:

```typescript
import { Controller, Get } from '@nestjs/common';

@Controller('cats')
export class CatsController {
  @Get()
  findAll(): string {
    return 'This action returns all cats';
  }
}
```

>[!note]
>The argument given to the `@Controller` decorator specifies that any method within this class will respond to any route with the URL prefix `/cats`. This includes routes such as `/cats/find-all`