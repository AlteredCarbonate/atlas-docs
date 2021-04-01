---
description: Learn more about Configuration
---

# Configuration

## Introduction

We created a very small and simple to use configuration service. This service is used inside the framework. If you want, you can use it too.

{% hint style="info" %}
We are not doing any fancy magic stuff here. Thanks go to [`Lodash`](https://lodash.com/)
{% endhint %}

## Base Configuration

The base configuration is our minimal setup to run all the framework stuff. Keep in mind, if you're not using Discord, don't remove the params. They will not be used but to prevent compile errors on your side. It is better to keep them.

```javascript
{
  "database": {
    "name": "default",
    "type": "mysql",
    "host": "altv-mysql",
    "port": 3306,
    "username": "gamemode",
    "password": "demo123",
    "database": "gamemode",
    "synchronize": true,
    "logging": false
  },
  "discord": {
    "client_id": "",
    "client_secret": "",
    "bot_secret": "",
    "server_id": "",
    "redirect_url": "http://127.0.0.1:1337/auth/discord",
    "auth_url": "https://discord.com/api/oauth2/authorize",
    "auth_token_url": "https://discord.com/api/oauth2/token",
    "user_me_url": "https://discord.com/api/users/@me"
  }
}
```

## Get Config Parameter

Getting a parameter from the config file is quite simple. Use the power of Dependency Injection, and you're ready to go.

{% hint style="success" %}
You can get nested elements by seperating the child with a dot.
{% endhint %}

```javascript
export class MyComponent {

    // Load with dependency injection
    constructor(
        private readonly configService: ConfigService
    ) {}

    // Work with the resolved class
    public yourMethod(): void {
        const myValue = this.configService.get('discord.auth_token_url');
        // myValue is now the value of auth_token_url
    }
}
```

{% hint style="success" %}
You can retrieve a config value with all nested elements if you require it.
{% endhint %}

```javascript
export class MyComponent {

    // Load with dependency injection
    constructor(
        private readonly configService: ConfigService
    ) {}

    // Work with the resolved class
    public yourMethod(): void {
        const myValue = this.configService.get('discord');
        // myValue is now the whole discord config object
    }
}
```

{% hint style="success" %}
In some cases, you want to get the default value. For example, when your required param is empty or does not exist.
{% endhint %}

```javascript
export class MyComponent {

    // Load with dependency injection
    constructor(
        private readonly configService: ConfigService
    ) {}

    // Work with the resolved class
    public yourMethod(): void {
        const myValue = this.configService.get('param.not.exists', 'defaultValue');
        // myValue is now the string 'defaultValue'
    }
}
```

## Set Config Parameter

It is possible to create your own parameters or complete objects if you want. To prevent overriding the required base configuration, your custom configuration is stored inside its own scope. They are automatically merged, if you want to get a value from the config service.

```typescript
export class MyComponent {
    // Load with dependency injection
    constructor(
        private readonly configService: ConfigService
    ) {}

    // Work with the resolved class
    public yourMethod(): void {
        this.configService.set('hello', 'world');
        // now you have created the config key 'hello' with the value 'world'
    }
}
```

{% hint style="info" %}
Keep in mind, nesting works too. You can nest your params as deep as you want, the ConfigService will handle it all for you.
{% endhint %}
