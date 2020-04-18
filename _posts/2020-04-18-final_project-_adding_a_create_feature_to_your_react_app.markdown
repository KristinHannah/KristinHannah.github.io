---
layout: post
title:      "Final Project- Adding a "create" feature to your react app"
date:       2020-04-18 20:58:59 +0000
permalink:  final_project-_adding_a_create_feature_to_your_react_app
---


React has not been my favorite section. Starting out the final project was intimidating. So I opted to follow along with some video lectures flatiron had. At the end of the project, I feel so much more comfortable with react. I'm going to walk you through the steps of adding an "create" function to your react app. 

First, you need to decide where you would like the form to appear. For my final project I made an app for brides-to-be to keep track of vendors. They're categorized by type, so there is one section for photographers, caterers, venues, florists, you get the idea. In each section they can enter their possible vendors, quotes, and availability. Once hired, they can enter that too. 

We have a bride who is looking into getting a photobooth for her big day. So she needs to add another vendor type. We've got to add a form so she can add her own vendor types, other than the ones we've already created.

I have a VendorTypesContainer where I am rendering components related to vendor types, such as a list of vendor types that are already there. To me, this seems like a good place to put the form too. The form can go at the top, and the list of current vendor types can go right below. To add in the form, I will need to add somethings to my VendorTypesContainer. 

## VendorTypesContainer Additions:
I need to make sure I'm importing the form. I am going to call it "VendorTypeInput", and it will go in my components folder. At the top of the VendorTypesContainer, I would add:
```
import VendorTypeInput from '../components/VendorTypeInput'
```

Great, now I need to make sure the form will be rendered. In the render function, I would the below:
```
<VendorTypeInput />
```

## VendorTypeInput Component
So now our container is importing and rendering this VendorTypeInput component. However, it does not yet exist. So let's add a file to our components folder called "VendorTypeInput.js".

This component will need to have state so that we can create a controlled form, so it should be a class component. 

We need to import some things:
```
import React from 'react';
import { connect } from 'react-redux';
```

We will need this component to be able to connect with our store since it will be responsible for creating new vendor types. 

What should go inside the component? As mentioned before we need the state. It should include the attributes the user will enter for the VendorType:

```
    state = {
        name: '',
        img: ''
    }
```

I want them to be able to upload an image representing the vendor type, to add a little pizazz to list of vendor types. 

Each component has to render something. This one will be rendering the form for users to fill in. See the controlled form below. 

```
 render() {
        return (
            <div>
                <h1> Add a new type of vendor you are looking to hire here (such as florist, photographer):</h1>
                <form onSubmit={this.handleSubmit}>
                    <label>Vendor Type: </label> <br />
                    <input type="text" placeholder="name" name="name" value={this.state.name} onChange={this.handleChange} /> <br /> <br />
                    <label>Image (should be an URL): </label> <br />
                    <input type="text" placeholder="Image URL" name="img" value={this.state.img} onChange={this.handleChange} />
                    <input type="submit" />
                </form>
            </div>
        )
    }
```

I've included references to a few functions in my form: handleSubmit, and handleChange. 

The handleChange function is going to make sure that when a user enters something in the input, our state is changing to match it:

```

    handleChange = (event) => {
        this.setState({
            [event.target.name]: event.target.value
        })
    }
```

And of course, we need the handleSubmit to persist our changes on the backend. We don't want the page to refresh, so remember to add an "event.preventDefault()". To persist our changes we will need to add an action. We'll call it "addVendorType". It will come in as a prop, and we will send it the state which holds the information for the new vendor type we're creating. Make sure the rest the state so a new vendor type can be added afterwards.

```
 handleSubmit = (event) => {
        event.preventDefault();
        this.props.addVendorType(this.state)
        this.setState({
            name: '',
            img: ''
        })
    }
```

Back to that addVendorType action. From what we've done so far, our component will not know where or what that is. We've got to import it. I'm going to create a file in my actions folder called addVendorType, so I'll import it like this in the input component. 
```
import { addVendorType } from '../actions/addVendorType';
```

But that is not all, we need to add this as a mapDispatchToProps at the bottom of the file, like so. Remember to add null before the action. The connect always looks at the first item in the paranthese to be a mapStateToProps. We don't have one, so we can put nul.
```
export default connect(null, { addVendorType })(VendorTypeInput)
```

## addVendorType Action

The data the user entered has to be sent to the backend. We can do this using a fetch request. The data will come in as parameters. 

```
export const addVendorType = (data) => {
    return (dispatch) => {
        fetch('http://localhost:3000/api/v1/vendor_types', {
            headers: {
                'Content-Type': 'application/json',
                'Accept': 'application/json'
            },
            method: "POST",
            body: JSON.stringify(data)
        })
            .then(resp => resp.json())
            .then(data => dispatch({
                type: 'ADD_VENDORTYPE',
                payload: data
            }))
    }
}
```

At the end we are dispatching two items, the action type, and the payload where we are storing the new vendor type data. Where is this being dispatched to? Anyone??

## vendorTypeReducer
Yes, that's right it's to our reducer. My reducer is already set up with my 'FETCH_VENDORTYPES' action for my vendorTypeList I was teling you about before. We need to add a case for the action.type "ADD_VENDORTYPE". 

```
case 'ADD_VENDORTYPE':
            return { ...state, vendorTypes: [...state.vendorTypes, action.payload.data] }
```

I'm returning the original state, and adding in the new vendor type that's coming in through the payload.

and voila. 

Now you can add your photobooth vendors, DJs, lighting specialists, whatever you're looking for!


