# Reactivity adapter for Meteor Tracker

## Adapter

```js
// import in project without meteor
import { Tracker } from 'meteor-ts-tracker'

// import in project with meteor
import { Tracker } from 'meteor/tracker'

import type { ReactivityAdapter } from 'signaldb'

const reactivityAdapter: ReactivityAdapter = {
  create: () => {
    const dep = new Tracker.Dependency()
    return {
      depend: () => {
        if (!Tracker.active) return
        dep.depend()
      },
      notify: () => dep.changed(),
    }
  },
  onDispose: (callback) => {
    if (!Tracker.active) return
    Tracker.onInvalidate(callback)
  },
}
```

## Usage

```js
import { Collection } from 'signaldb'

const posts = new Collection({
  reactivity: reactivityAdapter,
})

Tracker.autorun(() => {
  console.log(posts.find({ author: 'John' }).count())
})
```