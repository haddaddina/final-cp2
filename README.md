# A Day in the Life of Dina for environmental change!

A walkthrough of a day in the life of myself, to show climate and environmental awareness in an ordinary day.

## Summary

The goals of this project are to bring individual environmental change awareness to the user of the game. This "game" will be interactive storytelling, in which the user of the site will walk through the life of Dina and make decisions that affect the rest of the day. Throughout the day, many elements will be recorded such as the amount of plastic consumed, how much emissions you wasted, money spent, etc. With these elements being tracked the user begins to notice their individual impact on the environment.

The main goal is to bring environmental awareness but another goal is to show the solutions to many of the causes of day to day environmental decisions. For example, the user will select ordering food for delivery instead of making food at home, the result was plastic used to store the food, emissions from the delivery person, and money wasted. With that path chosen towards the end of the day. The total of all the waste will be totaled and shown to the viewer with tips and tools to create less waste. 


## Component Parts

This project will be based in react and will be a platform on a website that people will have access to. 

## Challenges

The most time-consuming part will be just the creation of the pages, the hardest part, however, will be the storing of the information and creating the code for it to be tallied up and shown to the user at the end of the “day.”

## Timeline

- Week 1: Write the proposal

- Week 2: Real life research, aka, go out and perform the many decisions in the day of Dina, begin the webpage design and begin writing the pages.

- Week 3: Finish writing the different pages for the day in the life scenarios and begin creating the storage of the information.

- Week 4: Complete the storage of the information and work on the tally and page at the end with the tips and tools for environmental change. 

- Week 5: Present!

## References and link

Tutorials, comments, videos, magazine articles - Anything you found that helps you understand your project 

https://docs.gamesparks.com/tutorials/cloud-code-and-the-test-harness/storing-custom-player-data.html 

Looking at games such as Sims and BitLife to understand the cause and effect of decision making. 


 
##Final Code

import React, { Component } from "react";
import ReactDOM from "react-dom";

import "./styles.css";

var pages = {
  start: {
    content: "Welcome to a day in the life of Dina!",
    label1: "Start",
    label2: "Eh nah thanks",
    page1: "Begin",
    page2: "No",
    image: "Dina.jpg"
  },
  No: {
    content: "Why are you looking at this then? C'mon let's start!",
    page1: "Begin",
    label1: "Start",
    image: "meme.png"
  },
  Begin: {
    content: "It's friday morning, I have two classes today.",
    label1: "Wake up",
    label2: "Snooze",
    page1: "WakeUp",
    page2: "WakeUp",
    image: "alarm.jpg"
  },
  WakeUp: {
    content: "Will you make a breakfast or buy at A2?",
    image: "o.jpg",
    label1: "A2 Cafe",
    label2: "Make Breakfast",
    dollar1: 7.5,
    plastic1: 1,
    page1: "Plastic",
    page2: "Plastic"
  },
  Plastic: {
    content: "You have to go to school its either a 25 minute walk or an Uber",
    image: "desk.jpg",
    label1: "Uber",
    label2: "Walk",
    dollar1: 4.93,
    emission1: 484.8,
    page1: "School",
    page2: "School"
  },
  School: {
    content:
      "You look up at the clock on the wall and you're just half way done with class",
    label1: "Take a nap in class",
    label2: "Pay Attention",
    page1: "Lunch",
    page2: "Lunch",
    image: "1.jpg"
  },
  Lunch: {
    content:
      "It's lunch time you have a break between your two classes, you forgot to make food for the day. Do you just wait till you go home or buy another meal? ",
    label1: "Buy Lunch",
    label2: "Wait",
    dollar1: 10.97,
    plastic1: 2,
    page1: "Staying",
    page2: "Staying",
    image: "Lunch.jpg"
  },
  Staying: {
    content:
      "You have a ton of work to do, do you stay late and work on it at school or do you catch the shuttle home?",
    label1: "Stay Late",
    label2: "Go Home",
    page1: "Stay",
    page2: "Final",
    image: "work.jpg"
    //PaigeeWorld is the artist
  },
  Stay: {
    content: "You finished a lot of work but it's getting late and the bus is",
    label1: "Uber",
    label2: "Walk",
    dollar1: 4.98,
    emission1: 484.8,
    page1: "Final",
    page2: "Final",
    image: "Walk.png"
  },
  Final: {
    content: "Wow, you did a lot today let's see your impact:"
  }
};

class Page extends Component {
  render() {
    var pageData = pages[this.props.pageName];
    var goToPage = this.props.goToPage;
    var saveUserData = this.props.saveUserData;
    var getUserData = this.props.getUserData;

    function goToPage1() {
      goToPage(pageData.page1);
      if (pageData.dollar1) {
        saveUserData("dollars", pageData.dollar1 + getUserData("dollars")); // missing the old dollars that should be added to dollars1
      }
      if (pageData.plastic1) {
        saveUserData("plastic", pageData.plastic1 + getUserData("plastic")); // missing the old dollars that should be added to dollars1
      }
      if (pageData.emission1) {
        saveUserData("emission", pageData.emission1 + getUserData("emission")); // missing the old dollars that should be added to dollars1
      }
    }
    function goToPage2() {
      goToPage(pageData.page2);
    }

    var image = "";
    if (pageData.image) {
      image = (
        <div>
          <img className="main-page-image" src={pageData.image} />
        </div>
      );
    }
    var button1 = "";
    if (pageData.page1) {
      button1 = <button onClick={goToPage1}>{pageData.label1}</button>;
    }
    var button2 = "";
    if (pageData.page2) {
      button2 = <button onClick={goToPage2}>{pageData.label2}</button>;
    }

    return (
      <div>
        <p>{pageData.content}</p>
        {image}
        {button1}
        {button2}
        <p />
        The amount of money you spent:{getUserData("dollars")}
        <p />
        How many times you used plastic today: {getUserData("plastic")}
        <p />
        Emissions of CO2 you exhausted: {getUserData("emission")}
      </div>
    );
  }
}

class App extends Component {
  constructor(props) {
    super(props);

    this.state = {
      page: "start",
      userData: {}
    };

    this.goToPage = this.goToPage.bind(this);
    this.saveUserData = this.saveUserData.bind(this);
    this.getUserData = this.getUserData.bind(this);
  }

  goToPage(pageName) {
    this.setState({
      page: pageName
    });
  }
  saveUserData(key, value) {
    function updateState(state) {
      var newState = { userData: { ...state.userData, [key]: value } };
      return newState;
    }
    this.setState(updateState);
  }

  getUserData(key) {
    if (!(key in this.state.userData)) {
      return 0;
    }
    return this.state.userData[key];
  }

  //get user data function with just the (key)
  render() {
    return (
      <div className="App">
        <Page
          pageName={this.state.page}
          goToPage={this.goToPage}
          saveUserData={this.saveUserData}
          getUserData={this.getUserData}
        />
      </div>
    );
  }
}

const rootElement = document.getElementById("root");
ReactDOM.render(<App />, rootElement);
