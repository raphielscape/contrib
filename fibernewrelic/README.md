---
id: fibernewrelic
---

# Fibernewrelic

![Release](https://img.shields.io/github/v/tag/gofiber/contrib?filter=fibernewrelic*)
[![Discord](https://img.shields.io/discord/704680098577514527?style=flat&label=%F0%9F%92%AC%20discord&color=00ACD7)](https://gofiber.io/discord)
![Test](https://github.com/gofiber/contrib/workflows/Tests/badge.svg)
![Security](https://github.com/gofiber/contrib/workflows/Security/badge.svg)
![Linter](https://github.com/gofiber/contrib/workflows/Linter/badge.svg)

[NewRelic](https://github.com/newrelic/go-agent) support for Fiber.

## Install

```
go get -u github.com/gofiber/fiber/v2
go get -u github.com/gofiber/contrib/fibernewrelic
```

## Signature

```go
fibernewrelic.New(config fibernewrelic.Config) fiber.Handler
```

## Config

| Property          | Type             | Description                            | Default        |
|:------------------|:-----------------|:---------------------------------------|:---------------|
| License           | `string`         | Required - New Relic License Key       | `""`           |
| AppName           | `string`         | New Relic Application Name             | `fiber-api`    |
| Enabled           | `bool`           | Enable/Disable New Relic               | `false`        |
| ~~TransportType~~ | ~~`string`~~     | ~~Can be HTTP or HTTPS~~ (Deprecated)  | ~~`"HTTP"`~~   |
| Application       | `Application`    | Existing New Relic App                 | `nil`          |
| ErrorStatusCodeHandler       | `func(c *fiber.Ctx, err error) int`    | If you want to change newrelic status code, you can use it.                 | `DefaultErrorStatusCodeHandler`          |

## Usage

```go
package main

import (
	"github.com/gofiber/fiber/v2"
	"github.com/gofiber/contrib/fibernewrelic"
)

func main() {
	app := fiber.New()

	app.Get("/", func(ctx *fiber.Ctx) error {
		return ctx.SendStatus(200)
	})

	cfg := fibernewrelic.Config{
		License:       "FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF",
		AppName:       "MyCustomApi",
		Enabled:       true,
	}

	app.Use(fibernewrelic.New(cfg))

	app.Listen(":8080")
}
```

## Usage with existing New Relic application

```go
package main

import (
	"github.com/gofiber/fiber/v2"
	"github.com/gofiber/contrib/fibernewrelic"
	"github.com/newrelic/go-agent/v3/newrelic"
)

func main() {
	newrelicApp, err := newrelic.NewApplication(
		newrelic.ConfigAppName("MyCustomApi"),
		newrelic.ConfigLicense("FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF"),
		newrelic.ConfigEnabled(true),
	)

	app := fiber.New()

	app.Get("/", func(ctx *fiber.Ctx) error {
		return ctx.SendStatus(200)
	})

	cfg := fibernewrelic.Config{
		Application:       newrelicApp
	}

	app.Use(fibernewrelic.New(cfg))

	app.Listen(":8080")
}
```
