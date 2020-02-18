
> This is a mini project that implements a select box which suggests results when someone types in it. On clicking suggestions it is added to the selectbox as a removable pill which clears on click.

> Check demo and full code at [Codesandbox](https://codesandbox.io/s/gifted-cache-sywdt)

```javascript
//POC/MVP type project | does not focus on styling and detailed features, keeps all components and services in one file
//Github free Api rate limit exceeeds beyond threshold
//Author: Amit Sharma | Date 11 Feb 2020

import React, { Component } from "react";
import { Row, Col, Button, Form } from "reactstrap";
import axios from "axios";
import { from, of } from "rxjs";
import { map, catchError, first } from "rxjs/operators";

const DEFAULT_FAILURE_MESSAGE = { response: "failure", message: "" }; // default failure message.

// Method to get search results based on keywords
export const getResults = queryObject => {
  const queryObjectNew = queryObject ? queryObject : "";
  return from(
    axios.get(`https://api.github.com/search/repositories?q=${queryObjectNew}`)
  ).pipe(
    first(),
    map(response => {
      if (response && response.data) {
        return response.data;
      }
      return { ...DEFAULT_FAILURE_MESSAGE, message: "Results not retrieved." };
    }),
    catchError(err => {
      console.log(err);
      return of({
        ...DEFAULT_FAILURE_MESSAGE,
        message: "Failed to retrieve results."
      });
    })
  );
};

//Search list item
function SearchItem(props) {
  const handleSearchItemClick = event => {
    const { id } = props;
    props.handleSearchItemClicks(id);
  };

  return (
    <Col
      xs={{ size: 12 }}
      style={{
        border: "1px solid #c3c3c3",
        borderTop: "0px solid #c3c3c3",
        cursor: "pointer"
      }}
      onClick={handleSearchItemClick}
    >
      <span>
        <p
          id={props.id}
          name={props.name}
          className="my-1"
          onChange={handleSearchItemClick}
        >
          {props.name}
        </p>
      </span>
    </Col>
  );
}

//Selected item
function SelectedItem(props) {
  const handleSelectedItemRemove = event => {
    const { id } = props;
    props.handleSelectedItemRemove(id);
  };

  const { name } = props;
  return (
    <span
      style={{
        border: "0px solid #c3c3c3",
        borderRadius: "50px",
        cursor: "pointer",
        background: "#ebe8e8",
        padding: "5px",
        margin: "5px",
        marginBottom: "15px",
        paddingRight: "10px"
      }}
      onClick={handleSelectedItemRemove}
    >
      <span>{name}</span>
      <span>{` x`}</span>
    </span>
  );
}

class App extends Component {
  constructor(props) {
    super(props);
    this.state = {
      results: [],
      selectedItems: [],
      searchTerm: "",
      isError: false,
      submitDisabled: false,
      isLoading: false,
      isOpenResults: false //dangling comma
    };
  }

  componentWillUnmount() {
    if (this.searchSubscription) {
      this.searchSubscription.unsubscribe(); // free subscription
    }
  }

  handleSearchItemClicks = searchItemId => {
    //  handle clicks on search results

    let newResults = this.state.results;
    let newSelectedItems = this.state.selectedItems;

    let present = false;

    //check if already present
    for (
      let indexSelected = 0;
      indexSelected < newSelectedItems.length;
      indexSelected++
    ) {
      if (newSelectedItems[indexSelected].id === searchItemId) {
        present = true;
        break;
      }
    }

    //check if not already present, and add
    if (!present) {
      for (let index = 0; index < newResults.length; index++) {
        if (newResults[index].id === searchItemId) {
          newSelectedItems.push(newResults[index]);
        }
      }
    }
    this.setState({
      selectedItems: newSelectedItems
    });
  };

  handleSearchTermChange = evt => {
    // updates current search term to state
    const newSearchTerm = evt.target.value;

    this.setState({
      searchTerm: newSearchTerm,
      isLoading: true
    });

    this.searchSubscription = getResults(newSearchTerm).subscribe(data => {
      if (data && data.items) {
        this.setState({
          results: data.items.splice(0, 10), //using 10 results for now for simplicity
          isLoading: false,
          isOpenResults: true
        });
      } else {
        this.setState({
          results: [],
          isLoading: false,
          isError: true
        });
      }
    });
  };

  //Removes a selected item
  handleSelectedItemRemove = itemId => {
    const newSelectedItems = this.state.selectedItems.filter(
      item => item.id !== itemId
    );

    this.setState({
      selectedItems: newSelectedItems
    });
  };

  //Closes search results
  toggleResults = () => {
    this.setState({
      isOpenResults: false
    });
  };

  //submits selected items to console
  handleSubmit = event => {
    event.preventDefault();
    console.log(this.state.selectedItems);
  };

  render = () => {
    const title = this.props.title ? this.props.title : "";
    return (
      <>
        <Form id="searchSelectApiForm" onSubmit={this.handleSubmit}>
          <div id="apiSearch" name="apiSearch" className="my-4">
            <Row>
              <Col xs={{ size: 4 }}>
                <label className="form-label" htmlFor="searchTerm">
                  {title} Search Hint: reactb{" "}
                </label>
              </Col>
              <Col xs={{ size: 7 }}>
                <Row>
                  <Col
                    xs={{ size: 12 }}
                    style={{
                      border: "1px solid #c3c3c3",
                      background: "#FFF",
                      borderRadius: "50px",
                      padding: "15px"
                    }}
                  >
                    {this.state.selectedItems.length > 0 && (
                      <>
                        {this.state.selectedItems.map(item => (
                          <SelectedItem
                            key={item.id}
                            {...item}
                            handleSelectedItemRemove={
                              this.handleSelectedItemRemove
                            }
                          />
                        ))}
                      </>
                    )}
                    <input
                      type="text"
                      className="form-control input-sm"
                      id="searchTerm"
                      name="searchTerm"
                      value={this.state.searchTerm}
                      onChange={this.handleSearchTermChange}
                      style={{
                        border: "0px solid #c3c3c3",
                        background: "#eFeFeF",
                        width: "auto",
                        display: "inline-block"
                      }}
                      placeholder="Search Github repo name"
                    />
                  </Col>
                  {this.state.results.length > 0 && this.state.isOpenResults && (
                    <Col xs={{ size: 12 }}>
                      {this.state.results.map(result => (
                        <SearchItem
                          key={result.id}
                          {...result}
                          handleSearchItemClicks={this.handleSearchItemClicks}
                        />
                      ))}
                    </Col>
                  )}
                </Row>
              </Col>
            </Row>
          </div>
          {this.state.isLoading && <></>
          //Loading animation here
          }
          {this.state.results.length > 0 && this.state.isOpenResults && (
            <Row className="mt-5">
              <Col
                xs={{ size: 4, offset: 4 }}
                style={{ marginLeft: "33.33333%", marginTop: "10px" }}
              >
                <Button
                  block
                  className="btn-dark-orange btn-rounded btn-lg"
                  onClick={this.toggleResults}
                >
                  Close results
                </Button>
              </Col>
            </Row>
          )}
          <Row className="mt-5">
            <Col
              xs={{ size: 4, offset: 4 }}
              style={{ marginLeft: "33.33333%", marginTop: "10px" }}
            >
              <Button
                block
                className="btn-dark-orange btn-rounded btn-lg"
                type="submit"
                disabled={this.state.submitDisabled}
              >
                Submit to Console
              </Button>
            </Col>
          </Row>
        </Form>
      </>
    );
  };
}

export default App;
```

Previous: [Lifecycle Methods](lifecycle.md)