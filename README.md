# ewen.io

Source code for [my site](https://ewen.io), based on
[devinschulz/blog](https://github.com/devinschulz/blog).

The underlying tech of this website uses the static site generator
[Hugo](https://gohugo.io/). All styles are handled by
[Tailwind CSS](https://tailwindcss.com), and a minimal amount of JavaScript is
utilized throughout.

## Installation

```shell
npm install
```

The Hugo binary is managed by `hugo-bin`, so there you don't have to worry about
having it installed globally.

## Running in development mode

```shell
npm start
```

## Building the app for production

```shell
npm run build
```

## Notes

Create a new post as a page bundle:

```shell
hugo new --kind post-bundle posts/post-name
```
