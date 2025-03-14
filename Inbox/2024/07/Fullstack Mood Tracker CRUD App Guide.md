---
created: 2024-07-05T14:59:55 (UTC -03:00)
tags: []
source: https://jme2791dev.hashnode.dev/step-by-step-guide-to-creating-a-fullstack-mood-tracker-crud-app-with-react-nodejs-and-sqlite
author: Jesus Esquer
---

# Fullstack Mood Tracker CRUD App Guide

> ## Excerpt
> Step-by-step guide to building a fullstack CRUD Mood Tracker app using React, Node.js, and SQLite

---
##### Introduction

Hi all, welcome, and thanks for stopping by!

In this step-by-step guide, we will learn how to create a full-stack application with create, read, update, and delete (CRUD) functionality.

We will use Node.js and SQLite for the backend, and React for the frontend to create our user interface (UI).

Our project will be a mood tracker app where users can input their mood on a scale of 1-10. The app will display these moods in a list, with options to edit or delete them. Additionally, we will include a linear chart to track and visualize mood trends.

The final version will look like this:

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/184ny9dmxnqr3sbzipwb.png)

Note: This tutorial assumes you have Node.js installed and an IDE installed preferably VScode. If not please go here to install Node.js and here for VScode.

Ok without further ado, let’s jump to it!

##### Backend Development

We will start with the backend. First, create a backend directory and open it in your favorite IDE. For this article, we will use VS Code.

After opening the backend directory in your code editor, open the terminal and run the following command:

This will register our project as a Node application and create a package.json file inside our directory.

Next, we need to add our project dependencies.

We will use Express to create our server, SQLite3 for our database, and Body-Parser to extract the body of requests.

In the terminal, run:

```
npm install express sqlite3 body-parser
```

After the installation is complete, we will set up our directory structure.

The folder structure inside the backend directory should look like this (create directories and files as needed):

```
  /node_modules
  /db
    mood.db
  /routes
    index.js
    mood.js
  /models
    mood.js
  server.js
  package.json
```

Open server.js, the main file of the Node.js application, and copy and paste the following code:

```
<span>const</span> express = <span>require</span>(<span>'express'</span>);
<span>const</span> sqlite3 = <span>require</span>(<span>'sqlite3'</span>).verbose();
<span>const</span> bodyParser = <span>require</span>(<span>'body-parser'</span>);
<span>const</span> app = express();

app.use(bodyParser.json());

<span>let</span> db = <span>new</span> sqlite3.Database(<span>'./db.sqlite'</span>);

app.get(<span>'/'</span>, <span>(<span>req, res</span>) =&gt;</span> {
  res.send(<span>'Hello World!'</span>);
});

app.listen(<span>3000</span>, <span>() =&gt;</span> {
  <span>console</span>.log(<span>'Server is running on port 3000'</span>);
});
```

-   We imported this code's express, sqlite3, and body-parser modules and initialized a new Express application.
    
-   We used body-parser as middleware to parse incoming request bodies. We also initialized a new SQLite database (or opened it if it already exists).
    
-   Defined a route handler for GET requests to the root URL (/), and started the server on port 3000.
    

To run the app with hot reload (so you don't have to restart the server each time you make a change). We can use a dependency called nodemon.

Install it by running:

```
npm install --save-dev nodemon
```

To use nodemon, update the scripts section inside package.json. Change the start script to look like this:

```
<span>"start"</span>: <span>"nodemon server.js"</span>
```

Open your terminal and run:

Then, open your default browser and navigate to [localhost:3000](http://localhost:3000/). You should see "Hello World".

Your terminal should display the following message:

```
[nodemon] starting <span>`node server.js`</span>
Server started on port <span>3000</span>
Connected to the mood database.
```

Congrats, you have created and run an Express server!

If you don’t see it running, check your terminal for any errors.

Now let's start working on the model and routes for our application.

A model defines the structure of an object, and routes perform the CRUD operations.

Open models/mood.js and add the following code:

```
<span>const</span> sqlite3 = <span>require</span>(<span>'sqlite3'</span>).verbose();

<span>let</span> db = <span>new</span> sqlite3.Database(<span>'./db/moods.db'</span>, <span>(<span>err</span>) =&gt;</span> {
    <span>if</span> (err) {
        <span>console</span>.error(err.message);
    }
    <span>console</span>.log(<span>'Connected to the moods database.'</span>);
});

db.run(<span>`CREATE TABLE IF NOT EXISTS moods(
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    mood INTEGER NOT NULL
)`</span>, <span>(<span>err</span>) =&gt;</span> {
    <span>if</span> (err) {
        <span>console</span>.error(err.message);
    }
    <span>console</span>.log(<span>"Moods table created"</span>);
});

<span>module</span>.exports = db;
```

In this code, we imported SQLite and defined our database. Created our table with id and mood columns, and exported our database.

Next, let's create our routes. Copy and paste the following code:

```
<span>const</span> express = <span>require</span>(<span>"express"</span>);
<span>const</span> router = express.Router();
<span>const</span> db = <span>require</span>(<span>"../models/mood"</span>);

router.post(<span>"/"</span>, <span>(<span>req, res</span>) =&gt;</span> {
  <span>const</span> { mood } = req.body;
  <span>const</span> query = <span>`INSERT INTO moods(mood) VALUES(?)`</span>;

  db.run(query, [mood], <span><span>function</span> (<span>err</span>) </span>{
    <span>if</span> (err) {
      <span>console</span>.error(err.message);
      <span>return</span> res.status(<span>500</span>).json({ <span>message</span>: err.message });
    }
    res.send({ <span>id</span>: <span>this</span>.lastID, <span>mood</span>: mood });
  });
});

router.get(<span>"/"</span>, <span>(<span>req, res</span>) =&gt;</span> {
  <span>const</span> query = <span>`SELECT * FROM moods`</span>;

  db.all(query, [], <span>(<span>err, rows</span>) =&gt;</span> {
    <span>if</span> (err) {
      <span>console</span>.error(err.message);
      <span>return</span> res.status(<span>500</span>).json({ <span>message</span>: err.message });
    }
    res.send(rows);
  });
});

router.put(<span>"/:id"</span>, <span>(<span>req, res</span>) =&gt;</span> {
  <span>const</span> { mood } = req.body;
  <span>const</span> { id } = req.params;
  <span>const</span> query = <span>`UPDATE moods SET mood = ? WHERE id = ?`</span>;

  db.run(query, [mood, id], <span><span>function</span> (<span>err</span>) </span>{
    <span>if</span> (err) {
      <span>console</span>.error(err.message);
      <span>return</span> res.status(<span>500</span>).json({ <span>message</span>: err.message });
    }
    res.send({ <span>id</span>: id, <span>mood</span>: mood });
  });
});

router.delete(<span>"/:id"</span>, <span>(<span>req, res</span>) =&gt;</span> {
  <span>const</span> { id } = req.params;
  <span>const</span> query = <span>`DELETE FROM moods WHERE id = ?`</span>;

  db.run(query, [id], <span><span>function</span> (<span>err</span>) </span>{
    <span>if</span> (err) {
      <span>console</span>.error(err.message);
      <span>return</span> res.status(<span>500</span>).json({ <span>message</span>: err.message });
    }
    res.send({ <span>changes</span>: <span>this</span>.changes });
  });
});

<span>module</span>.exports = router;
```

Here, we imported Express to create a server application.

Using Express, we made a router object to define routes for the mood application.

We also imported our recently created database model.

For our CRUD operations, we created a new route for each action:

-   **Create**: This route responds to all POST requests at the root path (/). We extract the mood from the request and then define a SQL query to insert a new mood. Run the query on our db module, passing the mood value as a parameter. We handle errors and send the response back to the client or a status code of 500 with the error message.
    
-   **Read**: This route responds only to GET requests at the root path (/). We define a SQL query to select all moods, run the query, and send back the response with moods if successful. Otherwise, we send a status 500 code with the error message.
    
-   **Update**: This route responds to PUT requests at the path /:id, mood id should be in the path like /6. Like POST, we extract the body from the request and get the id from the path. We create a query to update a specific mood based on the id. Then run the query, passing mood and id as parameters, and include error handling logic.
    
-   **Delete**: This route responds only to DELETE requests at the path /:id. We get the id from the request, then create a query to delete it from the moods table if it exists, and apply the same error handling logic.
    

We exported our router object to use the routes in our server.js file. Let's update our server.js file to include our router.

Also, remove the database file reference we moved inside our model. Updated server.js:

```
<span>const</span> express = <span>require</span>(<span>"express"</span>);
<span>const</span> bodyParser = <span>require</span>(<span>"body-parser"</span>);
<span>const</span> moodsRouter = <span>require</span>(<span>"./routes/mood"</span>);
<span>const</span> cors = <span>require</span>(<span>"cors"</span>);

<span>const</span> app = express();

app.use(cors());
app.use(bodyParser.json());
app.use(<span>"/moods"</span>, moodsRouter);

app.get(<span>"/"</span>, <span>(<span>req, res</span>) =&gt;</span> {
  res.send(<span>"Hello World!"</span>);
});

app.listen(<span>3001</span>, <span>() =&gt;</span> {
  <span>console</span>.log(<span>"Server started on port 3001"</span>);
});
```

Notice we also added the CORS middleware for local development and switched the port to 3001.

This will be helpful when integrating and testing our UI. In your terminal, run the following command to install CORS:

```
npm install --save-dev cors
```

Find out more about CORS [here](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).

Now start your server app, and let’s test the routes. We can use curl inside the terminal to test.

Here's how you can do it for each type of request:

1.  **GET request**: To fetch all moods, use the following command:

```
curl http:<span>//localhost:3001/moods</span>
```

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/tp7n1d26btqlf1ouctzw.png)

2.  **POST request**: To create a new mood, use the -d flag to send data:

```
$body = @{
    mood = <span>7</span>
} | ConvertTo-Json

$response = Invoke-WebRequest -Uri http:<span>//localhost:3001/moods -Method POST -Body $body -ContentType "application/json"</span>
```

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/3s9mrpij2ig0yygyophp.png)

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/eenju54m2gipd1wp0xwc.png)

3.  **PUT request**: To update a mood, you need to know the id of the mood you want to update. Replace :id with the actual id of the mood:

```
# Replace :id <span>with</span> the actual id <span>of</span> the mood you want to update
$id = <span>":id"</span>

$body = @{
    mood = <span>8</span>
} | ConvertTo-Json

$response = Invoke-WebRequest -Uri http:<span>//localhost:3001/moods/$id -Method PUT -Body $body -ContentType "application/json"</span>
```

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/pbjrf7sfz8sv7qgnybor.png)

4.  **DELETE request**: To delete a mood, you also need to know the id of the mood. Replace :id with the actual id of the mood:

```
# Replace :id <span>with</span> the actual id <span>of</span> the mood you want to <span>delete</span>
$id = <span>":id"</span>

$response = Invoke-WebRequest -Uri http:<span>//localhost:3001/moods/$id -Method DELETE</span>
```

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/e285jr3y7ujgqkljv860.png)

Check your console for any errors.

You made it all the way here, congratulations! You built an application with CRUD functionality!

Now we are ready to move to the UI.

##### Frontend Development

For UI development, we first need to create a React app.

We'll use a tool called Create React App, which provides us with a template application to build on.

Open a new terminal, change the directory to frontEnd, and then type the following command:

```
npx create-react-app mood-tracker-ui
```

npx allows us to run npm packages without installing them.

It runs create-react-app, the third argument is the directory's name and application.

Now that we've created our app, change the directory to mood-tracker-ui:

Let's clean up by removing files and code we won't use.

Remove the following files: App.test.js, logo.svg, reportWebVitals.js, and setupTests.js.

We'll clean up index.js to look like this:

```
<span>import</span> React <span>from</span> <span>'react'</span>;
<span>import</span> ReactDOM <span>from</span> <span>'react-dom/client'</span>;
<span>import</span> <span>'./index.css'</span>;
<span>import</span> App <span>from</span> <span>'./App'</span>;

<span>const</span> root = ReactDOM.createRoot(<span>document</span>.getElementById(<span>'root'</span>));
root.render(
  <span><span>&lt;<span>React.StrictMode</span>&gt;</span>
    <span>&lt;<span>App</span> /&gt;</span>
  <span>&lt;/<span>React.StrictMode</span>&gt;</span></span>
);
```

Next, App.js should look like this: this:

```
<span><span>function</span> <span>App</span>(<span></span>) </span>{
  <span>return</span> (
    <span><span>&lt;<span>div</span> <span>className</span>=<span>"App"</span>&gt;</span>
      <span>&lt;<span>h1</span>&gt;</span>Hello World<span>&lt;/<span>h1</span>&gt;</span>
    <span>&lt;/<span>div</span>&gt;</span></span>
  );
}

<span>export</span> <span>default</span> App;
```

Now let's set up our folder structure and run our application. The folder structure should look like this (create directories and files as needed):

```
/frontend
    /src
        /components
            /MoodInput
            /MoodList
            /MoodChart
        /services
            /apiService.js
        /assets
            /styles
                /main.css
        App.js
        index.js
    /public
        index.html
    package.json
```

Inside your terminal, run the following command:

A new tab will open in your default browser and navigate to [localhost:3000](http://localhost:3000/). You should see something like this:

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/vezkgaihi1ygwbuddp27.png)

Great! Let's start with our API service to add our CRUD functionality. We need methods for GET, POST, PUT, and DELETE requests.

Open apiService.js and insert the following code:

```
<span>const</span> API_URL = <span>"http://localhost:3001"</span>;

<span>export</span> <span>const</span> getMoodsAPI = <span>async</span> () =&gt; {
  <span>const</span> response = <span>await</span> fetch(<span>`<span>${API_URL}</span>/moods`</span>);
  <span>const</span> data = <span>await</span> response.json();
  <span>return</span> data;
};

<span>export</span> <span>const</span> addMoodAPI = <span>async</span> (mood) =&gt; {
  <span>const</span> response = <span>await</span> fetch(<span>`<span>${API_URL}</span>/moods`</span>, {
    <span>method</span>: <span>"POST"</span>,
    <span>headers</span>: {
      <span>"Content-Type"</span>: <span>"application/json"</span>,
    },
    <span>body</span>: <span>JSON</span>.stringify({ mood }),
  });
  <span>const</span> data = <span>await</span> response.json();
  <span>return</span> data;
};

<span>// Update mood</span>
<span>export</span> <span>const</span> updateMoodAPI = <span>async</span> (id, updatedMood) =&gt; {
  <span>const</span> response = <span>await</span> fetch(<span>`<span>${API_URL}</span>/moods/<span>${id}</span>`</span>, {
    <span>method</span>: <span>"PUT"</span>,
    <span>headers</span>: {
      <span>"Content-Type"</span>: <span>"application/json"</span>,
    },
    <span>body</span>: <span>JSON</span>.stringify(updatedMood),
  });

  <span>if</span> (!response.ok) {
    <span>throw</span> <span>new</span> <span>Error</span>(<span>"Failed to update mood"</span>);
  }

  <span>return</span> response.json();
};

<span>// Delete mood</span>
<span>export</span> <span>const</span> deleteMoodAPI = <span>async</span> (id) =&gt; {
  <span>const</span> response = <span>await</span> fetch(<span>`<span>${API_URL}</span>/moods/<span>${id}</span>`</span>, {
    <span>method</span>: <span>"DELETE"</span>,
  });

  <span>if</span> (!response.ok) {
    <span>throw</span> <span>new</span> <span>Error</span>(<span>"Failed to delete mood"</span>);
  }

  <span>return</span> response.json();
};
```

Here's what each function does:

-   **getMoodsAPI**: Fetches all moods from the API.
-   **addMoodAPI**: Sends a POST request to add a new mood.
-   **updateMoodAPI**: Sends a PUT request to update a specific mood by ID.
-   **deleteMoodAPI**: Sends a DELETE request to remove a mood by ID.

Now that our API service is ready, let's build our components.

Start with MoodInput.js. Open the file and paste this code:

```
<span>import</span> { useState } <span>from</span> <span>"react"</span>;

<span>const</span> MoodInput = <span>(<span>{ addMood }</span>) =&gt;</span> {
  <span>const</span> [mood, setMood] = useState(<span>""</span>);
  <span>const</span> [error, setError] = useState(<span>null</span>);

  <span>const</span> handleSubmit = <span>(<span>e</span>) =&gt;</span> {
    e.preventDefault();
    <span>if</span> (mood === <span>""</span> || mood &gt; <span>10</span>) {
      setError(<span>"Please enter a value between 1 and 10"</span>);
    } <span>else</span> {
      setError(<span>null</span>);
      addMood(mood);
      setMood(<span>""</span>);
    }
  };

  <span>return</span> (
    <span><span>&lt;<span>form</span> <span>onSubmit</span>=<span>{handleSubmit}</span> <span>className</span>=<span>"mood-form"</span>&gt;</span>
      <span>&lt;<span>label</span> <span>htmlFor</span>=<span>"mood-input"</span>&gt;</span>
        On a scale of 1-10, how are you feeling today?
      <span>&lt;/<span>label</span>&gt;</span>
      <span>&lt;<span>input</span>
        <span>id</span>=<span>"mood-input"</span>
        <span>type</span>=<span>"number"</span>
        <span>min</span>=<span>"1"</span>
        <span>max</span>=<span>"10"</span>
        <span>value</span>=<span>{mood}</span>
        <span>onChange</span>=<span>{(e)</span> =&gt;</span> setMood(e.target.value)}
      /&gt;
      <span>&lt;<span>button</span> <span>type</span>=<span>"submit"</span>&gt;</span>Submit<span>&lt;/<span>button</span>&gt;</span>
      {error &amp;&amp; <span>&lt;<span>p</span>&gt;</span>{error}<span>&lt;/<span>p</span>&gt;</span>}
    <span>&lt;/<span>form</span>&gt;</span></span>
  );
};

<span>export</span> <span>default</span> MoodInput;
```

In this component:

-   We use the useState hook to manage the mood and error states. The handleSubmit function handles form submission. Validates the mood input, and adds the mood if valid.
    
-   The form includes an input for the mood and a submit button. If the input has an error we render an error message. Finally, we export the MoodInput component.
    

Next, let's create the MoodList component. This component will display all moods. Paste the following code inside the MoodList.js file:

```
<span>import</span> { useState } <span>from</span> <span>"react"</span>;
<span>import</span> {
  updateMoodAPI,
  deleteMoodAPI,
} <span>from</span> <span>"../../services/apiService/moodService"</span>;

<span>const</span> MoodList = <span>(<span>{ moods, deleteMood, updateMood }</span>) =&gt;</span> {
  <span>const</span> [editingMoodId, setEditingMoodId] = useState(<span>null</span>);
  <span>const</span> [editingMoodValue, setEditingMoodValue] = useState(<span>""</span>);

  <span>const</span> handleUpdateMood = <span>async</span> (id) =&gt; {
    <span>try</span> {
      <span>const</span> updated = <span>await</span> updateMoodAPI(id, { <span>mood</span>: editingMoodValue });
      updateMood(id, updated);
      setEditingMoodId(<span>null</span>);
      setEditingMoodValue(<span>""</span>);
    } <span>catch</span> (error) {
      <span>console</span>.error(error);
    }
  };

  <span>const</span> handleDeleteMood = <span>async</span> (id) =&gt; {
    <span>try</span> {
      <span>await</span> deleteMoodAPI(id);
      deleteMood(id);
    } <span>catch</span> (error) {
      <span>console</span>.error(error);
    }
  };

  <span>return</span> (
    <span><span>&lt;<span>div</span> <span>className</span>=<span>"mood-list"</span>&gt;</span>
      <span>&lt;<span>ul</span>&gt;</span>
        {moods?.map((mood, index) =&gt; (
          <span>&lt;<span>li</span> <span>key</span>=<span>{index}</span>&gt;</span>
            {index}-
            {editingMoodId === mood.id ? (
              <span>&lt;&gt;</span>
                <span>&lt;<span>input</span>
                  <span>type</span>=<span>"number"</span>
                  <span>min</span>=<span>"1"</span>
                  <span>max</span>=<span>"10"</span>
                  <span>value</span>=<span>{editingMoodValue}</span>
                  <span>onChange</span>=<span>{(e)</span> =&gt;</span> setEditingMoodValue(e.target.value)}
                /&gt;
                <span>&lt;<span>button</span> <span>onClick</span>=<span>{()</span> =&gt;</span> handleUpdateMood(mood.id)}&gt;Save<span>&lt;/<span>button</span>&gt;</span>
              <span>&lt;/&gt;</span>
            ) : (
              <span>&lt;&gt;</span>
                {mood.mood}
                <span>&lt;<span>button</span>
                  <span>onClick</span>=<span>{()</span> =&gt;</span> {
                    setEditingMoodId(mood.id);
                    setEditingMoodValue(mood.mood);
                  &#124;&#124;
                &gt;
                  Edit
                <span>&lt;/<span>button</span>&gt;</span>
              <span>&lt;/&gt;</span>
            )}
            <span>&lt;<span>button</span> <span>onClick</span>=<span>{()</span> =&gt;</span> handleDeleteMood(mood.id)}&gt;Delete<span>&lt;/<span>button</span>&gt;</span>
          <span>&lt;/<span>li</span>&gt;</span></span>
        ))}
      &lt;/ul&gt;
    &lt;/div&gt;
  );
};

<span>export</span> <span>default</span> MoodList;
```

In this component:

-   We import the necessary hooks and API services.
-   We define the MoodList functional component, initializing the state for the ID and value of the edited mood.
-   The handleUpdateMood and handleDeleteMood methods use the API service methods for updating and deleting moods. We use try-catch blocks for error handling.
-   When rendering, we map over the moods and create a list item for each one. If the current mood is being edited, an input and save button are displayed. Otherwise, we display the current mood with an edit button. The delete button is always available. \*Finally, we export the MoodList component.

We'll add a chart component for a visual representation of the moods.

This component uses the open-source libraries react-chartjs-2 and chart.js. Run the following command to install them:

```
npm install --save react-chartjs<span>-2</span> chart.js
```

After installing the dependencies, paste the following code inside the MoodChart component:

```
<span>import</span> React <span>from</span> <span>"react"</span>;
<span>import</span> {
  Chart <span>as</span> ChartJS,
  CategoryScale,
  LinearScale,
  PointElement,
  LineElement,
  Title,
  Tooltip,
  Legend,
} <span>from</span> <span>"chart.js"</span>;
<span>import</span> { Line } <span>from</span> <span>"react-chartjs-2"</span>;

ChartJS.register(
  CategoryScale,
  LinearScale,
  PointElement,
  LineElement,
  Title,
  Tooltip,
  Legend
);

<span>const</span> MoodChart = <span>(<span>{ moods }</span>) =&gt;</span> {
  <span>const</span> moodValues = moods?.map(<span>(<span>mood</span>) =&gt;</span> <span>Number</span>(mood.mood));
  <span>const</span> data = {
    <span>labels</span>: moods?.map(<span>(<span>mood, index</span>) =&gt;</span> <span>`Day <span>${index + <span>1</span>}</span>`</span>),
    <span>datasets</span>: [
      {
        <span>label</span>: <span>"Mood"</span>,
        <span>data</span>: moodValues,
        <span>borderColor</span>: <span>"rgb(75, 192, 192)"</span>,
        <span>backgroundColor</span>: <span>"rgba(75, 192, 192, 0.5)"</span>,
      },
    ],
  };
  <span>const</span> options = {
    <span>responsive</span>: <span>true</span>,
    <span>plugins</span>: {
      <span>legend</span>: {
        <span>position</span>: <span>"top"</span>,
      },
      <span>title</span>: {
        <span>display</span>: <span>true</span>,
        <span>text</span>: <span>"Mood Chart"</span>,
      },
    },
    <span>scales</span>: {
      <span>x</span>: {
        <span>type</span>: <span>"category"</span>,
      },
      <span>y</span>: {
        <span>type</span>: <span>"linear"</span>,
        <span>beginAtZero</span>: <span>true</span>,
      },
    },
  };

  <span>return</span> <span><span>&lt;<span>Line</span> <span>options</span>=<span>{options}</span> <span>data</span>=<span>{data}</span> /&gt;</span></span>;
};

<span>export</span> <span>default</span> MoodChart;
```

In this component:

-   We import the necessary libraries and components.
-   Then register the required Chart.js components.
-   Inside our MoodChart component. we map through the moods array to extract mood values and labels.
-   We define the data and options for the chart.
-   The Line chart component is rendered with the required properties.
-   Finally, we export the MoodChart component.

Now that we have our components, it's time to import them into App.js. Add the following code:

```
<span>import</span> { useState, useEffect } <span>from</span> <span>"react"</span>;
<span>import</span> MoodChart <span>from</span> <span>"./components/MoodChart/MoodChart"</span>;
<span>import</span> MoodInput <span>from</span> <span>"./components/MoodInput/MoodInput"</span>;
<span>import</span> MoodList <span>from</span> <span>"./components/MoodList/MoodList"</span>;
<span>import</span> { getMoodsAPI, addMoodAPI } <span>from</span> <span>"./services/apiService/moodService"</span>;
<span>import</span> <span>"./App.css"</span>;

<span><span>function</span> <span>App</span>(<span></span>) </span>{
  <span>const</span> [moods, setMoods] = useState([]);

  <span>const</span> updateMood = <span>(<span>id, updatedMood</span>) =&gt;</span> {
    setMoods(<span>(<span>prevMoods</span>) =&gt;</span>
      prevMoods.map(<span>(<span>mood</span>) =&gt;</span> (mood.id === id ? updatedMood : mood))
    );
  };

  <span>const</span> deleteMood = <span>(<span>id</span>) =&gt;</span> {
    setMoods(<span>(<span>prevMoods</span>) =&gt;</span> prevMoods.filter(<span>(<span>mood</span>) =&gt;</span> mood.id !== id));
  };

  useEffect(<span>() =&gt;</span> {
    <span>const</span> fetchMoods = <span>async</span> () =&gt; {
      <span>const</span> moods = <span>await</span> getMoodsAPI();
      setMoods(moods);
    };

    fetchMoods();
  }, []);

  <span>const</span> addMood = <span>async</span> (mood) =&gt; {
    <span>const</span> newMood = <span>await</span> addMoodAPI(mood);
    setMoods([...moods, newMood]);
  };

  <span>return</span> (
    <span><span>&lt;<span>div</span> <span>className</span>=<span>"app"</span>&gt;</span>
      <span>&lt;<span>div</span> <span>className</span>=<span>"mood-header"</span>&gt;</span>
        <span>&lt;<span>h1</span>&gt;</span>Mood Tracker<span>&lt;/<span>h1</span>&gt;</span>
        <span>&lt;<span>MoodInput</span> <span>addMood</span>=<span>{addMood}</span> /&gt;</span>
      <span>&lt;/<span>div</span>&gt;</span>
      <span>&lt;<span>div</span> <span>className</span>=<span>"mood-container"</span>&gt;</span>
        <span>&lt;<span>MoodList</span>
          <span>moods</span>=<span>{moods}</span>
          <span>deleteMood</span>=<span>{deleteMood}</span>
          <span>updateMood</span>=<span>{updateMood}</span>
        /&gt;</span>
        <span>&lt;<span>MoodChart</span> <span>moods</span>=<span>{moods}</span> /&gt;</span>
      <span>&lt;/<span>div</span>&gt;</span>
    <span>&lt;/<span>div</span>&gt;</span></span>
  );
}

<span>export</span> <span>default</span> App;
```

In this file:

-   We import the necessary libraries and newly created components.
-   Inside our App component, we initialize the state for moods. Define methods for handling mood creation, updates, and deletions.
-   The useEffect hook fetches all moods from our database when the component renders.
-   We render our components and pass them the necessary props.
-   Finally, we export the App component.

Run the app and you should see the full functionality now, make sure your server app is running too, and check your console for any errors.

Amazing! You’ve reached this point, we’ve successfully created a full-stack application. Now, we need to add some styling. In the next section, we’ll cover minor styling tweaks, but feel free to get creative and add your flair.

##### Styling

Open App.css and paste the following code:

```
body {
    <span>background</span>: mintcream;
}

.mood-header {
    <span>display</span>: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    margin-bottom: <span>20</span>px;
}

.mood-container {
    <span>display</span>: grid;
    grid-template-columns: <span>1</span>fr <span>1</span>fr <span>1</span>fr <span>0.5</span>fr <span>0.5</span>fr;
    gap: <span>10</span>px;
}

.mood-form &gt; * {
    margin-left: <span>10</span>px;
}

.mood-list {
    grid-column: <span>2</span>;
}

.mood-list li {
    margin-bottom: <span>10</span>px;
}

.mood-list li button {
    margin-left: <span>10</span>px;
}

.mood-chart {
    grid-column: <span>3</span> / span <span>2</span>;
}
```

Here’s what we’re doing:

-   **Body**: Setting the background color to mintcream.
-   **Header**: Using Flexbox to center items vertically and horizontally, with some space below.
-   **Mood Container**: Using a grid layout to position MoodList and MoodChart. MoodList takes one column, and MoodChart spans two columns, both centered.
-   **Mood List**: Adding space between list items and buttons.

Import App.css in your App.js file, then reload your application to see the changes.

##### Conclusion

Throughout this journey, we built a full-stack application with a React UI, an Express.js server, and SQLite for data storage. We also added some styling using Flexbox and Grid layouts.

##### Next Steps

Here are some ideas for further improvements:

-   **Testing**: Add tests using Jest and React Testing Library.
-   **Functionality**: Enhance features, like adding real dates to moods.
-   **Styling**: Add more styles, such as icons instead of buttons, custom fonts, and shadows.
-   **Deployment**: Deploy the application.

Thanks for sticking with it! You’ve learned a thing or two. Please share your feedback and like the article.

[Code Repo](https://github.com/jm27/mood-tracker)
