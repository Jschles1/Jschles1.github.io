---
layout: post
title:  "React-Redux Final Project: Workout Planner"
date:   2017-09-25 22:51:59 +0000
---


For my React-Redux final project, I created a simple app that allows users to create workouts and add/remove exercises to each workout. When adding an exercise, you can specify its name, number of repetitions, number of sets, and the rest period between sets.

The first thing I decided to do when building this app was to build the Rails API backend. I found this easy to build since I wasn't dealing with an external API, and would just dealing with API calls to two resources: Workouts and Exercises. Workouts have many Exercises, and Exercises belong to a workout. To ensure the API functioned properly and rendered the right responses to different requests without having to build out the front end first, I used TDD with RSpec. The Rails API live lecture done by Luke was invaluable in guiding me through this process.

Once the Rails API backend was completed, the time came to create the React app and connect it to the Rails API backend. For this project, I chose to use the method provided in the project readme and keep both the frontend and the backend in the same repository. I executed the create-react-app command to create the frontend and place it in the client folder in the repo. Then, I enabled the Webpack development server to proxy requests to the API to negate any issues with CORS. Finally, I installed the Foreman gem to allow me to start my frontend and backend servers at the same time. I know i'll probably have to take the other approach if I choose to deploy this app to somewhere like Heroku, but i'll cross that bridge when I get to it.

The next step was to build out the React-Redux front end. I have really grown fond of React and how it allows for the continous alteration of content on the page without requiring a page refresh through the virtual DOM. This greatly improves the user experience overall. Creating new components and figuring out how to make them work together through props and state to get the outcome you want is what I enjoy the most about working with React. For CSS, I implemented the React Semantic UI library which has quickly become my new favorite to use.

The hardest part of building this app was dealing with Redux. At first I was confused about what the role of Redux would be in relation to the Rails API, since I thought it was sort of used as a backend itself in the lessons and labs. It turns out that I wasn't understanding it correctly. The Rails API is concerned with persisting data to the database. Redux is only concerned with maintaining the app's state. The state is what React checks in its Virtual DOM for changes so it knows when to re-render the page. The data stored in the Redux state is not persisted in any way. Once you refresh the page, the state is gone.

Once I clarified the roles that React, Redux and the Rails API would play, I had to get each of them to work together to get the app working. The way the data flows between the front end and backend in an app like this works as follows, using an example found in my app of loading a list of workouts:

1. The user clicks the "My Workouts" button on the navbar, which loads the WorkoutsPage component. This is a container component, meaning that this component is connected to the Redux store. This provides the component both the workout data in the store's state and action creators as props through the `mapStateToProps()` and `mapDispatchToProps()` functions respectively.

```
const mapStateToProps = (state) => {
  return { workouts: state.workouts };
}

const mapDispactchToProps = (dispatch) => {
  return { actions: bindActionCreators(actions, dispatch) }
}

export default connect(mapStateToProps, mapDispactchToProps)(WorkoutsPage);
```

2. Once the WorkoutsPage has been rendered, the `componentDidMount()` lifecycle method will be executed. This will in turn call the `fetchWorkouts()` action creator (which is passed to the WorkoutsPage as a prop through `mapDispatchToProps()`.

```
componentDidMount() {
  this.props.actions.fetchWorkouts();
}
```

3. In the `fetchWorkouts()` action creator, we use Redux Thunk middleware to return a function that receives the store's dispatch function. This allows us to dispatch multiple actions: first, we dispatch a `LOADING_WORKOUTS` action to the store which sets the state attribute of `loadingWorkouts` to true so our state knows that we're about to make a request to our API. Then, we make the fetch request to `/api/workouts` route. The API receives the request and tells the API::Workouts controller to execute the index action, which retrieves all the workout objects in the database and renders them as response. Once the response is received, the fetch promise is resolved and we then convert the response data to JSON. Then we dispatch another action with a `FETCH_WORKOUTS` type, passing in the response data as the payload.

```
export function fetchWorkouts() {
  return function(dispatch) {
    dispatch({type: 'LOADING_WORKOUTS'})
    return fetch(`/api/workouts`)
      .then(resp => resp.json())
      .then(workouts => dispatch({type: 'FETCH_WORKOUTS', payload: workouts}))
  }
}
```

We dispatch multiple actions here because API calls using `fetch()` are asynchronous, so we want to represent the state of the app in between the user asking for data, and the app receiving the data. It is important that the Redux state always reflects the current state of the app.

4. The Workouts reducer takes the `FETCH_WORKOUTS` action type from the dispatch and uses it to update the state, setting it equal to action's payload (which is the response data from the API).

```
export default function workoutsReducer(state = [], action) {
  switch(action.type) {
    case 'FETCH_WORKOUTS':
      return action.payload;
    case 'ADD_WORKOUT':
      return state.concat(action.payload);
    case 'DELETE_WORKOUT':
      return state.filter(workout => workout.id !== action.id);
    default:
      return state;
  }
}
```

5. The WorkoutsPage component passes the workouts contained in the updated state as props to the WorkoutsList functional stateless component, which renders the list of workouts onto the page.

```
<WorkoutsList workouts={this.props.workouts}/>
```

I made this app simple because I wanted to get the basics down when it comes to building a full-stack app, since using React, Redux, and a Rails API can get complicated very quickly. I plan on building new features for this app in the near future, including:

* Implementing user accounts and authentication
* Create a feature where users can get suggested workouts and exercises based on what their goal is. This will primarily be useful for those who don't know where to start when it comes to exercising and getting into shape. I'm deciding whether I should use an external API for this or if I should seed the database with premade workouts and exercises.

[Here](https://github.com/Jschles1/workout-planner) is the link to the GitHub repo of my project.

