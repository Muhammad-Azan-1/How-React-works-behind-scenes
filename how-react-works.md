# When we do npm run dev then what happens ?


# 1. npm Checks the package.json File :

  * When you run npm run dev, npm looks into the package.json file in your project and finds the "scripts" section.
  Specifically, it looks for the "dev" key. For example :

  ![Scripts](./scripts.png)

  * Here, the "dev": "vite" means that running npm run dev executes the vite command. This starts the Vite local development server.




# 2. Vite local Development Server Starts :

  * Vite (or another tool like Webpack or Next.js, depending on your setup) is a modern build tool that starts a local development server. 

  * This server typically runs on a default address like ```http://localhost:5173``` (Vite’s default port) or
  ```http://localhost:3000 ```(common for Next.js or other tools). At this point, Vite is running in the background, ready to handle requests from the browser.




# 3. Browser Requests the URL :

  * When you open your browser and navigate to  ```http://localhost:5173 ``` (or the port Vite is running on), the following happens:

  * Browser Sends a Request to the Local Dev Server through this url :

  * The browser sends an HTTP request to the local development server (e.g., ```http://localhost:5173/```).
    The server responds with the entry point of your application, which is typically an index.html file located in your project’s root or public folder

    ### This index.html files containes

  * The ```<div id="root"></div>``` is an empty container where your React application will be mounted.

  * The ```<script type="module" src="/src/main.jsx"></script>``` tells the browser to load the main.jsx file, which is the entry point for your JavaScript/React code.

  * The type="module" attribute indicates that the script is an ES Module, allowing modern JavaScript features like import and export.  



# 4. Browser Requests main.jsx :

  * When the browser encounters the ```<script type="module" src="/src/main.jsx">``` tag, it sends a request to the Vite local development server to fetch the main.jsx file. Here’s what happens next:

  ### Vite local Development server Processes main.jsx:
           
  * main.jsx contains JSX (React’s syntax) and ES Module import statements, which browsers can’t execute directly.

  * Vite transforms main.jsx into executable JavaScript:

  * Converts JSX (e.g., <App />) into React.createElement calls.
    Converts JSX to JavaScript (e.g., <App /> → React.createElement(App)).

  * Resolves import statements (e.g., for react-dom/client and App.jsx).

  * Vite sends this processed JavaScript to the browser as an ES Module.

  #### Example of main.jsx before processing:

  ![main.jsx before](./before.png)

  #### Example of main.jsx After Vite’s processing (simplified):

  ![main.jsx after](./after.png)

  ### Browser Loads main.jsx:

  * The browser receives the processed JavaScript code and loads it into memory. At this point, nothing is displayed on the screen because the code hasn’t been executed yet.


  ### Browser Executes main.jsx:

  * The browser runs the JavaScript code line-by-line. This is where createRoot, document.getElementById('root'), and root.render(<App />) come into play.


# 5. Understanding createRoot

  ## What is createRoot?

  * createRoot is a function from the react-dom/client library (introduced in React 18). It’s used to set up a React root, which is a connection point between your React application and the browser’s DOM.

  * Syntax: ```createRoot(container)``` takes a DOM element (e.g.,```<div id="root">Root Container</div>```) as an argument and returns a root object that React uses to manage rendering.

   * What Does It Do in createRoot(document.getElementById('root'))?

   * document.getElementById('root') is standard JavaScript that finds the ```<div id="root">``` element in the index.html file. For example:


  ``` <div id="root"></div> ```
  

   * createRoot(document.getElementById('root')) tells React: “Use this``` <div id="root">``` as the root container where I’ll render my React application.”

  * It returns a React root object, which has methods like render to control what gets displayed inside ```<div id="root">```.



# 6. What Happens in root.render(<App />)?

  * The render method (called on the root object) tells React to render the App component (and its children) into the 
  ```<div id="root">.```

  ### When root.render(<App />) is executed:

  * React calls the App component (from App.jsx), which returns JSX. For example:

  ![App.jsx before](./AppComponent.png)

  * React converts the JSX into a Virtual DOM, which is a JavaScript object representing the UI structure. For the above  App, the Virtual DOM looks like (simplified):

  ![App.jsx code converted into virtual Dom and passed in root container](./App-jsx-into-VirtualDom.png)

  * React uses the Virtual DOM to update the real DOM inside ```<div id="root">```. After rendering, the real DOM looks like:

  ![Real Dom](./AppComponent.png)


# NOTE

  The Real DOM lives inside the root container (```<div id="root">```), and React continuously updates it based on changes detected in the Virtual DOM.”