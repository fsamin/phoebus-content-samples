---
title: "Go Basics Quiz"
type: quiz
estimated_duration: "10m"
---

# Go Basics Quiz

## [multiple-choice] What is the zero value of a `string` in Go?

- [ ] `nil`
- [ ] `"null"`
- [x] `""` (empty string)
- [ ] `0`

> **Explanation:** Every type in Go has a zero value. For `string` it's `""`, for `int` it's `0`, for `bool` it's `false`, and for pointers/slices/maps it's `nil`.

## [multiple-choice] Which declaration is only valid inside a function?

- [ ] `var x = 10`
- [x] `x := 10`
- [ ] `const x = 10`
- [ ] `var x int`

> **Explanation:** The short variable declaration `:=` can only be used inside functions. Package-level variables must use `var`.

## [short-answer] What keyword is used for all loops in Go?

    for

> **Explanation:** Go has only one looping construct: `for`. It covers classic for loops, while-style loops (`for condition {}`), infinite loops (`for {}`), and range-based iteration (`for i, v := range slice {}`).

## [multiple-choice] What happens if you import a package but don't use it?

- [ ] A runtime warning is logged
- [ ] The import is silently ignored
- [x] The program fails to compile
- [ ] The unused package is automatically removed

> **Explanation:** Go treats unused imports as compile errors. This enforces clean dependency hygiene. Use `_` as alias to import a package for its side effects only: `import _ "net/http/pprof"`.

## [multiple-choice] How do you check if a key exists in a map?

- [ ] `if config.has("key") {}`
- [ ] `if config["key"] != nil {}`
- [x] `if value, ok := config["key"]; ok {}`
- [ ] `if key in config {}`

> **Explanation:** Go uses the "comma ok" idiom: `value, ok := m[key]`. If the key exists, `ok` is `true` and `value` contains the value. If not, `ok` is `false` and `value` is the zero value for the map's value type.

## [multiple-choice] What does `go mod tidy` do?

- [ ] Formats all Go source files
- [ ] Runs the test suite
- [x] Adds missing and removes unused module dependencies
- [ ] Updates all dependencies to their latest versions

> **Explanation:** `go mod tidy` ensures `go.mod` and `go.sum` are consistent with the source code. It adds any imported but missing modules and removes modules that are no longer imported. To update dependencies, use `go get -u`.
