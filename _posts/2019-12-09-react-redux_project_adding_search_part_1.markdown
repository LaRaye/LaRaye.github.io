---
layout: post
title:      "React-Redux Project: Adding Search Part 1"
date:       2019-12-09 05:42:33 +0000
permalink:  react-redux_project_adding_search_part_1
---


For my react-redux final project I built a mock up of an Airbnb app. One of the features I wanted to include in order to more closely model the user experience of using something like Airbnb was the ability to search for stays by location. Realistically, a user would not just want to browse manually through an index list of all the stays available in order to find one in a specifc location. 

To allow my users the ability to filter through stays by location, I first built a separate SearchBar component in order to provide a form for my user to be able to submit a location input that I could render from within my container component while passing in the event handlers from my container component:

```
import React from 'react';

const SearchBar = (props) => {
  return (
    <div>
      <form onSubmit={props.handleSubmit} >
        <input type="text" placeholder="Enter a Location" name="searchInput" onChange={props.handleChange} value={props.searchInput} />
        <button type="submit" >Search Stays</button>
      </form>
    </div>
  )
}

export default SearchBar;
```

I next built a StaysFilterWrapper component that I would use to wrap my indexing container components, StaysList and FilteredStaysList, which I updated my "/stays" route in my App.js to render instead of my original StaysList component:

```
...

class App extends Component {

  render (){
    return (
      <div className="App">
        <Router>
            <Navbar />
              <Switch>
              {/*// <Route
              //   exact path="/"
              //   render={(props) =>
              //     <LogIn
              //       email={this.state.loginForm.email}
              //       password={this.state.loginForm.password}
              //       handleSubmit={this.handleSubmit}
              //       handleChange={this.handleChange}
              //     />
              //   }
            // />*/}
                <Route exact path="/" component= { Home } />
                <Route exact path="/stays" component= { StaysFilterWrapper } />
                <Route path="/stays/new" component={ NewStay } />
                <Route path="/stays/:id/edit" component={ EditStay } />
                <Route path="/stays/:id" component={ StayShowContainer } />
              </Switch>
        </Router>
      </div>
    );
  }
}
...
```

From within my StaysFilterWrapper class component, I imported the following and mapped the stays from my redux store to this component's props in order to access the stays for filtering. I also set up an initial state for this component with props for storing my user search input, an array of filtered stays based on that input (if any), as well as a prop to store whether or not a search had been conducted:

```
import React, { Component } from 'react';
import { connect } from 'react-redux';
import SearchBar from '../components/SearchBar';
import StaysList from "./StaysList";
import FilteredStaysList from "./FilteredStaysList";

class StaysFilterWrapper extends Component {
  constructor(props) {
    super(props)

    this.state = {
      searchInput: "",
      filteredStays: [],
      completedSearch: false
    }
  }

  render() {
	 return()
	}
	
const mapStateToProps = state => {
  return {
    stays: state.stays
  }
}

export default connect(mapStateToProps)(StaysFilterWrapper);
```

Next, I still wanted to be able to render my list of stays, as well as the new search bar:

```
return (
          <div>
            <h1>Stays</h1>

              <SearchBar />

              <StaysList />
          </div>
        )
```

I then needed to be able to capture the location input that a user would enter into the search field in order to use that input to filter through the available stays. Similar to those I needed previously to create and edit stays, I needed event handlers to handle any change in input in the search field, along with submission. The former would assign that input value as the value of the searchInput prop in my StaysFilterWrapper's state, and the latter would prevent the default submit action and instead call the function I intended to deal with filtering my stays:

```
handleChange = event => {
    this.setState({
      ...this.state,
      searchInput: event.target.value
    })
  }

  handleSubmit = event => {
    event.preventDefault()
    this.filterStaysOnSearch()
  }
```

These I passed from my parent, StaysFilterWrapper, to my SearchBar child component:

```
  <SearchBar
          searchInput={this.state.searchInput}
          handleChange={this.handleChange}
          handleSubmit={this.handleSubmit}
    />
```

Next I built out the function that would use the location gained from the searchInput to find stays whose locations matched that input in order to be able to display them to my user.  To do so I used .filter() to create a new array of stay objects for which those stay's locations were equal to that of the user entered location, stored as a variable that I then set as the new value of my filteredStays prop from within my setState() call. I also updated the value of my completedSearch prop to now be true, since a search had conducted once my stays were filtered based on the search input:

```
filterStaysOnSearch = () => {

    let myFilteredStays = this.props.stays.filter(stay => stay.location.toLowerCase() === this.state.searchInput.toLowerCase())

    this.setState({
      ...this.state,
      filteredStays: myFilteredStays,
      completedSearch: true
    })
  }
```

At this point, I needed to consider how I wanted to display to my user either the complete index of stays or the filtered list of stays.





