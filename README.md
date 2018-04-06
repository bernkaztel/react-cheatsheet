# react-cheatsheet
A React cheetsheet with useful commands and functions

##Creating a component with a  class
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

