---
id: fiberzerolog
---

# Fiberzerolog

![Release](https://img.shields.io/github/v/tag/gofiber/contrib?filter=fiberzerolog*)
[![Discord](https://img.shields.io/discord/704680098577514527?style=flat&label=%F0%9F%92%AC%20discord&color=00ACD7)](https://gofiber.io/discord)
![Test](https://github.com/gofiber/contrib/workflows/Tests/badge.svg)
![Security](https://github.com/gofiber/contrib/workflows/Security/badge.svg)
![Linter](https://github.com/gofiber/contrib/workflows/Linter/badge.svg)

[Zerolog](https://zerolog.io/) logging support for Fiber.

## Install

This middleware supports Fiber v2.

```sh
go get -u github.com/gofiber/fiber/v2
go get -u github.com/gofiber/contrib/fiberzerolog
go get -u github.com/rs/zerolog/log
```

## Signature

```go
fiberzerolog.New(config ...fiberzerolog.Config) fiber.Handler
```

## Config

| Property      | Type                           | Description                                                                                                                                                                   | Default                                                                     |
|:--------------|:-------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:----------------------------------------------------------------------------|
| Next          | `func(*Ctx) bool`              | Define a function to skip this middleware when returned true                                                                                                                  | `nil`                                                                       |
| Logger        | `*zerolog.Logger`               | Add custom zerolog logger.                                                                                                                                                        | `zerolog.New(os.Stderr).With().Timestamp().Logger()`                                                      |
| GetLogger        | `func(*fiber.Ctx) zerolog.Logger`           | Get custom zerolog logger, if it's defined the returned logger will replace the `Logger` value.   | `nil`                                                      |
| Fields        | `[]string`                     | Add fields what you want see.                                                                                                                                                 | `[]string{"latency", "status", "method", "url", "error"}`                            |
| Messages      | `[]string`                     | Custom response messages.                                                                                                                                                     | `[]string{"Server error", "Client error", "Success"}`                       |
| Levels        | `[]zerolog.Level`              | Custom response levels.                                                                                                                                                       | `[]zerolog.Level{zerolog.ErrorLevel, zerolog.WarnLevel, zerolog.InfoLevel}` |
| SkipURIs      | `[]string`                     | Skip logging these URI.                                                                                                                                                       | `[]string{}`                                                                |
| GetResBody    | func(c *fiber.Ctx) []byte      | Define a function to get response body when return non-nil.<br />eg: When use compress middleware, resBody is unreadable. you can set GetResBody func to get readable resBody.  | `nil`                                                                       |

## Example

```go
package main

import (
    "github.com/gofiber/fiber/v2"
    "github.com/gofiber/contrib/fiberzerolog"
    "github.com/rs/zerolog"
)

func main() {
    app := fiber.New()
    logger := zerolog.New(os.Stderr).With().Timestamp().Logger()

    app.Use(fiberzerolog.New(fiberzerolog.Config{
        Logger: &logger,
    }))

    app.Get("/", func (c *fiber.Ctx) error {
        return c.SendString("Hello, World!")
    })

    if err := app.Listen(":3000"); err != nil {
        logger.Fatal().Err(err).Msg("Fiber app error")
    }
}
```
