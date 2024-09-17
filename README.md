# Learn Nestjs

This repository contains a list of Nestjs concepts, examples and best practices.

![GitHub](https://img.shields.io/github/license/manthanank/learn-nestjs)

## Table of Contents

- [Introduction](#introduction)
- [Getting Started](#getting-started)
- [Controllers](#controllers)
- [Providers](#providers)
- [Modules](#modules)
- [Middleware](#middleware)
- [Guards](#guards)
- [Interceptors](#interceptors)
- [Filters](#filters)
- [Pipes](#pipes)
- [Exception Filters](#exception-filters)
- [Validation Pipes](#validation-pipes)
- [Swagger](#swagger)
- [TypeORM](#typeorm)
- [Mongoose](#mongoose)
- [GraphQL](#graphql)
- [Websockets](#websockets)
- [Microservices](#microservices)
- [Testing](#testing)

## Introduction

- Nestjs is a progressive Node.js framework for building efficient, reliable, and scalable server-side applications. It uses modern JavaScript, is built with TypeScript, and combines elements of OOP (Object Oriented Programming), FP (Functional Programming), and FRP (Functional Reactive Programming).

- Nestjs is built on top of Express, which is a fast, unopinionated, minimalist web framework for Node.js.

## Getting Started

- Install Nestjs CLI globally

```bash
npm i -g @nestjs/cli
```

- Create a new Nestjs project

```bash
nest new project-name
```

- Start the Nestjs project

```bash
cd project-name
npm run start
```

- Open the browser and navigate to `http://localhost:3000`

## Controllers

- Controllers are responsible for handling incoming requests and returning responses to the client.

- Create a new controller

```bash
nest generate controller controller-name
```

- Example of a controller

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

## Providers

- Providers are a fundamental concept in Nestjs. They are used to create services, repositories, factories, helpers, and other reusable components.

- Create a new provider

```bash
nest generate provider provider-name
```

- Example of a provider

```typescript
import { Injectable } from '@nestjs/common';

@Injectable()
export class CatsService {
  private readonly cats: string[] = ['Cat 1', 'Cat 2', 'Cat 3'];

  findAll(): string[] {
    return this.cats;
  }
}
```

## Modules

- Modules are used to organize the application structure. They are used to group related controllers, providers, and other components.

- Create a new module

```bash
nest generate module module-name
```

- Example of a module

```typescript
import { Module } from '@nestjs/common';
import { CatsController } from './cats.controller';
import { CatsService } from './cats.service';

@Module({
  controllers: [CatsController],
  providers: [CatsService],
})
export class CatsModule {}
```

## Middleware

- Middleware functions are functions that have access to the request object (req), the response object (res), and the next middleware function in the application’s request-response cycle.

- Create a new middleware

```bash
nest generate middleware middleware-name
```

Example of a middleware

```typescript
import { Injectable, NestMiddleware } from '@nestjs/common';
import { Request, Response } from 'express';

@Injectable()
export class LoggerMiddleware implements NestMiddleware {
  use(req: Request, res: Response, next: Function) {
    console.log('Request...');
    next();
  }
}
```

## Guards

- Guards are used to protect routes from unauthorized access.

- Create a new guard

```bash
nest generate guard guard-name
```

- Example of a guard

```typescript
import { Injectable, CanActivate, ExecutionContext } from '@nestjs/common';
import { Observable } from 'rxjs';

@Injectable()
export class AuthGuard implements CanActivate {
  canActivate(
    context: ExecutionContext,
  ): boolean | Promise<boolean> | Observable<boolean> {
    const request = context.switchToHttp().getRequest();
    return validateRequest(request);
  }
}
```

## Interceptors

- Interceptors are used to intercept incoming requests and outgoing responses.

- Create a new interceptor

```bash
nest generate interceptor interceptor-name
```

- Example of an interceptor

```typescript
import { Injectable, NestInterceptor, ExecutionContext, CallHandler } from '@nestjs/common';
import { Observable } from 'rxjs';

@Injectable()
export class LoggingInterceptor implements NestInterceptor {
  intercept(context: ExecutionContext, next: CallHandler): Observable<any> {
    console.log('Before...');
    const now = Date.now();
    return next
      .handle()
      .pipe(tap(() => console.log(`After... ${Date.now() - now}ms`)));
  }
}
```

## Filters

- Filters are used to handle exceptions that are thrown during the execution of the application.

- Create a new filter

```bash
nest generate filter filter-name
```

- Example of a filter

```typescript
```typescript
import { ExceptionFilter, Catch, ArgumentsHost } from '@nestjs/common';

@Catch(HttpException)
export class HttpExceptionFilter implements ExceptionFilter {
    catch(exception: HttpException, host: ArgumentsHost) {
        const ctx = host.switchToHttp();
        const response = ctx.getResponse();
        const request = ctx.getRequest();

        const status = exception.getStatus();

        response
            .status(status)
            .json({
                statusCode: status,
                timestamp: new Date().toISOString(),
            });
    }
}
```

## Pipes

- Pipes are used to transform input data to the desired output.

- Create a new pipe

```bash
nest generate pipe pipe-name
```

- Example of a pipe

```typescript
import { PipeTransform, Injectable, ArgumentMetadata } from '@nestjs/common';

@Injectable()
export class ValidationPipe implements PipeTransform {
  transform(value: any, metadata: ArgumentMetadata) {
    return value;
  }
}
```

## Exception Filters

- Exception filters are used to handle exceptions that are thrown during the execution of the application.

- Create a new exception filter

```bash
nest generate filter exception-filter-name
```

- Example of an exception filter

```typescript
import { ExceptionFilter, Catch, ArgumentsHost } from '@nestjs/common';

@Catch(HttpException)
export class HttpExceptionFilter implements ExceptionFilter {
    catch(exception: HttpException, host: ArgumentsHost) {
        const ctx = host.switchToHttp();
        const response = ctx.getResponse();
        const request = ctx.getRequest();

        const status = exception.getStatus();

        response
            .status(status)
            .json({
                statusCode: status,
                timestamp: new Date().toISOString(),
            });
    }
}
```

## Validation Pipes

- Validation pipes are used to validate the input data.

- Create a new validation pipe

```bash
nest generate pipe validation-pipe-name
```

- Example of a validation pipe

```typescript
import { PipeTransform, Injectable, ArgumentMetadata } from '@nestjs/common';

@Injectable()
export class ValidationPipe implements PipeTransform {
  transform(value: any, metadata: ArgumentMetadata) {
    return value;
  }
}
```

## Swagger

- Swagger is a tool that helps you document your API.

- Install Swagger

```bash
npm install --save @nestjs/swagger swagger-ui-express
```

- Add Swagger to the application

```typescript
import { DocumentBuilder, SwaggerModule } from '@nestjs/swagger';

const app = await NestFactory.create(AppModule);

const config = new DocumentBuilder()
  .setTitle('Nestjs API')
  .setDescription('The Nestjs API description')
  .setVersion('1.0')
  .addTag('nestjs')
  .build();

const document = SwaggerModule.createDocument(app, config);
SwaggerModule.setup('api', app, document);

await app.listen(3000);
```

- Open the browser and navigate to `http://localhost:3000/api`

## TypeORM

- TypeORM is an Object Relational Mapping (ORM) library for TypeScript.

- Install TypeORM

```bash
npm install --save @nestjs/typeorm typeorm mysql
```

- Create a new entity

```typescript
import { Entity, Column, PrimaryGeneratedColumn } from 'typeorm';

@Entity()
export class Cat {
  @PrimaryGeneratedColumn()
  id: number;

  @Column()
  name: string;

  @Column()
  age: number;
}
```

- Create a new repository

```typescript
import { EntityRepository, Repository } from 'typeorm';
import { Cat } from './cat.entity';

@EntityRepository(Cat)
export class CatRepository extends Repository<Cat> {}
```

- Use the repository in the service

```typescript
import { Injectable } from '@nestjs/common';
import { CatRepository } from './cat.repository';

@Injectable()
export class CatService {
  constructor(private readonly catRepository: CatRepository) {}

  async findAll(): Promise<Cat[]> {
    return this.catRepository.find();
  }
}
```

## Mongoose

- Mongoose is an Object Data Modeling (ODM) library for MongoDB and Node.js.

- Install Mongoose

```bash
npm install --save @nestjs/mongoose mongoose
```

- Create a new schema

```typescript
import * as mongoose from 'mongoose';

export const CatSchema = new mongoose.Schema({
  name: String,
  age: Number,
});
```

- Create a new model

```typescript
import { Document } from 'mongoose';

export interface Cat extends Document {
  name: string;
  age: number;
}
```

- Use the model in the service

```typescript
import { Injectable } from '@nestjs/common';
import { InjectModel } from '@nestjs/mongoose';
import { Model } from 'mongoose';
import { Cat } from './cat.model';

@Injectable()
export class CatService {
  constructor(@InjectModel('Cat') private readonly catModel: Model<Cat>) {}

  async findAll(): Promise<Cat[]> {
    return this.catModel.find().exec();
  }
}
```

## GraphQL

- GraphQL is a query language for APIs.

- Install GraphQL

```bash
npm install --save @nestjs/graphql graphql-tools graphql apollo-server-express
```

- Create a new resolver

```typescript
import { Resolver, Query } from '@nestjs/graphql';

@Resolver('Cat')
export class CatResolver {
  @Query()
  async cats() {
    return [];
  }
}
```

- Create a new schema

```typescript
import { buildSchema } from 'type-graphql';

export const schema = buildSchema({
  resolvers: [CatResolver],
});
```

- Add GraphQL to the application

```typescript
import { GraphQLModule } from '@nestjs/graphql';
import { schema } from './schema';

const app = await NestFactory.create(AppModule);

app.use(
  '/graphql',
  GraphQLModule.forRoot({
    schema,
  }),
);

await app.listen(3000);
```

- Open the browser and navigate to `http://localhost:3000/graphql`

## Websockets

- Websockets are used to establish a two-way communication channel between the client and the server.

- Install Websockets

```bash
npm install --save @nestjs/websockets socket.io
```

- Create a new gateway

```typescript
import { WebSocketGateway, WebSocketServer } from '@nestjs/websockets';

@WebSocketGateway()
export class AppGateway {
  @WebSocketServer() server;
}
```

- Use the gateway in the controller

```typescript
import { Controller, Get } from '@nestjs/common';
import { AppGateway } from './app.gateway';

@Controller()
export class AppController {
  constructor(private readonly appGateway: AppGateway) {}

  @Get()
  async findAll(): Promise<string> {
    this.appGateway.server.emit('events', 'Hello World');
    return 'Hello World';
  }
}
```

## Microservices

- Microservices are used to build distributed systems.

- Install Microservices

```bash
npm install --save @nestjs/microservices
```

- Create a new microservice

```typescript
import { NestFactory } from '@nestjs/core';
import { Transport } from '@nestjs/microservices';
import { AppModule } from './app.module';

async function bootstrap() {
  const app = await NestFactory.createMicroservice(AppModule, {
    transport: Transport.TCP,
  });

  app.listen(() => console.log('Microservice is listening'));
}

bootstrap();
```

- Use the microservice in the controller

```typescript
import { Controller, Get } from '@nestjs/common';
import { Client, ClientProxy, Transport } from '@nestjs/microservices';

@Controller()
export class AppController {
  @Client({
    transport: Transport.TCP,
  })
  client: ClientProxy;

  @Get()
  async findAll(): Promise<string> {
    return this.client.send<string>('events', 'Hello World');
  }
}
```

## Testing

- Testing is an important part of the development process.

- Install Jest

```bash
npm install --save @nestjs/testing @nestjs/schematics jest
```

- Create a new test

```typescript
import { Test, TestingModule } from '@nestjs/testing';
import { CatsService } from './cats.service';

describe('CatsService', () => {
  let service: CatsService;

  beforeEach(async () => {
    const module: TestingModule = await Test.createTestingModule({
      providers: [CatsService],
    }).compile();

    service = module.get<CatsService>(CatsService);
  });

  it('should be defined', () => {
    expect(service).toBeDefined();
  });
});
```

- Run the tests

```bash
npm run test
```

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
