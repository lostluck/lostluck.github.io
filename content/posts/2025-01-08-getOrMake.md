---
title: "Tutorial: GetOrMake for maps."
slug: tutorial-getormake
date: 2025-01-08T22:06:58-08:00
tags: 
 - golang
 - tutorial
categories: 
 - Go
 - Tutorial
---

Today, I'm going to quickly go over one of my favorite convenience functions,
made possible with Go's generics. It's invaluable if you deal with maps of maps in Go.

<!--more-->

### Maps

Maps are one of the original "generic" types available to Go users. If you aren't
familiar with them, you can learn about them in [Effective Go](https://go.dev/doc/effective_go#maps).

The important bit is that the key type must be `comparable`, which is a special
built-in interface that defines all comparable types.
See https://pkg.go.dev/builtin#comparable for more details on that.

### Nested Maps

However, the values of those maps can be any Go type at all, including any other map.

```go
// nested maps, two deep.
var mapOfMaps map[string]map[string]string
```

And the values for those nested maps can also be maps.

```go

// nested maps, three deep.
var mapOfMapsOfMaps map[string]map[string]map[string]string
```

### Setting nested maps

But this nesting has a downside. What if you want to get to that final map so
you can set the value on its key?

Say it's a field Map on a struct, and we have a method to set the innermost value.
The code would then need to initialize all the maps along the way to setting the value.

```go

func (w *mWrapper) Set(k1, k2, k3, value string) {
	if w.Map == nil {
		w.Map = make(map[string]map[string]map[string]string)
	}
	v1, ok := w.Map[k1]
	if !ok {
		v1 = make(map[string]map[string]string)
		w.Map[k1] = v1
	}
	v2, ok := v1[k2]
	if !ok {
		v2 = make(map[string]string)
		v1[k2] = v2
	}
	v2[k3] = value
}
```

What's happening here is that for each level with an inner map,
we need to check if that map exists. We use the "comma ok" idiom, 
where the value of ok indicates if the map has a value for the given key or not.
If it doesn't exist, we make a new instance of the map type and assign it both
to the value variable and to the key in the map itself.

As you can imagine, the more layers of nesting you have, the more tedious
creating the earlier nested map types becomes.

### Generics to the rescue!

Using generics, we can avoid some of this!

```go
func (w *mWrapper) Set(k1, k2, k3, value string) {
	if w.Map == nil {
		w.Map = make(map[string]map[string]map[string]string)
	}
	v1 := getOrMake(w.Map, k1)
	v2 := getOrMake(v1, k2)
	v2[k3] = value
}
```

Now it's eight lines shorter! Four per level we've removed, including the
tedious repetition of the nested type.

Here's the code for getOrMake.

```go
// getOrMake is a generic helper function for extracting or initializing a sub-map.
func getOrMake[K, VK comparable, VV any, V map[VK]VV, M map[K]V](m M, key K) V {
	v, ok := m[key]
	if !ok {
		v = make(V)
		m[key] = v
	}
	return v
}
```

First, we've declared our generic types:

K is the key for the outermost map.
VK is the key for the value map.
VV is the value for the value map.
V is the type of the value map: map[VK]VV
M is the type of the map being passed in: map[K]V
The function then takes in an instance m of type M and the key of type K,
returning an instance of the value map.

Then the code is pretty straightforward.

We look up the value of the key and whether it exists or not.
If the key has no value, then we do as we did before:
create an instance of the value map and assign that to the key.
Then we return the value itself.

I wouldn't pull out getOrMake the first time I need it, and it only works for maps.
But if I start repeating the pattern around in a package, or I start to play
with the inner types, using getOrMake can reduce some effort around that refactoring.

### Aside: Top-Level Maps

It also can't work for top-level maps. For example:

```go
func NoArgMakeMap[K comparable, V any, M map[K]V]() M {
	return make(M)
}
```

That doesn't work, since Go's type inference doesn't pull from the return value,
even if it would be helpful.

The closest you can get is to explicitly hint at the type by including an unused
parameter.

```go
func OneArgMakeMap[K comparable, V any, M map[K]V](_ M) M {
	return make(M)
}
```

That'll be enough to have the compiler do the right thing.

There is at least one issue in the Go issue tracker for this feature request,
but no real proposals at this time.

I don't think it's critical for Go to get this, though, since the workaround
isn't terrible at this point.

### Conclusion

Generics in Go are a powerful tool to reduce boilerplate, even with small
utility functions, especially with Go's original generic types, like map!

Thanks for reading.