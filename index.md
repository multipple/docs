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
**value**: Refer to [meta options]()\
Contain extension information. These info are defined during implementation of the extension in the `.metadata` file.

### Declare(this, […resources])
**type**: *function*\
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
  - **Share State**:
  - **UI Store**:
  - **Request**:
  - **Permissions**:
  - **Notifications**:
  - **Database**:
  - **String**:

### payload
**type**: *object*\
It's the input data provided to the extension during usage

### theme
**type**: *string*\
**value**: `light` | `dark`\
This tells which is the current theme mode of multipple UI. Usefull for extension to adapt to user's theme mode.
> Note: For frontend extension only

### screen
**type**: *string*\
**value**: `xs` | `sm` | `md` | `lg` | `xl`\
This tells the currenty device screen size being use by the workspace. Usefull for UI responsiveness
  - `xs`: Extra-small screen size
  - `sm`: Small screen size
  - `md`: Medium screen size
  - `lg`: Lage screen size
  - `xl`: Extra-large screen size

> Note: For frontend extension only

### mode
**type**: *string*\
**value**: `qs` | `hs` | `fs`\
This tells what screen size is allocated to the extension display in the workspace
  - `qs`: Quarter screen mode
  - `hs`: Half screen mode
  - `fs`: Full screen mode

> Note: For frontend extension only

### context
**type**: *string*\
Given execution context to run the extension with. On the frontend, it represent the main page on which the user is on. If user is browsing courses page, then context is `course`. This help an extension to operate responsively to different pages of the app.

> Use case: **SmartSearch** extension use context to auto apply determine query target from one page to another. When user is on courses page, SmartSearch direct search to courses and when user browse to programs, it automatically switch search queries to programs.

Context application for backend extension are yet to be defined.

### user
**type**: *object*\
**value**: Refer to [user data]()\
Provide the current session's user data. For privacy policies, It's only available when extension request `user.*` permission. This permission can be define in `.metadata.resource.permissions` and is `granted` or `denied` by the user during installation of an extension.

### tenant
**type**: *object*\
**value**: Refer to [tenant data]()\
Provide the current tenant/institution data. For privacy policies, It's only available when extension request `tenant.*` permission. This permission can be define in `.metadata.resource.permissions` and is `granted` or `denied` only by admistrator user during installation of an extension.