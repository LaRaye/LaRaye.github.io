---
layout: post
title:      "React-Redux Project: Adding Search Part 2"
date:       2019-12-14 00:05:45 +0000
permalink:  react-redux_project_adding_search_part_2
---


At this stage, I've set up everything I need to grab my filtered stays based on the location input provided by my user. I now just need to worry about to render them as opposed to my full index of stays. Initially, to render my list of stays, I was using my StaysList container component to map my stays from my redux store to my StaysList props, then mapping over each stay and rendering a StayCard component with that stay's data.

```
import React, { Component } from 'react';
import { connect } from 'react-redux';
import StayCard from '../components/StayCard';
import { Grid } from "../components/CardStyle";

class StaysList extends Component {

  render() {
    console.log(this.props.stays)

    if (this.props.stays.length === 0) {
      return <p>Loading...</p>
    }

    return (
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
    )
  }
}

const mapStateToProps = state => {
  return {
    stays: state.stays
  }
}

export default connect(mapStateToProps)(StaysList);
```

To render my filtered stays, I could repurpose this same StayList container component, and change how the component is receiving the stays it is mapping over, through what it receives as props from the new FilterStaysWrapper parent component, whteher that be the entire list of stays from my redux store, or the filtered list of stays. However, I felt as though the additional logic that I would want to include, for example, in case there were no search results for that location etc. made it so that it made it neater in my mind to have an entirely separate container component for my filtered stays. To that end, I created a FilteredStaysList component that would be primarily responsible for mapping over the filtered stays provided by my FilteredStaysWrapper, and rendering StayCard components for each.

```
import React, { Component } from 'react';
import StayCard from '../components/StayCard';
import { Grid } from "../components/CardStyle";

class FilteredStaysList extends Component {

  render () {
    if (this.props.completedSearch && this.props.filteredStays.length === 0) {
      return (<p>Sorry we cannot find any stays at that location</p>)
    }

    return (
      <div>
        <Grid>
          {this.props.filteredStays.map(stay =>
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
    )
  }
}

export default FilteredStaysList;
```

Addtionally, this is where I placed the if statement to handle the cases where a search has been completed, but no stays were found matching the search criteria. 

Back within my FilteredStaysWrapper, I began with rendering the inital components I still wanted my user to see on navigating to the stays route: StaysList and now SearchBar as well. Note that being passed into the SearchBar component are the search input provided by the user and the event handlers previously built to capture any change in the search input field and the submission of an input:

```
render() {

    if (this.props.stays.length === 0) {
      return <p>Loading...</p>
    }

        return (
          <div>
            <h1>Stays</h1>

              <SearchBar
                searchInput={this.state.searchInput}
                handleChange={this.handleChange}
                handleSubmit={this.handleSubmit}
              />

              <StaysList />
          </div>
        )
    }
  }
}
```

I know also wanted to be able to render the list of filtered stays if a user performed a search. In order to do so, I added another if/else statement within my render() aside from the inital one handling the case where during the async call to the backend, stays are not yet mapped to props. This if/else statement makes it such that if the searchInput is no longer an empty string, as it was first set in the component's intial state, or if the user submits an input, then what will instead be rendered is the SearchBar component and the FilteredStaysList component. Otherwise, the SearchBar and StaysList component will be rendered. What is useful about about using the searchInput as an empty string as the condition is that the complete list of unfiltered stays is shown if a user clicks submit on the search bar without an input. 

```
render() {

    if (this.props.stays.length === 0) {
      return <p>Loading...</p>
    }

    if (this.state.searchInput !== "") {

      return (
        <div>
          <h1>Stays</h1>

            <SearchBar
              searchInput={this.state.searchInput}
              handleChange={this.handleChange}
              handleSubmit={this.handleSubmit}
            />

            <FilteredStaysList
              filteredStays={this.state.filteredStays}
              completedSearch={this.state.completedSearch}
            />
        </div>
      )
    } else {

        return (
          <div>
            <h1>Stays</h1>

              <SearchBar
                searchInput={this.state.searchInput}
                handleChange={this.handleChange}
                handleSubmit={this.handleSubmit}
              />

              <StaysList />
          </div>
        )
    }
  }
}
```

Here I am passing in as props from FilteredStaysWrapper to FilteredStaysList, the array of filtered stays, and whether a search has been completed. Going back to my FilteredStaysList component, I am using this second prop to ensure that the user is not receiving a message that no stays have been found at the location provided if just the filteredStays prop is an empty array, as it is initally set as an empty array, and remain so if no search is actually performed. Therefore a search must have been performed, and the filteredStays prop must be empty, aka no stays with that location have been found, in order for that to be the case. 

```
  render () {
    if (this.props.completedSearch && this.props.filteredStays.length === 0) {
      return (<p>Sorry we cannot find any stays at that location</p>)
    }

    return (
      <div>
        <Grid>
          {this.props.filteredStays.map(stay =>
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
    )
  }
}
```

Now, all of this functions to, on initially navigating to the stays route, displaying a search bar and list of stays to the user, and then rendering a filtered list of stays if the user submits a location to search by, based on that input. More work is needed, however to clear the searchInput and potentially the array of filteredStays once a search has been conducted and the results displayed to the user. Right now, the previously found search results render when a user is typing in a new search location. This is because both the filteredStays prop and the completedSearch prop still have the value they were set to within the call to the filtering function. As this point something needs to be built to deal with resetting all of this for the next search. 

Addtionally more logic needs to be added to the filtering process to account for things like white space in location inputs where the location is something like "San Diego", etc. 



