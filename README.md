# react-cheatsheet
A React cheetsheet with useful commands and functions

## Creating a component with a  class
```javascript
class Component extends React.Component {
    // Will be called before it is mounted
    constructor(props) {
        // Call this method before any other statement or this.props will be undefined
        // in the constructor
        super(props);
        // The constructor is also often used to bind event handlers to the class
        // instance. Binding makes sure the method has access to component attributes
        // like this.props and this.state
        this.method = this
            .method
            .bind(this);
        // The constructor is the right place to initialize state.
        this.state = {
            active: true
        };
    }
    // Invoked just before mounting occurs (before render())
    componentWillMount() {

    }
    // Invoked immediately after a component is mounted. If you need to load data
    // from a remote endpoint, this is a good place to instantiate the network
    // request.
    componentDidMount() {

    }
    // Invoked before a mounted component receives new props.
    componentWillReceiveProps(nextProps) { 

    }
    // Invoked just before rendering when new props or state are being received.
    componentWillUpdate(nextProps, nextState) { 

    }
    // Invoked immediately after updating occurs. This method is not called for the initial render.
    componentDidUpdate(prevProps, prevState) {

    }
    // Invoked immediately before a component is unmounted and destroyed.
    componentWillUnmount() { 

    }

    //render is required in every component
    render() {
        return (
            <div></div>
        )
    }
}

```

## Render
Render a React element into the DOM in the supplied container and return a reference to the component (or returns null for stateless components)

```javascript
ReactDOM.render(element, container)
```

## Stateless component 
They're the ones that only need to have the render function)
```javascript
const NotFound = (props) => {
    return (
        <h2>Not Found!111!!</h2>
    )  
}
```


## Routing with react
```javascript
//this needs to be above the components that we are going to import
import { BrowserRouter, Route, Switch } from "react-router-dom";

const Router = () => (
    //Basename is the url base where the app is going to be deployed
    <BrowserRouter basename="/catch-of-the-day/">
        <Switch>
            //When the path is the home page it should render StorePicker or the HomePage
            <Route exact path="/" component={StorePicker} />
|           //When the path is /store/123 it will render component
            <Route path="/store/:storeId"    component={App}  />
            //When the path is not found it will render the notfound component
            <Route component={NotFound} />
        </Switch>
    </BrowserRouter>
);
render(<Router />, document.querySelector("#main"));
//To force the routing to one component to another we use the properties and push
this.props.history.push(`/store/${storeId}`);
```


## Helpers
```javascript
//To use a helper function in a separate file 
//We export the function in brackets 
import {getFunName} from './helpers.js'
<input type="text" defaultValue={getFunName()}/>
```


## Events
```javascript
//Events in react are done in line
<form onSubmit={this.goToStore}/>
class StorePicker extends React.Component{ 
    //first we need to bind our new component (goToStore)
    constructor(){
    super();
    this.goToStore = this.goToStore.bind(this);
    }
    goToStore(){
        event.preventDefault();
        //here we put anything we want our function to execute
    }
}
```

## References
```javascript
//To put reference in an element 
//storeInput if the reference to that input
<input type="text" defaultValue={getFunName()} ref={(input) => {this.storeInpput = input }}/>
//To get the reference
console.log(this.storeInpput)
```


## State
```javascript
//State is a representation of all the data in our appp. 
//Is a object that holds all the data. 
constructor(){
    super();
    this.state={
        fishes: 
        order:
    }
}
getInitialState(){

}

//To run a function that updates the state
addFish(fish) {
    //first take a copy of the state
    const fishes = this.state.fishes;
    //second we add our second fish
    fishes[`fish5`] = fish;
    //thrid we update our state
    this.setState({fishes});
}


//To render from an object
//Keys returns an array of the keys so that we are able to loop
{Object.keys(this.state.fishes).keys
//Then we use map to do somenthing for each key 
//We put inside a key to make it unique 
//We use details to pass the information inside each key to the component
//We use index to pass the key
.map(key => <Fish key={key}  index={key} details={this.state.fishes[key]}/>)}
//To access the details inside the component Fish 
this.props.details.whatever

```


## Props 
```javascript
//Passing props  to a component 
<Inventory
//We pass the state
fishes={this.state.fishes}
//And we pass the function
addFish= {this.addFish}
/>
//To access them in the Inventory component 
this.props.addFish

```
## Reseting a form 
```javascript
//To reset a form 
<form ref={(input) => this.fishForm = input}/>
this.fishForm.reset();
```


## Loading data with a click
```javascript
//To load data on a click 
<button onClick={this.props.loadSamples}></button>
//This needs to be where the state lives 
loadSamples(){
    this.setState({
        fishes: sampleFishes
    })
}

```

## To make an order

```javascript
//To make an order 
//First is goin to check if the status inside the details is available
const isAvailable = details.status === 'available';
//If it's available the button is going to be add to order, if not it will be sold out
const buttonText = isAvailable ? 'Add to order' : 'Sold Out';
//To disable the button when it's not available 
<button disabled={!isAvailable} onClick={ () => this.props.addToOrder(this.props.index)}></button>

//We make the function in the App component
addToOrder(key){
    //take a copy of the state
    const order = this.state.order;
    //update or add the new fish to the order
    //if it already had one it's going to add plus one, if not, just one. 
    order[key] = order[key] +1 || 1;
    //update our state 
    this.setState{(order)}
}

//To display the order in jsx 
//In the App
<Order fishes={this.state.fishes} order={this.state.order}/>

//We grab the order keys 
const orderIds = Object.keys(this.props.order);
//To get the total 
const total= orderIds.reduce((prevTotal, key) =>{
    //We get the fish in the order
    const fish = this.props.fishes[key]
    //How many 
    const count = this.props.order[key]
    //Is it available? 
    const isAvailable = fish && fish.status === 'available'
    //If it's available make the operation
    if(isAvailable) {
        return prevTotal + (count * fish.price) || 0
    } 
    //If not return the previous total 
    return prevTotal 
});

//To print the order 
<ul>
    {orderIds.map(this.renderOrder)}
</ul>

//We make the function that will render
renderOrder(key) {
    const fish = this.props.fishes[key]; 
    const count = this.props.order[key];
    //If the state of the fishs is unavailable
    if(!fish || fish.status === 'unavailable') {
        //If we now the name we return the fish name if not, we just say fish
        return <li>Sorry, {fish ? fish.name : 'fish'}is no longer availabe</li>
    }
    else { 
        return (
            <li>
                <span>{count}lbs {fish.name} </span>
                <span>{count*fish.price}</span>
            </li>
        )
    }
}

```




# Using firebase 
```javascript

// How to use firebase to save our data 
//We make a new file (base.js) 
import Rebase from 're-base'
const base = Rebase.createClass({ 
//We take the code of firebase
 // Initialize Firebase
 var config = {
    apiKey: "AIzaSyCxF4Lma4dKRSogNURQHcEvhe_a_EYztXo",
    authDomain: "catch-of-the-day-f3e0d.firebaseapp.com",
    databaseURL: "https://catch-of-the-day-f3e0d.firebaseio.com",
  };
  firebase.initializeApp(config);
});
export default base; 


//Back to app.js 
import base from '../base'
componentWillMount(){
    this.ref = base.syncState(`storeid/fishes`, 
    {
    context: this,
    state: 'fishes'    
    })
}

componentWillUnmount(){
    base.removeBinding(this.ref)
}
```

## Updating the state 
```javascript

//How to update of the state
<input onChange={(e) => this.handleChange(e,key)}/>
//We make the function 
    handleChange(e,key){
        //we take the fish that it's doing the change
        const fish = this.props.fishes[key]
        //we take the fish object
        const updatedFish = Object.assign({}, fish);   
        //And we update the fish with the new values
        updatedFish[e.target.name] = e.target.value;
        //We need to make a function to update the fish in the app and pass the key and the obj
        this.props.updatedFish(key, updatedFish)
}
//Then we update the state in the app
    updateFish(key, updatedFish){
            //We take a copy
            const fishes = this.state.fishes
            //We update the fish
            fishes[key] = updatedFish
            //We update the state
            this.state({fishes})
    }
<Inventory updateFish={this.updatedFish}/>
```





## Saving data in local Storage

```javascript
//How to save order in local Storage
//It's used when there are new props and the component needs to be updated 
//next props and nextstate are the new props and state
//we need to convert it with JSON
//this runs befre the component is rendered

componentWillUpdate(nextProps, nextState ){
localStorage.setItem('order', JSON.stringify(nextState.order));
}
//After setting we need to get the item before the component mounts
componentWillMount(){
    const localStorageRef = localStorage.getItem('order');
    //If there's somenthing in the local storage, refresh the state
    if (localStorageRef) {
        this.setItem({
            order: JSON.parse(localStorageRef)
        })
    }
    }

````


# Authentication

```javascript

    //How to make autentication 
    //We choose our provider and we follow the steps to activate it. 

    //First we set in default the ui and the owner inside the state
    this.state={
        uid: null, 
        owner: null
    }

    //We check if no one is loged 
    if(!this.state.uid){ 
        return 
        (
        <p>Sign in to manage your store's inventory</p>
        <button className="github" onClick={() => this.authenticate('github')}>Log In with Github</button>
        <button className="facebook" onClick={() => this.authenticate('facebook')} >Log In with Facebook</button>
        <button className="twitter" onClick={() => this.authenticate('twitter')} >Log In with Twitter</button>
        )
    }

//We check if it's the owner
    if(!this.state.uid !== this.state.owner){
        return ( 
            <p>You're not the owner of the store</p>
        )
    } 


    //We create the autenthicate fufnction 
    //We need to import base 
    import base from "./base"
    //Create the function that is going to handle the login with the provider and callback function
    authenticate(provider){
        base.AuthWithOAuthPopup(provider, this.authHandler)
    }
    //We create the authHandler 
    authHandler(err, authData){
        //This is the information for the user
        console.log(authData)
        //We need to save it into the state 
        //If there's an error...
        if(err){
            console.log(err);
            return;
        }
        //we need a ref to our firebase databse with a ref with the piece we want
        const storeRef= base.database().ref('owner')
        //we get the data once with a snapshot ((firebase object of data))
        storeRef.once('value', (snapshot) => {
                //the val has the actual value
                const data = snapshot.val() || {}
                //if it doesn't have an owner
                if(!data.owner){
                    storeRef.set({
                        owner: authData.user.uid;
                    })
                }
            //We set the state in the app 
                this.setState({
                uid: authData.user.uid,
                owner: data.owner || authData.user.uid
            })
        })
    }

    //To save the userID even when a refresh happens 
    //When the component mounts its going to lisent(on) when we load the page to auth again
    componentDidMount(){
        base.onAuth((user) => {
            //If there's already a user
            if(user){
                //We call the auth function with the user
                this.authHandler(null, {user})
            }
        })
    }
//To logout
<button onClick={this.logout}></button>

logout(){
    base.unauth();
    this.setState({uid: null})
}

```

