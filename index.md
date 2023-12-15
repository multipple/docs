# Multipple Extensions

Extensions are pieces of codes purposely design to add and execute custom and independent functionalities in Multipple platform.

There are two ways to extend Multipple:
  -	Frontend extensions using UI rendering engines
  -	Backend extensions using edge functions

## Frontend extensions

Frontend extensions are UI components capable to execute and renderer a subset of application within Multipple’s platform. We aim to support every UI rendering engine (ReactJS, VueJS, AngularJS, MarkoJS, …) out there so that every frontend developer could easily build extensions using the UI engine they are comfortable with.

> Note: Currently, because Multipple’s frontend application is build using MarkoJS engine, default available boilerplate to implement an extension is in MarkoJS.</blockquote>


The strategy is to keep all required features, syntax and structure as stated in their documentation, then inject a set of meta functions into the component that will make it perform as extension.

Here is a simple sample code of markoJS extension:

```marko
// index.marko
class {
  onCreate({ Declare }){
    Declare(this)
  }
}

<h1>Hello world!</h1>
```

Extension meta functions & properties are injected as imputs to the component at creation. Hence, they are available as object argument of the component's `onCreate()` lifecycle method.

## List of extension input/props

### meta
**type**: *object*\
**value**: Refer to [meta options]()

Contain extension information. These info are defined during implementation of the extension in the `.metadata` file.

### Declare(this, […resources])
**type**: *function*

It's a runtime function when called transform a regular component into extension by adding super features to the main component's `this`. These features allow the component to access and perform direct or indirect operations on Multipple platform as extension plugin or application.
  - `this.App`: *Object* Provide application features to the extension
  - `this.Plugin`: *Object* Provide plugin features to the extension

#### Extension features
  - `.use()`: 
  - `.extends()`: 
  - `.getConfig()`: 
  - `.setConfig()`: 
  - `.emit()`: 
  - `.quit()`: 
  - `.refresh()`: 
  - `.debug()`: 
  - `.signal()`: 
  - `.features`: 
  - `.deps`: 
  - `.data`: 

### Extesion resources
  - **Share State**
  
  Enable a shared state between main and all subcomponents of the extension
  ```JS
    /* index.marko */

    // Initialize a state
    this.App.State.init({ name: 'Bob' })

    // Set new state
    this.App.State.set('username', 'dupont')

    // Get state value
    this.App.State.get('username')

    // Set state value and force component to re-render
    this.App.State.dirty('username')

    // Define a new state API
    this.App.State.define('user')
    .action('profile', async () => { /* Retrieve user profile data */ })
    .action('update', async () => { /* Update user data */ })

    // Listen to event anytime a state key change
    this.App.State.on('name', value => { /* Do something */ })
    // Listen to event once a state key change
    this.App.State.once('name', value => { /* Do something */ })
    // Remove existing event listener
    this.App.State.off('name', value => { /* Do something */ })
  ```

  Accessing the State from a sub-component
  ```JS
    /* sub-component.marko */

    // Share state keys with a sub-component
    this.App.State.share( this, ['name', 'username'])
    // Unshare state keys with a sub-component
    this.App.State.unshare( this, ['username'])
  ```

  - **UI Store**

  Enable access browser/webview local storage
  ```JS
    // Store a new value
    this.App.UIStore.set('username', 'dupont')
    // Temporaty store a new value: Get automatically cleared when stored value is read
    this.App.UIStore.flash('name', 'Bob')
    // Update stored dataset: Array
    this.App.UIStore.set('roles', ['USER', 'ADMIN'])
    this.App.UIStore.update('roles', 'MANAGER', 'push') // Push update to array
    this.App.UIStore.update('roles', 'MANAGER', 'pop') // Remove stored array's last item

    // Get stored value
    console.log( this.App.UIStore.get('username') ) // dupont
    console.log( this.App.UIStore.get('roles') ) // USER, ADMIN

    // Delete a stored value
    this.App.UIStore.clear('username')
    this.App.UIStore.clear(['name', 'roles'])
  ```

  - **Request**

  Enable extension to consume multipple API and send request to external services or API providers.

  ```JS
    const options = {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json'
      },
      body: {
        name: 'dupont'
      }
    }

    // Send request
    try {
      const courses = await this.Plugin.Request('/courses', options )

      // Do something ...
    }
    catch( error ){ /* Handle request error here ...*/ }
  ```

  - **Permissions**:

  Request permission to access a resourse on multipple platform
  ```JS
    // Return the list of permissions listed in `.metadata.resource.permissions`
    await this.App.Permission.getScope()

    // Set/Update a new permissions scope to `.metadata.resource.permissions`
    await this.App.Permission.setScope('user:*')
    
    // Manually ask permission scope and wait for granted list
    await this.App.Permission.ask('permission', ['tenant:name']) // Return true or false

    // 
    await this.App.Permission.mandatory(['tenant.apps'])
  ```
  
  - **Notifications**:
  - **Database**:
  - **String**:

### payload
**type**: *object*

It's the input data provided to the extension during usage

### theme
**type**: *string*\
**value**: `light` | `dark`

This tells which is the current theme mode of multipple UI. Usefull for extension to adapt to user's theme mode.
> Note: For frontend extension only

### screen
**type**: *string*\
**value**: `xs` | `sm` | `md` | `lg` | `xl`

This tells the currenty device screen size being use by the workspace. Usefull for UI responsiveness
  - `xs`: Extra-small screen size
  - `sm`: Small screen size
  - `md`: Medium screen size
  - `lg`: Lage screen size
  - `xl`: Extra-large screen size

> Note: For frontend extension only

### mode
**type**: *string*\
**value**: `qs` | `hs` | `fs`

This tells what screen size is allocated to the extension display in the workspace
  - `qs`: Quarter screen mode
  - `hs`: Half screen mode
  - `fs`: Full screen mode

> Note: For frontend extension only

### context
**type**: *string*

Given execution context to run the extension with. On the frontend, it represent the main page on which the user is on. If user is browsing courses page, then context is `course`. This help an extension to operate responsively to different pages of the app.

> Use case: **SmartSearch** extension use context to auto apply determine query target from one page to another. When user is on courses page, SmartSearch direct search to courses and when user browse to programs, it automatically switch search queries to programs.

Context application for backend extension are yet to be defined.

### user
**type**: *object*\
**value**: Refer to [user data]()

Provide the current session's user data. For privacy policies, It's only available when extension request `user.*` permission. This permission can be define in `.metadata.resource.permissions` and is `granted` or `denied` by the user during installation of an extension.

### tenant
**type**: *object*\
**value**: Refer to [tenant data]()

Provide the current tenant/institution data. For privacy policies, It's only available when extension request `tenant.*` permission. This permission can be define in `.metadata.resource.permissions` and is `granted` or `denied` only by admistrator user during installation of an extension.
