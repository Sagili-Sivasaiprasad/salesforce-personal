🌈 Lifecycle Hooks in Lightning Web Components (LWC)

Lifecycle hooks allow you to control what happens when a Lightning Web Component is created, rendered, updated, or destroyed.

Think of them as checkpoints in a component’s life.

🔁 LWC Lifecycle Flow (Easy to Remember)
Constructor → ConnectedCallback → Render → RenderedCallback → DisconnectedCallback
                                     ↓
                                 ErrorCallback

🧩 List of Lifecycle Hooks
    Hook	                Purpose
🟢 Constructor	            Component is created
🔵 ConnectedCallback	    Component added to DOM
🟡 Render	                Controls UI rendering
🟠 RenderedCallback	        UI fully rendered
🔴 DisconnectedCallback	    Component removed
⚠️ ErrorCallback	        Error handling
======================================================================================
🟢 1. constructor()

📌 When it runs:
When the component instance is created.

📘 Aura equivalent: init()
✅ Key Points
Runs first
Flow: Parent → Child
Child DOM elements ❌ NOT available
@api properties ❌ NOT available yet
Must call super() (mandatory)
⚠️ Why super() is required
LWC extends LightningElement.
Calling super() ensures proper initialization of this.

❌ Avoid
DOM access
Apex calls
Accessing @api values
======================================================================================
🔵 2. connectedCallback()

📌 When it runs:
When the component is inserted into the DOM.

✅ Key Points
Flow: Parent → Child
@api properties ✅ available
DOM elements ❌ still not available
Apex calls ✅ recommended
Parent DOM access ✅ allowed
Use this.isConnected to verify DOM connection

✅ Best Use Cases
Call Apex
Initialize data
Subscribe to events
======================================================================================
🟡 3. render()

📌 When it runs:
After connectedCallback(), before rendering.

✅ Key Points
Flow: Parent → Child
Used to override default template
Allows conditional rendering
⚠️ Not a true lifecycle hook
Protected method of LightningElement
⚠️ Advanced Usage Only
Most components do not need this hook.
======================================================================================
🟠 4. renderedCallback()

📌 When it runs:
After the component is fully rendered in the UI.

✅ Key Points
Flow: Child → Parent
DOM elements ✅ available
Called every time the component re-renders
Can cause infinite loop if not handled carefully

⚠️ Best Practices
Use a private flag:
    if (this.isRendered) return;
    this.isRendered = true;
❌ Do NOT update reactive properties here
❌ Do NOT make Apex calls

======================================================================================
🔴 5. disconnectedCallback()

📌 When it runs:
When the component is removed from the DOM.

✅ Key Points
Flow: Parent → Child
Used for cleanup

✅ Best Use Cases
Unsubscribe from events
Clear timers / intervals
Remove listeners
======================================================================================
⚠️ 6. errorCallback(error, stack)

📌 When it runs:
When an error occurs in:

Constructor
ConnectedCallback
Rendering
Event handlers

✅ Key Points
Similar to catch {} in JavaScript
error → JavaScript Error object
stack → stack trace string

✅ Best Use Cases
Log errors
Show fallback UI
Render error templates
======================================================================================
A lifecycle hook is a callback method that triggers at a specific phase of a component instance’s lifecycle.

What is a Host Element?

The host element is a Custom Element. It is attached to the browser’s DOM, so that our component can be rendered. We can think of this as an input or div type element, except that is a custom Web Component e.g., <my-component>. It also acts as the “container” for our component in the parent’s DOM.

We can access it via this, this.template and this.template.host in your JavaScript, as below –

1. this – Refers to the component instance (JavaScript class). Can be used to access reactive properties, methods, or @api public properties/methods defined in the component’s class.

Example:
        export default class MyComponent extends LightningElement {
        myProperty = 'value';
        
        handleClick() {
            console.log(this.myProperty); // Access component's property
        }
        }

2. this.template – Refers to the The component’s shadow root, representing the DOM inside the component’s template.. Used to query or manipulate elements within the component’s shadow DOM.

Example:
        <!-- Component Template -->
        <template>
        <div data-id="myDiv">Hello</div>
        </template>

        // Access the div element
        renderedCallback() {
        const div = this.template.querySelector('[data-id="myDiv"]');
        }

3. this.template.host – Refers to The host DOM element (the custom element itself, e.g., <my-component>). Used to interact with the host element’s attributes e.g., adding a class or retrieving an attribute).

Example:
        connectedCallback() {
        // Add a class to the host element
        this.template.host.classList.add('my-class');
        
        // Get an attribute from the host
        const id = this.template.host.getAttribute('data-id');
        }

Types of Life cycles Hooks –

Below are the 5 lifecycle hooks that we have in LWC in Salesforce –
constructor()
connectedCallback()
renderedCallback()
disconnectedCallback()
errorCallback(error, stack)

================================ Constructor==============================================
1. constructor():The constructor() is the first lifecycle hook to execute. It is called when the LWC component instance is created. This happens before it is added to the DOM.

Key Points:
    1.	Flows from parent to child i.e. if you have a parent and child component, the constructor() defined in the parent component will fire first.
    2.	Used for initializing private variables.
    3.	The first statement must be super(). This is because the Lightning Web Component inherits from LightningElement. It includes its own constructor that should not be skipped during initialization.
    4.	Child elements and Public properties (@api) can’t be accessed. This is because they don’t exist yet and are undefined.
    5.	You cannot access or interact with host element and also can’t add properties to host element. This is because the component is not fully loaded in the DOM.
    6.	Avoid return (other than an early return;) or using document.write()/open().

Constructor example Code –
The example LWC showcases the below functionality –
    1.	Initialization of Private Properties
        In the constructor, private properties are initialized with default values. For instance:
            count is initialized to 0.
            isLoading is initialized to false.
            These initializations ensure the component has well-defined states before interacting with other parts of the application.

    2.	Inaccessibility of Public Properties (@api) in Constructor
        Public properties, such as message, cannot be accessed in the constructor and return undefined. This limitation exists because public properties are only available after the component’s lifecycle progresses beyond the constructor.
        
    3.	Inaccessibility of Child Elements in Constructor
        Attempting to access child elements (e.g., <lightning-button>) using this.template.querySelector in the constructor results in null. This happens because the DOM is not yet rendered during the constructor phase.
        
    4.	Avoiding DOM Manipulation of Host Elements in Constructor
        Manipulating the DOM or adding attributes to the host element in the constructor is not recommended. For example:
            Adding a CSS class like child-host to the host element in the constructor is discouraged. This is because the host element does not exist during this phase.


================================connectedCallback===========================================
2. connectedCallback()

The connectedCallback() lifecycle hook fires when a component is inserted into the DOM.

Key Points:
    1.	Flows from parent to child i.e. if you have a parent and child component, the connectedCallback() defined in the parent component will fire first.
    2.	To check if a component is connected to the DOM use this.isConnected.
    3.	Can be used for –
        a.	Calling Apex Method.
        b.	Initializing event listeners
        c.	Calling External API using Fetch.
        d.	Calling Navigation service.
        e.	Subscribing and Unsubscribing from a Message Channel.
    4.	Public properties (@api) can be accessed.
    5.	You can access or interact with host element and also add properties to host element, using this, this.template and this.template.host.
    6.	Child elements can’t be accessed. This is because they don’t exist yet and are undefined.
    7.	Can run multiple times, such as when an element is reordered. Add logic to ensure code runs only once if needed.

================================renderedCallback===========================================
3. renderedCallback()
    The renderedCallback() lifecycle hook fires after the component renders or re-renders into the DOM.

Key Points:
    1.	Flows from child to parent component i.e. if you have a parent and child component, the renderedCallback() defined in the child component will fire first.
    2.	renderedCallback() runs multiple times, as a component is usually rendered many times during the lifespan of an application. To avoid repeated execution, use a boolean flag (e.g., isRendered) to track if renderedCallback() has already executed.
    3.	Can be used for below, but make sure to use guard logic using boolean flag to guarantee a one-time run of renderedCallback()-
        a.	Loading custom CSS style from static resources.
        b.	Loading external scripts from static resources.
        c.	Calling Apex Method
        d.	Calling External API using Fetch.
        e.	Calling Navigation service.
        f.	Adding event listeners.
    4.	Perform logic after a component has finished rendering. This is useful for tasks related to UI interaction. Examples include computing node sizing or adding event listeners.
    5.	Avoid updating component state (e.g., @track or public properties) within renderedCallback(), as it can lead to infinite rendering loops.Instead, use getters and setters for reactive changes.
    6.	Child elements can be accessed using this.template.querySelector() and this.template.querySelectorAll().
    7.	Event listeners added in renderedCallback won’t duplicate, as browsers ignore identical listeners. However, it’s best practice to remove them in disconnectedCallback to avoid potential memory leaks.

================================disconnectedCallback========================================
4. disconnectedCallback()
    The disconnectedCallback() lifecycle hook fires when the component is removed from the DOM.

Key Points:
    1.	Flows from child to parent component i.e. if parent and child component are removed, the disconnectedCallback() defined in the child component will fire first.
    2.	Can be used to clean up work done in the connectedCallback() like –
        a.	Cleaning up event listeners to prevent memory leaks.
        b.	Unsubscribing from message channel.
        c.	Removing or resetting timers such as setTimeout or setInterval.
        d.	Disconnecting observers like MutationObserver or IntersectionObserver.
        e.	Releasing references to DOM elements or data bindings.
        f.	Purging caches
    3.	Important to release resources: Ensures efficient memory usage and prevents unintended behavior in the application
    4.	Runs once for each component removal: Executes each time the component is removed from the DOM. Add logic to avoid redundant cleanup if necessary.
================================errorCallback========================================
5. errorCallback(error, stack)
    The errorCallback() lifecycle hook acts as an error boundary for uncaught exceptions in descendant components or template event handlers.It is invoked when the component throws error in one of the lifecycle hooks (instantiating the component, connecting or rendering).

•	Key Points:
    a.	Similar to JavaScript catch{} block, has two parameters.
        i.	error is the thrown error object.
        ii.	stack is a string indicating the component path (e.g., <c-parent> <c-child>).
    b.	Triggered if a descendant component (child or nested child) in the same DOM tree throws an error during the rendering or lifecycle phases
    c.	To capture the stack information and render a different template when error is occurred.
    d.	Can be used for:
        i.	Logging errors for debugging or monitoring purposes.
        ii.	Displaying user-friendly error messages.
        iii.	Preventing the error from propagating further in the component hierarchy.
        iv.	Sending error details to external logging services.
    e.	Handles errors only for child components within the current component’s DOM subtree.
    f.	Does not catch all errors like Errors thrown asynchronously (e.g., in a setTimeout) or errors outside the component tree, like global event handlers

LWC Lifecycle Flow –

Lightning web components have a lifecycle managed by the framework. The framework creates components, inserts them into the DOM, renders them, and removes them from the DOM. It also monitors components for property changes.