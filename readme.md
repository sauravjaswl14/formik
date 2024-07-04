Forms are vital part of any business application. We use forms to register, login, submit a request,place an order, schedule an appointment and perform countless other tasks.

While developing forms it's important to create an experience that guides the user efficiently and effectively through the workflow.

As developers we need to:
-Handle form data
-validation
-visual feedback with error messages
-Form submission

formik: Formik is a small library that helps you deal with forms in React and React Native.

WHY WOULD YOU WANT TO USE FORMIK?

Well formik helps you with three main parts

-> Managing the form data i.e getting the values from different form fields in and out of the form state
-> Form Submission :- formik helps you easily handle form submissions
-> Form validation and displaying error messages

Formik helps you deal with forms in a scalable, performant and easy way.

## COURSE STRUCTURE

-Build a simple form
-useFormik Hook
-Manage the form state
-Handle form submission
-Form form submission
-Form Validation
-Formik Components : we'll see various components that formik provides to abstract some repetition and the code
-Few handy features
-Reusable components for input, textarea, select, radio buttons and checkboxes
Build a user registration form

import {useFormik} from 'formik'

//A hook is basically a function, we need to invoke it inside our component

export default function YoutubeForm(){
//useFormik hook accepts an object as a parameter

//this hook will then return an object which contains variety of useful properties and methods that we could use with our form

const formik = useFormik({})

//now this formik object is what is going to help us with the three points:

1. Managing the form state
2. Handling form submission
3. Validation and error messages

}

MANAGING FORM STATE

let's understand how formik helps us with managing our form state, now what do we mean by that

We know that our youtube form has three fields: Name, Email and Channel

At the moment thought we're not tracking the values of these form fields, when we type in something the value of the form field changes and in React if a value changes we need a state variable for that. We need state variables for Name, Email and channel. Collectively we can call that form states

Form State is nothing but an object which maintains the value of different form fields

if we change the name to "Vishwas" it should be reflected in the form state object, if we change the email to "v@example.com", the form state again should be updated, similarly change the channel to "Codeevolution", the same should be reflected in our form state.

if we are able to manage this form state we can eventually submit this data when the user click on the submit button.

import React from 'react'
import {useFormik} from 'formik'

function YoutubeForm(){

const formik = useFormik({})

return (

  <div>

    <form>

      <label htmlFor='name'>Name</label>
      <input type='text' id='name' name='name'/>

      <label htmlFor='email'>Email</label>
      <input type='email' id='email' name='email'/>

      <label htmlFor='channel'>Channel</label>
      <input type='text' id='channel' name='channel'/>

      <button>Submit</button>

    </form>

  </div>

)
}

The first step is to pass in a property called initialValues in the object we pass to the useFormik Hook

the initialValues property is an object

const formik = useFormik({
initialValues: {
name: '',
email: '',
channel: ''
}
})

this object contains the initial values for all our form fields

It is very important to note that the properties for initial values correspond to the name attribute of the individual fields

STEP 2: WE NEED TO ADD THE onChange and the value prop for each of the form fields, this is required to ensure the form fields are tracked in react by formik

this is where the formik constant comes into picture

<input type='text' id='name' name='name' onChange={formik.handleChange} value={formik.values.name}/>

Once you do this the formik will automatically track the form field values for you.

<input type='email' id='email' name='email' onChange={formik.handleChange} value={formik.values.email}/>

console.log('Form values', formik.values)

so we want to take peek into what formik.values actually is which we're using here for our value prop

formik.values is a object where the key corresponds to the name attribute of the form field and then the value of that form field

formik.values is an object which always reflects the state of the form, this is what we are feeding back as the value to the form fields

handleChange method is formik's helper to update the values object

step1: we pass in the initial values to our useFormik hook. the properties must match the name attribute of the form fields. This initialValues object is mandatory

step2: we need to add the onChange and the value attribute or props to all the fields that need to be tracked. the onChange prop is equal to formik helper method called handleChange, this method will basically read the name attribute and update the corresponding property in the values object. the values object will contain values for all the form fields. we then access each of them individually and assign it to value prop on each of the form fields

Flow:-

First, take the values from initialValues object and set that in the values object. Then when you change the form field values the onChange event is triggered which fires the handleChange method which will update the values object.

When the values object updates, it's passed back into the form field. This cycle ensures that the form state is managed correctly

## Handling Form submissions

Recap:

we learned how to manage our form state using formik, We learned that we need to specify the initialValues in the object passed to the useFormik Hook. We learnt that we need to add the onChange prop to all our form fields. and we also learnt that formik.values contains all of the form field values which can then be assigned individually to the form fields. let's learn how to get hold of the form state when the user clicks on the subit button.

-> Handling Form submissions with formik is really simple. there are two steps.

For the first step we need to specify the onSubmit handler on the form tag and this is going to be equal to formik.handleSubmit

<form onSubmit={formik.handleSubmit}>

So again our formik constant is giving us a helper method to handle form submission.

for the second step we need to revisit the object we pass in to the useFormik Hook

As it turns out apart from initial values we can specify another property called onSubmit. this property is a method which automatically receives the form state as its argument.

onSubmit we are going to have a arrow function which receives value as it's argument.

function YoutubeForm(){
const formik = useFormik({
initialValues: {
name: 'Vishwas',
email: '',
channel: ''
},
onSubmit: values => {
console.log('Form data', values)
}
})
}

What we need to notice is that values is pretty much what we were refering to so far using formik.values

And the best thing here is when you click on the submit button, formik is going to automatically execute this onSubmit method.

<button type="submit">Submit</button>

First step, on the form tag specify onSubmit prop set it equal to formik.handleSubmit. What this will do is automatically tie the onSubmit method to the submit event.
the onSubmit method at all times will receive the latest values of the form data as it's argument. this can then be used as per the requirement

## Form Validation

What formik does is let you define a validation function. And that validation function needs to be assigned to a property called validate in the object that we pass to the useFormik hook. so just like onSubmit we specify a third property called validate and this again just like the onSubmit property is a function which automatically recieves the values object as it's argument. and we know that values is an object which contains three properties with three form fields. so we pretty much hanve values.name, values.email and values.channel

this validate function is a function tha must satisfy some conditions for formik to work as intended.

The first condition is that this function must return an object

the second condition is that the keys for this error object should be similar to that of the values object i.e errors.name, errors.email, errors.channel

Again these keys will corespond to the name attribute for the three form fields.

the third condition is that the values of these keys should be a string indicating what the error message should be for that particular field.

for example errors.name = "This field is required"

so if name is a mandatory field this is the error message we want to be displayed.

function YoutubeForm(){
const formik = useFormik({
initialValues: {
name: 'Vishwas',
email: '',
channel: ''
},
onSubmit: values => {
console.log('Form data', values)
}
})

validate: values => {
let errors = {}

    return errors

}
}

validate: values => {
const errors = {}

if (!values.name) {
errors.name = 'Required'
}

if (!values.email) {
errors.email = 'Required'
} else if (!/^[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,4}$/i.test(values.email)) {
errors.email = 'Invalid email format'
}

if (!values.channel) {
errors.channel = 'Required'
}

return errors
}

lets refactor the code:

const initialValues = {
name: 'Vishwas',
email: '',
channel: ''
}

const onSubmit = values => {
console.log('Form data', values)
}

const validate = values => {
const errors = {}

if (!values.name) {
errors.name = 'Required'
}

if (!values.email) {
errors.email = 'Required'
} else if (!/^[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,4}$/i.test(values.email)) {
errors.email = 'Invalid email format'
}

if (!values.channel) {
errors.channel = 'Required'
}

return errors
}

function YoutubeForm(){
const formik = useFormik({
initialValues,
onSubmit,
validate
})
}

## Displaying error messages

previously, we created the validate function that checks the form field values and returns the appropriate error message for each of those fields, in this section let's see how to display these error messages.

We've already learnt that the formik object returned by useFormik() contains alot of useful properties and helper methods to manage our form.

It is this very object that also helps us to get hold of the error messages.

let's revisit one of the properties formik provides.

Formik also provides us with another property called errors and this again just like the values object is an object with the key value pairs for each of the form fields

console.log('Form errors', formik.errors)

you can see that the errors object is now populated with the same keys as the value object with the value itself for each of the keys is the error message that we have specified in our validate function

Form errors {name: "Required", email: "Required", channel: "Required"}

so basically, formik runs the validate function on change of your youtube form and then populate this formik.errors object all we have to do now is access these error messages using the object dot notation in js

<form onSubmit={formik.handleSubmit}>
  <label htmlFor='name'>Name</label>
  <input type='text' id='name' name='name' onChange={formik.handleChange} value={formik.values.name}/>
  {formik.errors.name ? <div>{formik.errors.name}</div> : null}
</form>

## Visited Fields

we learnt how to display a error message for particular field but we did have a setback. while our form works and the user sees error, it's not a great user experience for them.

Our validation function runs on each key stroke against the entire forms values and our errors object contains all validation errors at any given moment and in our youtubeform jsx we are just checking if an error exists and then immediately displaying it to the user.

this is awkward since we are going to show error messages for fields that the user hasn't even visited yet.

If there are 20 fields we should not display error messages for all the 20 fields if the user has just interacted with the first field.

Most of the time we only want to show a field's error message after our user is done typing in that field

Now the question is how do we keep track of the fields the user has interacted with or in simpler terms how do we keep track of the visited fields in a form, well that is where formic again comes to our help

if we have to track whether a form field has been visited we have to add the onBlur prop on the form input element and to this prop we pass in formik.handleBlur

Again handleBlur is a helper method that we get from our formic constant

<form onSubmit={formik.handleSubmit}>
<div className='form-control'>
<label htmlFor='name'>Name</label>
<input 
type='text' 
id='name'
name='name'
onChange={formik.handleChange}
onBlur={formik.handleBlur}
value={formik.values.name}
/>
{formik.errors.name ? (
  <div className='error'>{formik.errors.name}</div>
  ): null}
</div>

So we have done what formik requires us to do, keep track of the visited fields. But where does formik stores this information. It stors it in the object called touched. And that object is of the same shape as values object.
So similar to formik.values and formik.errors we also have formik.touched

console.log('Visited fields', formik.touched)

so the touched object gives us information if the field has been visited or not if it has been visited the property would be present with the value of true

## Improving validation UX

We learnt about the touched object that formik maintains to let us know the visited fields in a form.
let's use that object to improve the validation user experience

if you take a look at formik.touched object it maintains a key for every field, all we have to do now is render the error messages only if the field's error message exists and the user has visited that particular field

<form onSubmit={formik.handleSubmit}>
<div className='form-control'>
<label htmlFor='name'>Name</label>
<input 
type='text' 
id='name'
name='name'
onChange={formik.handleChange}
onBlur={formik.handleBlur}
value={formik.values.name}
/>
/**only if the field has been visited and the error message exists then show the error message**/
{formik.touched.name && formik.errors.name ? (
  <div className='error'>{formik.errors.name}</div>
  ): null}
</div>
</form>

To quickly summarize what we have done is handled the onBlur event of the input fields, we pass in a formik helper method called handleBlur as the event handler. What this does is store the information on whether a particular field has been visited or not and that information is stored in an object called touched. This touched object can then can be used to conditionally render the error message for a particular field.

This is pretty much the fundamentals about form validation with formik in react but as it turns out formik provides an alternate way of specifying the validation rules for any given form

## schema validation with Yup

let's discuss an alternate way of writing validation rules with formik. This alternate way depends on the library called yup.

-> npm i yup

import \* as Yup from 'yup'

Now what we're not going to do is understand how yup works. Instead we're just going to see how to rewrite our validate function with yup. once we understand what yup can do you can then explore on your own what yup is capable of.

For our first step we need to write a validation schema object. This is pretty much what yup is for: object schema validation.

so after the validate function, let's create a new constant called validationSchema, to this we assign a yup object schema.

const validationSchema = Yup.object()

to this we need to pass in an object which contains the rules for each of our form fields.

const validationSchema = Yup.object({
name: Yup.string().required('Required'),
email: Yup.string().email('Invalid email format').required('Required'),
channel: Yup.string().required('Required')
})

the second step is to pass this schema into our useFormik Hook

function YoutubeForm(){
const formik = useFormik({
initialValues,
onSubmit,
validationSchema
//validate
})
}

we're going to see how we can further simplify our code with formik by reducing boilerplate

if we look at our form fields, we can see that three props are sort of similar across the three fields:

onChange={formik.handleChange}
onBlur={formik.handleBlur} and
value which is accessed from formik.values using the name attribute

example:- value={formik.values.name}

So, formik sees this and tells us, "hey you sort of seem to be adding the same lines of code every time you create a new field, in other words there is some boilerplate code for every field in this form, let me help you with that, i am goiing to provide you a helper method called 'getFieldProps' which behind the scenes will add these props for you"

So what we can do is replace the three lines of code with a single call to formik.getFieldProps()

to this method we pass one argument which is the name of the form field. for our first field the string "name" is the name attribute

formik.getFieldProps('name')

Also for this helper method we need to add the spread operator

{...formik.getFieldProps('name')}
{...formik.getFieldProps('email')}
{...formik.getFieldProps('channel')}

<div className='form-control'>
  <label htmlFor='email'>E-mail</label>
  <input
    type='email'
    id='email'
    name='email'
    {...formik.getFieldProps('email')}
  />

{formik.touched.email && formil.errors.email ? (

<div className='error'>{formik.errors.email}</div>
): null}

</div>

And that pretty much is our first refactor

## Formik Components

We learnt about getFieldProps() helper method with which we could get rid of some boilerplate code. Although that is decent we still have to manually pass each input the getFieldProps() helper method

To save us even more time formik actually provides a few components that implicitly use react context to make our life easier and our code less verbose

Out of the several components that the formik library provides , we are going to take a look at four of them to refactor our YoutubeForm component:

Formik
Form
Field
ErrorMessage

The Formik component is a replacement to the useFormik hook. The argument which we pass to useFormik as an object will be passed in as props to the Formik Component

Let's see how to replace useFormik hook with the formic component step-by-step

first step: import Formik instead of useFormik

import {Formik} from 'formik'

step2 we remove the call to use formik

step 3 we wrap our entire form with this Formik component

final step: pass in the different props to this formik component and these are the same which we had specified when calling the useFormik hook

<Formik 
initialValues={initialValues} 
validationSchema={validationShema} 
onSubmit={onSubmit}>

  <form onSubmit={formik.handleSubmit}></form>
</Formik>

and that pretty much is the code refactoring for the formik component. It infact behaves as a context provided component that provides the different properties and helper methods for the other three components we are going to use .

## Form Component

We used the Formik component in our code, although that component doesnot necessarily reduce the number of lines or simplify our code it is needed for the remaining three components that do simplify our code

let's take a look at the Form component

first step: import form

import {Formik, Form} from 'formik'

second step: replace the html form element with the Form component

Third and final step: remove the onSubmit prop

We're able to do this because Form component is a small wrapper around the html form element that automatically hooks into formik's handleSubmit method

So the Form component helps us by automatically linking the onSubmit method to our form's submit event.

## Field Component

This component simplifes the code for a form field. Right now for every field we specify the getFieldProps() helper method passing in the name attribute value. Formik sees this as a common pattern which can be abstracted and provides us with the Field component to simplify the code.

first step: import Field from formik

import {Formik, Form, Field} from 'formik'

second step: replace each input tag with the Field component

third and final step: get rid of getFieldProps() helper method from each of the fields, and we are able to do this because the field component does three things

first, it will behind the scenes hookup inputs to the top-level Formik component

second, it uses the name attribute to match up with the formik state

third, by default field will render an input element which is what out youtubeform had as well

## ErrorMessage Component

At the moment for displaying error messages we check if the field has been visited, check if the error exists and if it does we render that error. This again seems like a pattern across the different form fields and when there is a pattern, formik wants to help us out.
And it is for this particular scenario formik provides the ErrorMessage Component

step 1: import ErrorMessage from formik

import {Formik, Form, Field, ErrorMessage} from 'formik'

step 2: Replace the block of code rendering the error message with the ErrorMessage Component

step 3: pass in a name prop which is equal to the name attribute on the Field Component

<ErrorMessage name='name'>

Now this replacement is possible because the error message component behind the scenes will take care of rendering the error message for particular field indicated by name prop, only if the field has been visited and if the error exists.

Again it is abstraction over what we were achieving manually.

## Let us quickly summarize the steps to refactor the form

first, import Formik and then wrap your entire form with the Formik component. this Formik component accepts initialValues, ValidationSchema and onSubmit handler as props.

next, replace the form html tag with the Form component from formik. This will automatically link the onSubmit event to the onSubmit method passed into Formik

Replace each of the form fields with the Field component. This Field component hooks into formik using the name attribute. It'll takecare of handling the value, handling onChange and the onBlur event

Finally, for your error message use the ErrorMessage component which conditionally renders the error corresponding to a form field only if the field has been visited and if the error exists

## Journey so Far

We started off by creating a simple form with three fields. That form though was not connected to react in any way. So we then installed the formik library and used the useFormik Hook to connect the form to react.

We needed to achieve three things:

1. Managing form state
2. Handling form submission
3. form validation

To handle form state we used the initialValues object and the handleChange helper method provided by formik.

To handle form submission, we used the onSubmit method and the handleSubmit helper method provided by formik

Validation wise we had a few more concepts to understand

we learnt about the validate function which returns an object containg the error messages for all the form fields

We also had a look at an alternate way of specifying the validation rules which was the validationSchema object, this was possible with the yup library

once we had the rules we could display them using the errors and touched objects accessing each individual field

Once we had a fully functional form we then refactored the code to use the components provided by the formik library.

We used Formik which works as the context provider, Form which takes care of the regular form tag, Field which takes care of the input fields and finally the ErrorMesssage component which takes care of validation.

## Field Revisited

By default Field component renders an HTML input element

Behind the scenes, Field Component hooks up input elemrnt to formik, i.e it hooks up handleChange, handleBlur and value of the form field

let's discuss few additional points about this Field component

Field component will pass through any additional props that you specify.

At the moment we are passing in the id attribute to all the field components, if you go to the browser and inspect we can see that the same has been passed through to the input element as well.

Similarly if we were to specify a placeholder prop on the channel field, the same would get passed through to the input element and you will be able to see the placeholder as well.

-> Any additional props in the Field components will be passed through

-> The ability to render a different element other than the input element.

let's say we want to add a textarea field to our Youtube form

first add it to the initial value object

const initialValues = {
name: 'Vishwas',
email: '',
channel: '',
comments: ''
}

To instruct formik to render Field component as textarea we simply need to add the as prop passing in 'textarea'. this is now going to render Field component as textarea element

<div className="form-control">
<label htmlFor="comment">Comments</label>
<Field as='textarea' id='comments' name='comments'/>
</div>

Now this as prop can accept as it's value either input or textarea or select or a custom react component as well. It's default value is input which is why we don't have to specify the prop when rendering an input element.

We'll learn more about rendering select drop-down and custom react components later on.

Field component accepts an as prop to decide what element to render

The third and final point of discussion is around the render props pattern now this is a slightly more advanced pattern which will give you more fine-grained control over the rendering and behavior of your form field.

We're not going to discuss in detail how to use each of the props that the render props object will provide, but we'll see how to use this render props pattern to create a field similar to what we already have.

let's say we need to render another input element to collect the user's address, we could do this the way we are rendering name or email or channel but we want to learn the render props way.

let's start with the initial values, we're going to add a property called address

const initialValues = {
name: 'Vishwas',
email:'',
channel:'',
comments: '',
address: ''
}

let's add the JSX

<div className='form-control'>
  <label htmlFor='address'>Address</label>
  <Field name = 'address'/>
</div>

Now this address field would work perfectly fine as it is but we definitely want the render props way

With the render props pattern we us a function as children to the component. So now our Field component will now have opening and closing tags
<Field></Field>

NExt as children we pass in a function, and this will be an arrow function. The arrow function will return JSX which in our case is an input element for the user to enter their address

<Field>
  {
    () => {
      return <input id='address' />
    }
  }
</Field>

but as it stands this input element is not hooked into formik in any way. This is where the props for this arrow function comes into picture.

<Field>
  {
    (props) => {
      console.log('Render props',props)
      return <input id='address' />
    }
  }
</Field>

We can see that we have three properties: field, form and meta

Render props { field:{...}, form: {...}, meta: {...}}

if we expand further, the field contains name, value, onChange, onBlur everything formik requires to manage the state.

we then have the form object, this is basically the formik constant that the useFormik hook used to return, it has all the properties and helper methods to modify your form in any way required.

in fact we have seen the few things before: errors object, values object, touched object and so on. You can alsoo see some few helper methods: handleChange, handleSubmit, handleBlur and so on.

The third prop provided is the meta prop, this gives you information on whether the particular field has been visited or not, if there is an error or not and ofcourse the value. This is what can be used to render your error messages.

Out of the rhree props, field and meta are more concerned at an individual field level. let's see how to make use of them with our address field.

let's begin by destructuring the render props

<Field>
  {
    (props) => {
      const {field, form, meta} = props
      console.log('Render props',props)
      return <input id='address' />
    }
  }
</Field>

To hook the input with formik we need to spread the field props

<Field>
  {
    (props) => {
      const {field, form, meta} = props
      console.log('Render props',props)
      return <input id='address' {...field} />
    }
  }
</Field>

This will take care of the name, value, handleChange, handleBlur

Next, we can use meta prop to render an error if there was one. Right now we dont have the validation rule, but let's see how to render error anyway.

<Field>
  {
    (props) => {
      const {field, form, meta} = props
      console.log('Render props',props)
      return (
        <div>
          <input type='text' id='address' {...field} />
          {meta.touched && meta.error ? <div>{meta.error}</div>:null}
        </div>
      )
    }
  }
</Field>

this is pretty much what the ErrorMessage component also does.

When you want to use custom components in your form and you want them to be hooked into formik this pattern will definitely come in handy

## ErrorMessage Revisited

if you can recollect the ErrorMessage component accepts a name prop and renders the error message for that particular field if the field has been visited and an error message exist for that field.

if you inspect the error message, you can see that it is just a text node, it's not trapped inside any HTML element let's see how to change that.

To inform the ErrorMessage component to wrap the error message with an HTML element, we need to use the component prop. let's set component prop equal to div.

<ErrorMessage name='name' component='div'/>

if you take a look at the browser, you can see that the error message is now wrapped within a div tag. so you can set it to any HTML element you want to.

Not only that you can even set it to a custom react component.

At the moment the error message is not in the red color, let's create a component that renders it's text in red color and then pass that component as the component prop of the ErrorMessage component.

create a new component file in components folder of your project directory

/components/TextError.js

import React from 'react

function TextError(props){
return (

<div className='error'>
{props.children}
</div>
)
}

<div>
  <label htmlFor='name'>Name</label>
  <Field type='text' id='name' name='name'/>
  <ErrorMessage name='name' component={TextError}>
</div>

An Alternative to this is to use render props pattern, so let's do that to the email field.

<div className='form-control'>
  <label htmlFor='email'>Email</label>
  <Field type='email' id='email' name='email'>
  <ErrorMessage name='email'>
  {
    (errorMsg) => {
        return <div className='error'>{errorMsg}</div>
    }

}
</ErrorMessage>

</div>

This function gets the error message as the prop

## Nested Objects

let's talk about nested objects in formik.

If you take a look at the youtube form currently, we can understand from the initialValues object that there are five fields. And we track our form values in the same structure as these key value pairs.

But sometimes you might want to group together some data into its own seperate object. The reason could be that API accepts the data in such a matter or the database stores the data in particular fashion. Whatever might be the reason we want to group together some of the fields in our form. For that we can make use of the nested objects.

let's understand with an example.

Suppose our youtube form should also collect information about the social presence that the user has. So the form will ask the information about fb and twitter profiles. Since they are related we want them to be grouped and stored as a nested object. let's see how to achieve that in two simple stpes.

First step, on the initialValues object, we're going to specify a property called 'social' and this is going to be equal to an object.

const initialValues = {
name: 'Vishwas',
channel: '',
comments: '',
address: '',
social: {
facebook: '',
twitter: ''
}
}

Second step: let's add the form fields in our JSX

<div className='form-control'>
  <label htmlFor='facebook'>Facebook profile</label>
  <Field type='text' id='facebook' name='social.facebook'/>
  //this time name is equal to social.facebook, this is important because we're dealing with nested objects.
</div>

<div className='form-control'>
  <label htmlFor='twitter'>Twitter profile</label>
  <Field type='text' id='twitter' name='social.twitter'/> 
</div>

## Arrays

let's take a look at managing the field state as an array.

let's say we need to collect the user's phone number, we want to collect their primary and secondary phone number. But while storing the data we dont really need any clearer distinction, we just want them to be stored as an array of a phone numbers under the same label, let's see how to do that.

first step: we need to add a property to our initialValues object.

const initialValues = {
name: 'Vishwas',
email: '',
channel: '',
comments: '',
address: '',
social: {
facebook: '',
twitter: ''
}
phoneNumbers: ['', '']
}

second step: add the JSX

<div className='form-control'>
  <label htmlFor='primaryPh'>Primary phone number</label>
  <Field type='text' id='primaryPh' name='phoneNumbers[0]'/>
</div>

<div className='form-control'>
  <label htmlFor='secondaryPh'>Secondary phone number</label>
  <Field type='text' id='secondaryPh' name='phoneNumbers[1]'/>
</div>

## FieldArray component

We're going to take a look at another component that the formik library provides, which is the FieldArray component.

It is a component that helps with a common scenario, namely dynamic form controls.

You can say that FieldArray component is meant to help you with common array or list manipulations. Let's understand how this component works with an example.

We learnt how to manage the state of a form field as an array.
In our initialValues object, we added a property called phone Numbers and assigned an array with two empty strings. In JSX we had the field component for each of those indexes, phoneNumbers[0] and phoneNumbers[1]

This is fine to collect two phone numbers from the users. Typically though collecting multiple phone numbers or multiple addresses is handled through dynamic form controls. What we mean by that is to begin with we only render one field for the user to enter their phone number, we then give them the option to add or provide more numbers if they wish to. the list of phone numbers will be managed as an array in our form state.

let's see how to implement the dynamic form control to collect the user's phone numbers using the FieldArray component.

first step: import FieldArray

import {Formik, Form, Field, ErrorMessage, FieldArray} from 'formik'

second step: we need to add a property to our initialValues object.

const initialValues = {
name: 'Vishwas',
email: '',
channel: '',
comments: '',
address: '',
social: {
facebook: '',
twitter: ''
},
phoneNumbers: ['', ''],
phNumbers: ['']
}

the value is going to be an array with one empty string, remember we start off by asking just one phone number, so only one value to begin with.

step3: we need to add the JSX.

step 3.1: Add the div tag and the label

<div className='form-control'>
  <label>List of phone numbers</label>
</div>

step 3.2: if you take a look at the previous form fields, after the label we have the Field components.

This time though since we're working with the list of form fields that are dynamic we'll use the FieldArray component. To this component we have to specify the name prop just like the Field component

<div className='form-control'>
  <label>List of phone numbers</label>
  <FieldArray name='phNumbers'>
  
  </FieldArray>
</div>

step 3.3 : to be in control of this dynamic form control, we need to use the render props pattern for this FieldArray component. Now this is similar to the render props pattern that the field components also provides which we have a look at for the address form field in one of the earlier sections.

If the term Render props is confusing to you just think of it as function as children, so in between the opening and closing tags of the field component, we have a function.

This function will automatically get some props and then using those props we will return some JSX. This is what we will do for our FieldArray Component as well.

function as children, so in between the opening and closing tags of the FieldArray component.

<div className='form-control'>
  <label>List of phone numbers</label>
  <FieldArray name='phNumbers'>
    {
      () => {
          //This function will automatically get some props that we can use in our JSX, let's call those props as fieldArrayProps
      }
    }
  </FieldArray>
</div>

<div className='form-control'>
  <label>List of phone numbers</label>
  <FieldArray name='phNumbers'>
    {
      (fieldArrayProps) => {
         console.log("FieldArrayprops",fieldArrayProps)
         return <div>Field Array</div>
      }
    }
  </FieldArray>
</div>

In the console if we expand the object, we can see that we have a few properties and a lot of functions or methods. There are methods like push , pop, remove, unshift which seem like common array manipulation operations. We also have the form object which is basically everything we need to manage entire form.

For our example we are intrested in just two functions and one property.

The two functions are push and remove

push we will use to add a new phone number

Remove we will use to remove an existing phone number, pretty straightforward.

form property on the other hand we need to go a few levels deep. If we expand the form object as well, we can see the values property and here is where we would find the values for our phNumbers fields. We need this particular property to render our JSX

<div className='form-control'>
  <label>List of phone numbers</label>
  <FieldArray name='phNumbers'>
    {
      (fieldArrayProps) => {
         console.log("FieldArrayprops",fieldArrayProps)
         const {push, remove, form} = fieldArrayProps
         const {values} = form
         const {phNumbers} = values
         return (
          <div>
            {phNumbers.map((phNumber, index) => {
              return (
                <div key={index}>
                  <Field name={`phNumbers[${index}]`}>
                  {index > 0 && (
                    <button type='button' onClick={() => remove(index)}>-</button>
                  )
                  }
                  <button type='button' onClick={() => push('')}>+</button>
                </div>
              )
            })}
          </div>
         )
      }
    }

  </FieldArray>
</div>

Now we have the field component, but what we also need are buttons for the user to add or remove fields dynamically

the click handlers for these buttons we are going to make use of remove and push functions

## FastField component

we're going to take a look at the last component that the formik library provides which is the FastField component and this component is mainly meant for performance optimization. According to the documentation it is probably worth considering this if your form has more than 30 fields or if there are fields with very complex validation requirements.

FastField is an optimezed version of the Field component which internally implements the the ship component lifecycle method to block all additional rerenders unless there are direct updates to the FastField form control itself.

so if you feel that a particular field is independent of all other fields in your form then you can use the FastField component

## when does validation run

what we know so far is that once the validation rules are run formik auto populates the formik.errors object with the error messages
