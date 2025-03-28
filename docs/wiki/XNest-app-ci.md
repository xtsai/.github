# XNestjs integrated

> 

## base env files

```bash
mkdir -p .conf/dev .conf/prod docs/wiki
# merge .gitignore

```

## Install XNestjs application env library

```bash
pnpm add -D @types/js-yaml chalk@4.1.2 cross-env rimraf
```

## Install xtsai dependencies library

1. add base libs

```bash
# app base config & swagger
pnpm add @nestjs/config @nestjs/core@10.0.0 @nestjs/swagger@8.1.0 class-transformer@0.5.1 class-validator@0.14.1 helmet@8.0.0 js-yaml iterare cookie-parser express@4.18.1
```

2. install xai library

> copy conf/*.yml

```sql
/* CREATE DB */
CREATE DATABASE IF NOT EXISTS `xtsai-db` CHARACTER SET 'utf8mb4' COLLATE 'utf8mb4_general_ci';

GRANT ALL PRIVILEGES ON `xtsai-db`.* TO 'admin'@'%';
FLUSH  PRIVILEGES;

```

```bash
# MQ
pnpm add @tsailab/ioredis-mq ioredis@5.4.1
```

3. create share/auth/api/ module

```bash
# global share mq
nest g mo share

# JWT auth
nest g mo auth
# backend api
nest g mo api
nest g mo admin api
```

3. edit main.ts

> main.ts

```ts
  const app = await NestFactory.create(AppModule);

  //允许跨域请求
  app.enableCors();
  // Web漏洞的
  app.use(helmet());

  const configService = app.get(ConfigService);
  const appPort = configService.get<number>('app.server.port', 18945);

  const apiPrefix = configService.get<string>('app.prefix', 'v1');
  app.setGlobalPrefix(apiPrefix, {
    exclude: [
      {
        path: 'health',
        method: RequestMethod.GET,
      },
    ],
  });
  await app.listen(appPort, '0.0.0.0');
```

## Integrate xtsai system & core

4. integrated xtsai

```bash
# common
pnpm add @tsailab/core-types @xtsai/xai-utils bcrypt@5.1.1 date-fns@4.1.0 eckey-utils@0.7.14 ms@3.0.0-canary.1 nanoid@3.3.4

# core typeorm
pnpm add @nestjs/typeorm@10.0.2 typeorm@0.3.20 mysql2@3.12.0
pnpm add @xtsai/core @xtsai/system

# jwt auth
pnpm add @nestjs/jwt@10.2.0 @nestjs/passport@10.0.3 passport passport-jwt svg-captcha-fixed

pnpm add -D @types/passport-jwt
```

5. app 

> main.ts

```ts
  // Validations
  app.useGlobalPipes(new ValidationPipe({ transform: true }));
  // Filter Exceptions
  app.useGlobalFilters(new HttpExceptionFilter());

```

6. integrate system

```ts
//imports
imports:[
    TypeOrmModule.forRootAsync({
      useClass: MysqlTypeormOptionFactory,
      inject: [ConfigService],
    }),
    SystemModule.forRoot(true),
]

```