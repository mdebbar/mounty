Mounting can be done through functions, and those functions are invoked at the moment the path is matched. That provides the flexibility to decide the mounting behavior at runtime.

```javascript
function mountContent() {
  return isPremiumUser() ? <PremiumContent /> : <FreeContent />
}

mounty('/', {
  'path': () => isPremiumUser() ? <PremiumContent /> : <FreeContent />,
  'profile/{id}': (route) => <ProfilePage id={route.params.id} />
})
```

We could even go crazier :-)
```javascript
function mountContent() {
  if (isPremiumUser()) {
    // Async mounting.
    return fetchSubscriptionLevel().then(level => {
      switch (level) {
        case 'silver': return <SilverContent />
        case 'gold': return <GoldContent />
        case 'platinum':
          // Lazy-load the PlatinumContent component using Webpack 2.0
          // See the lazy loading docs for more details.
          return System.import('./components/PlatinumContent').then(
            PlatinumContent => <PlatinumContent />
          )
      }
    })
  } else {
    // Immediate mounting.
    return <FreeContent />
  }
}

mounty('/', {
  'path': mountContent
})
```
