---
layout: post
title:      "THE END! Haha but not really..."
date:       2019-11-17 22:36:03 -0500
permalink:  the_end_haha_but_not_really
---

So what they say is true. When it comes to coding and being a developer, you are never done learning. That is the quintissential characteristic of the role. Now that I've finished the final react/redux project, I am of course reflecting on how much I've learned since starting the program, and just how much I still have to learn, despite the fact that it's the end. All in all, I feel really happy about far I've come, and it's exciting to see the progress. This final project did a good job of having you flex all of the knowledge that you've gotten from the curriculum. 

For my project, I did a mock Airbnb app that just allowed a user some CRUD functionality for interacting with Stays. Since one of the most important parts of building this project was undertsanding the flow of react/redux in terms of understanding state and props and passing them from between components, I'll give a general walkthrough of just the highlights of rendering the index for my Stays component.

So starting in my App.js file, amongst other code, I needed the following imports:

```
import React, { Component } from 'react';
import './App.css';
import { BrowserRouter as Router, Route, Switch} from 'react-router-dom';
import StaysList from './containers/StaysList';
import { connect } from 'react-redux';
import { fetchStays } from './actions/stays';
```

I made sure to connect my App component to my store, as well as my imported action of fetchStays :

```
export default connect(
    null,
    { fetchStays, userLogIn }
)(App);
```

I called this action inside of my componentDidMount() which was invoked as soon as my App component was mounted. That is so that I am immediately making a request to fetch my stays data from my database and giving it to my store so that all of the components nested inside of my high-level App component have access to my stays:

```
componentDidMount() {
    this.props.fetchStays()
  }
```

Here is the action in which I am making my fetch request and dispatching the payload of my action, my stays data, to give to my staysReducer:

```
export const fetchStays = () => {
  return (dispatch) => {
    dispatch({ type: 'LOADING_STAYS' })

    fetch('http://localhost:3001/stays')
    .then(response => response.json())
    .then(stayData => {dispatch({ type: 'ADD_STAYS', stays: stayData })})
  }
}
```

In my staysReducer, I am taking that data, and in the case in which the action type (ADD_STAYS) matches, I am adding those fetched stays to my state and returning my state: 

```
const staysReducer = (state = { stays: [], loading: false }, action) => {
  switch (action.type) {
    case 'LOADING_STAYS':
      return {
        ...state,
        stays: [...state.stays],
        loading: true
      }
    case 'ADD_STAYS':
      return {
        ...state,
        stays: action.stays,
        loading: false
      }
			
			...
			
			default:
      return state;
  }
}

export default staysReducer;
```


Now, back within my App component's render, using Browserrouter I set up my route to "http://localhost:3000/stays", passing it my container component that holds the list of my stays. Note that my routes were wrapped with <Switch> so that only the first route that matched that location was shown:

```
.....
<Router>
            <Navbar />
              <Switch>
							.....
							<Route exact path="/stays" component= { StaysList } />
							....
							
							</Switch>
        </Router>
```

Within my StaysList container component, I have all my necessary imports:

```
import React, { Component } from 'react';
import { connect } from 'react-redux';
import StayCard from '../components/StayCard';
import { Grid } from "../components/CardStyle";
```

I am using connect to connect this component, as well as using mapStateToProps:

```
export default connect(mapStateToProps)(StaysList);
```

I'm doing so to grab my stays data from my store and mapping it to the props of my StaysList component:

```
const mapStateToProps = state => {
  return {
    stays: state.stays
  }
}

```

In this way, within my StaysList container, I can iterate through the array of stay objects I now have in my props from my store to create a StayCard component (my presentational component for my stays) for each, which just displays the data for each stay:

```
class StaysList extends Component {

  render() {
    console.log(this.props.stays)
    if (this.props.stays.length === 0) {
      return <p>Loading...</p>
    }

    return(
      <div>
        <h1>Stays</h1>
        <div>
          <Grid>
          {this.props.stays.map(stay =>
            <StayCard
              key={stay.id}
              id={stay.id}
              title={stay.title}
              cost={stay.cost}
              location={stay.location}
            />
          )}
          </Grid>
        </div>
      </div>
    )
  }
}
```

And that is the main overview for how I set up the index for my Stays. 




