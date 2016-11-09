The main idea is that the developer can "mount" a React component depending on the current path. Mounting can be done in a few ways:


1. Constant mounting
--------------------
This is the simplest way to mount a component on a path. Basically just attach a React component directly to the path.

```javascript
mounty('/', {
  'home': <HomePage />
})
```


2. Dynamic mounting:
--------------------
You can also provide a function that performs the mounting whenever the path is matched. The function just returns the React component to be mounted/rendered.

```javascript
mounty('/', {
  'profile/{id}': (route) => <ProfilePage id={route.params.id} />
})
```


3. Nested mounting
------------------
This is useful expressing a hierarchy of routes and sub-routes e.g. `/settings/account`, `/settings/security`, `/settings/notifications`, etc.

```javascript
mounty('/', {
  'settings': {
    'account': <AccountSettings />,
    'security': <SecuritySettings />,
    'notifications': {
      // Deeper nesting
      'desktop': <DesktopNotificationSettings />,
      'mobile': <MobileNotificationSettings />
    }
  }
})
```


4. Async mounting
-----------------
In cases where the app needs remote data to decide how to mount, you can return a promise from the `mount` function.

```javascript
function mountPath() {
  // Assuming `fetchData` returns a promise.
  return fetchData().then(function(data) {
    if (data.mountA) {
      return <ComponentA />
    } else {
      return <ComponentB />
    }
  })
}

mounty('/', {
  'path': mountPath
})
```
Async mounting can also prove useful for code splitting and lazy loading. Some examples are discussed in the [`lazy loading` docs](03-lazy-loading.md).
