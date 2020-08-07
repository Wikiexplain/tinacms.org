---
title: Set Up the CMS
prev: /docs/getting-started/introduction
next: /docs/getting-started/edit-content
last_edited: '2020-08-07T18:19:19.970Z'
---
In this step we will install `tinacms`, create a CMS instance, wrap our app in the `TinaProvider`, and configure the CMS options.

## Install _tinacms_

Install the `tinacms` package. This is the main package that you will use to create your CMS. You'll also need to install `styled-components` to render the editing UI properly.

```bash,copy
yarn add tinacms styled-components
```

## Create a CMS instance, add _TinaProvider_

The first step to setting up Tina is to create an [instance of `TinaCMS`](/docs/cms). Once created, this [CMS object](/docs/cms#setting-up-the-cms-object) allows us to manage [Plugins](/docs/plugins), [APIs](/docs/apis), [Media Stores](/docs/media), [Events](/docs/events), [Editing UI](/docs/ui) and other key aspects of content management.

Next, wrap your site in the `TinaProvider`. This [provider](https://github.com/tinacms/tinacms/blob/master/packages/tinacms/src/components/TinaProvider.tsx) sets up the editing UI, and provides a [context](https://reactjs.org/docs/context.html) from which we can access the CMS throughout the app.

Open up `src/App.js` and follow these steps:

### The Steps

1. Import `TinaProvider` & `TinaCMS`
2. Create an instance of `TinaCMS`
3. Wrap `TinaProvider` around the contents of `App`, pass the `cms`.

**src/App.js**

```diff
import React from 'react';
+import { TinaProvider, TinaCMS } from 'tinacms';
import logo from './Icon.svg';
import './App.css';

function App() {
+ const cms = new TinaCMS();
  return (
+   <TinaProvider cms={cms}>
      <div className="App">
        <PageContent />
      </div>
+   </TinaProvider>
  );
}

export default App;

//...
```

> 🦙 After this step, the CMS will still not be accessible. We will enable the CMS in a few steps.

## Configure the CMS

When instantiating the CMS, you can pass [options](/docs/cms#cms-configuration) to configure how the [CMS options](/docs/cms#cms-configuration) works.

For this demo, we will be updating our CMS instance with the `sidebar` enabled, which renders forms, menus, and other UI that connect with content editing actions. 

### The Steps

1. Update your CMS instance with the `sidebar` option:

**src/App.js**

```js,copy
function App() {
  const cms = new TinaCMS({
    sidebar: true,
  })
  return (
      //...
  )
}
```

> 🦙 The CMS will not render unless the CMS is enabled.

## Enabling the CMS

> 🦙 The **CMS** **is** _**disabled by default**_. We need to enable the CMS to edit content.

There are various approaches to enable the CMS. One way is to set the `enabled` option in the [CMS configuration](/docs/cms#cms-configuration). Another is to use a `toggle` method to [enable / disable](/docs/cms#disabling--enabling-the-cms) the CMS.

Our demo contains an **"Edit this Site"** button, but you may have noticed that it doesn't do anything right now. Let's wire up this button to enable Tina using the CMS' `toggle` method.

We'll use the `useCMS` hook to get the CMS object from the `TinaProvider`. This hook can be useful throughout your App when needing to [access or update](/docs/cms#accessing-the-cms-object) the CMS. Once we can access the `cms`, we can toggle the enabled state.

### The Steps

1. Import the `useCMS` hook.
2. In the `EditButton` component: toggle the `cms` state on click and render different button text depending on the enabled state.

**src/App.js**

```diff
import React from 'react';
-import { TinaProvider, TinaCMS } from 'tinacms';
+import { TinaProvider, TinaCMS, useCMS } from 'tinacms';
import logo from './Icon.svg';
import './App.css';

//...

function EditButton() {
+ const cms = useCMS();
  return (
-   <button onClick={() => window.alert("Tina isn't configured yet!")}>
-     Edit This Site
+   <button onClick={() => cms.toggle()}>
+    {cms.enabled ? 'Exit Edit Mode' : 'Edit This Site'}
    </button>
  );
}
```

Head back to http://localhost:3000, refresh the page and click the 'Edit This Site' button. You should see an 'Edit Icon' in the lower right-hand corner. Click on the icon to open the _[Sidebar](/docs/ui#toolbar-configuration)_.

![tinacms-tutorial-cms-setup-step](/img/getting-started/cms-setup-step.png)

You'll notice that the sidebar is empty, that is because there are no forms registered to edit content — let's do that next.

## Additional Reading

* Explore additional options for configuring the [Sidebar UI](/docs/ui#sidebar-configuration).
* The example above uses `cms.toggle` to switch between enabled/disabled states. Checkout all of the [CMS methods](/docs/cms#reference) available.
* Another important editing UI is the _[Toolbar](/docs/ui)_ — it is useful with [Inline Editing](/docs/ui/inline-editing).