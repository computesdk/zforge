# ZForge

A go-specific framework for building web UIs with zero node dependencies. ZForge provides a type-safe, component-based approach to generating HTML and CSS using a Tailwind-like utility system.

## Features

- **Type-safe HTML generation** - Build HTML elements using Go structs and methods
- **Utility-first CSS** - Tailwind-inspired CSS utilities with automatic class tracking
- **Zero dependencies** - Pure Go implementation with no Node.js requirements
- **Minimal CSS output** - Only generates CSS for classes actually used
- **Component-based** - Compose UIs using reusable element structures

## Quick Start

```go
package main

import (
    "fmt"
    "net/http"
    
    "github.com/computesdk/zforge/css"
    "github.com/computesdk/zforge/html"
)

func main() {
    // Reset CSS tracking for new request
    css.ResetTracking()
    
    // Build HTML with utility classes
    page := html.Html(
        html.Head(
            html.Title("My App"),
        ),
        html.Body().Class(css.BgGray(100), css.MinH("screen")).AddChildren(
            html.Div().Class(css.P(8), css.MaxW("4xl"), css.Mx(0)).AddChildren(
                html.H1("Welcome to ZForge").Class(css.Text4xl(), css.FontBold()),
                html.P("Build web UIs with Go").Class(css.TextGray(600)),
            ),
        ),
    )
    
    // Render includes automatic CSS injection
    html := page.Render()
    fmt.Println(html)
}
```

## HTML Elements

Create HTML elements using the provided functions:

```go
// Document structure
html.Html(children...)
html.Head(children...)
html.Body(children...)
html.Title("Page Title")

// Common elements
html.Div().Class(css.P(4))
html.H1("Heading").Class(css.TextXl())
html.P("Paragraph text")
html.A("Link text").Attr("href", "/path")

// Method chaining
element := html.Div().
    Class(css.BgBlue(500), css.P(4)).
    ID("my-element").
    Attr("data-value", "123").
    AddChildren(
        html.P("Child content"),
    )
```

## CSS Utilities

ZForge provides Tailwind-inspired utility classes:

```go
// Spacing
css.P(4)        // padding: 1rem
css.M(8)        // margin: 2rem
css.Px(6)       // padding-left/right: 1.5rem

// Colors
css.BgBlue(500)    // background-color: blue-500
css.TextGray(800)  // color: gray-800

// Layout
css.Flex()         // display: flex
css.Grid()         // display: grid
css.Block()        // display: block

// Typography
css.TextXl()       // font-size: 1.25rem
css.FontBold()     // font-weight: 700
css.TextCenter()   // text-align: center

// Sizing
css.W("full")      // width: 100%
css.H("screen")    // height: 100vh
css.MaxW("4xl")    // max-width: 56rem
```

## HTTP Server Example

```go
func (s *Server) handleHome(w http.ResponseWriter, r *http.Request) {
    // Reset CSS tracking for each request
    css.ResetTracking()

    page := html.Html(
        html.Head(
            html.Title("My App"),
        ),
        html.Body().Class(css.BgGray(50), css.MinH("screen")).AddChildren(
            html.Div().Class(css.Container(), css.Mx(0), css.P(8)).AddChildren(
                html.H1("Dashboard").Class(css.Text4xl(), css.FontBold()),
                html.Div().Class(css.Grid(), css.Gap(4)).AddChildren(
                    html.Div().Class(css.BgWhite(), css.P(6), css.Rounded(8)).AddChildren(
                        html.H2("Card Title").Class(css.TextXl()),
                        html.P("Card content"),
                    ),
                ),
            ),
        ),
    )

    // Render automatically injects minimal CSS
    w.Header().Set("Content-Type", "text/html; charset=utf-8")
    fmt.Fprint(w, page.Render())
}
```

## CSS Generation

ZForge automatically tracks which CSS classes are used and generates minimal CSS:

```go
// Track classes as they're used
css.BgBlue(500)  // Registers "bg-blue-500"
css.P(4)         // Registers "p-4"

// Get all used classes
classes := css.GetUsedClasses()

// Generate minimal CSS (only for used classes)
stylesheet := css.GenerateMinimalCSS()
cssOutput := stylesheet.Generate()

// Generate full utility CSS
fullStylesheet := css.GenerateUtilities()
allCSS := fullStylesheet.Generate()

// Reset tracking (typically done per request)
css.ResetTracking()
```

## Architecture

- **css/**: Utility class generation and CSS output
- **html/**: HTML element creation and rendering
- **css/internal/**: Configuration-driven CSS generation from YAML files

The framework uses YAML configuration files to define utility classes, making it easy to extend and customize the available CSS utilities.

## Contributing

ZForge is designed to be minimal and focused. Contributions should maintain the zero-dependency philosophy and type-safe approach.
