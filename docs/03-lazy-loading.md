In large apps, lazy loading is a necessity. It's just impractical to load the whole application code on initial load. There needs to be an easy way to split the code and lazy-load it on demand.

Fortunately, doing lazy-loading is extremely easy using async mounting. We will demonstrate an example using Webpack's code splitting:

```javascript
function mountProfilePage() {
  return new Promise((resolve) => {
    require.ensure([], (require) => {
      const App = require('./components/App')
      resolve(<App />)
    })
  })
}

mounty('/', {
  'profile': mountProfilePage,
})
```

In Webpack 2.0:
```javascript
function mountProfilePage() {
  return System.import('./components/App').then(App => <App />)
}
```
