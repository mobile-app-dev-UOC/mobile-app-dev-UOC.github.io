---
layout: default
title: 4.3. Fragments
parent: 4. Android app architecture
nav_order: 3
---

# 4.3. Fragments

Fragments were initially created only to allow a portion of the interface to be reused in various parts of the application. A fragment can manage interface events. 

Nowadays, a fragment is considered a bundle of application logic and user interface that provides certain functionality and can be reused within an application. 
A typical example of this is the login fragment, which provides a login interface and also checks the user’s credentials.

> ![A login fragment](/images/04/login.png){:style="display:block; margin-left:auto; margin-right:auto"}
> *A login fragment.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

Fragments are also intended to facilitate reuse during the development of different applications.

A fragment must be created within an activity or another fragment.

A fragment has a design part that is implemented as a normal layout. For example, in the case of the login fragment, we have our `fragment_login.xml`.

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
   xmlns:app="http://schemas.android.com/apk/res-auto"
   xmlns:tools="http://schemas.android.com/tools"
   android:id="@+id/container1"
   android:layout_width="match_parent"
   android:layout_height="match_parent"
   android:paddingLeft="@dimen/fragment_horizontal_margin"
   android:paddingTop="@dimen/fragment_vertical_margin"
   android:paddingRight="@dimen/fragment_horizontal_margin"
   android:paddingBottom="@dimen/fragment_vertical_margin"
   tools:context=".ui.login.LoginFragment">

   <Button
       android:id="@+id/login"
       android:layout_width="wrap_content"
       android:layout_height="wrap_content"
       android:layout_gravity="start"
       android:layout_marginStart="48dp"
       android:layout_marginTop="16dp"
       android:layout_marginEnd="48dp"
       android:layout_marginBottom="64dp"
       android:enabled="false"
       android:text="@string/action_sign_in"
       app:layout_constraintBottom_toBottomOf="parent"
       app:layout_constraintEnd_toEndOf="parent"
       app:layout_constraintStart_toStartOf="parent"
       app:layout_constraintTop_toBottomOf="@+id/password"
       app:layout_constraintVertical_bias="0.2" />

   <EditText
       android:id="@+id/password"
       android:layout_width="0dp"
       android:layout_height="wrap_content"
       android:layout_marginStart="24dp"
       android:layout_marginTop="8dp"
       android:layout_marginEnd="24dp"
       android:hint="@string/prompt_password"
       android:imeActionLabel="@string/action_sign_in_short"
       android:imeOptions="actionDone"
       android:inputType="textPassword"
       android:selectAllOnFocus="true"
       app:layout_constraintEnd_toEndOf="parent"
       app:layout_constraintStart_toStartOf="parent"
       app:layout_constraintTop_toBottomOf="@+id/username" />

   <EditText
       android:id="@+id/username"
       android:layout_width="0dp"
       android:layout_height="wrap_content"
       android:layout_marginStart="24dp"
       android:layout_marginTop="96dp"
       android:layout_marginEnd="24dp"
       android:hint="@string/prompt_email"
       android:inputType="textEmailAddress"
       android:selectAllOnFocus="true"
       app:layout_constraintEnd_toEndOf="parent"
       app:layout_constraintStart_toStartOf="parent"
       app:layout_constraintTop_toTopOf="parent" />

  </androidx.constraintlayout.widget.ConstraintLayout>
```

We have a Kotlin class called LoginFragment that inherits from the Fragment class:

```kotlin
class LoginFragment : Fragment() {

   public lateinit var loginViewModel: LoginViewModel
   private var _binding: FragmentLoginBinding? = null
}
```

In this class, we usually include a model that will contain the data handled by the fragment and a binding to connect it with the elements of its layout. The class will also include the application logic that will be encapsulated in this fragment.

In the callback `onCreateView`, we can now connect the binding to the layout.

```kotlin
override fun onCreateView(
   inflater: LayoutInflater,
   container: ViewGroup?,
   savedInstanceState: Bundle?
): View? {

   _binding = FragmentLoginBinding.inflate(inflater, container, false)
   return binding.root

}
```

In the `onViewCreated` callback we can now create the model and add the methods to observe it:

```kotlin
override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
   super.onViewCreated(view, savedInstanceState)


   loginViewModel = ViewModelProvider(this, LoginViewModelFactory())
       .get(LoginViewModel::class.java)

   loginViewModel.loginFormState.observe(viewLifecycleOwner,
   Observer { loginFormState ->
       if (loginFormState == null) {
           return@Observer
       }
       loginButton.isEnabled = loginFormState.isDataValid
       loginFormState.usernameError?.let {
           usernameEditText.error = getString(it)
       }
       loginFormState.passwordError?.let {
           passwordEditText.error = getString(it)
       }
   })

}
```

Many fragments include a static method implementing the Factory design pattern so that instances of a given fragment can be generated based on input parameters. This method could perform different fragment initializations based on those parameters.

## 4.3.1. Lifecycle of a fragment

The lifecycle of a fragment has an intimate relationship with the **supportFragmentManager**. The supportFragmentManager is a general control of all fragments that are being used in our application. We can create the instance of a fragment at any time but, from the point of view of the system, the fragment does not exist (or have any state) until we add it to the supportFragmentManager.

> ![Lifecycle of a fragment](/images/04/fragment-lifecycle.png){:style="display:block; margin-left:auto; margin-right:auto"}
> *Lifecycle of a fragment.*  
> Source: [Android Developers](https://developer.android.com/guide/fragments/lifecycle) License: [CC BY 2.5](http://creativecommons.org/licenses/by/2.5/)


A fragment passes through the following states:
- **Created:** The fragment has been created but we cannot guarantee that its view is available. An error occurs if we try to access elements of the view using the binding in the callback `onCreate`.
- **Started:** In this state, the fragment has already been created and in the callback `onStart()` we can access the elements of the view using binding. The callback `onPause()` is executed when the fragment is no longer linked to a view, for example, because it is replaced by another one. The fragment remains in the started state if the view becomes invisible using a display property.
- **Resumed**: The view will be displayed when the callback `onResume` is complete.
- **Destroyed:** A fragment becomes destroyed when it is removed from the supportFragmentManager. This does not mean that the Kotlin variable representing the fragment is destroyed. What it means is that for the application's general fragment controller, that fragment is no longer available in the fragment system.

## 4.3.2. Creating a fragment

We can insert a fragment statically by adding it to the XML layout of our activity. Using this approach, the fragment will not be available in the fragment stack that will be described in the next Section 4.3.3. To achieve this, we use the `fragment` label and in the `name` attribute we indicate the class of the fragment we want to instantiate.

```xml
<LinearLayout
   android:id="@+id/placeholder1"
   android:layout_width="match_parent"
   android:layout_height="match_parent"
   android:orientation="vertical">

  

<fragment android:name="com.uoc.fragments1.ui.login.LoginFragment"
   android:id="@+id/login_fragment1"
   android:layout_weight="1"
   android:layout_width="match_parent"
   android:layout_height="match_parent"
   app:layout_constraintRight_toRightOf="parent"
   app:layout_constraintLeft_toLeftOf="parent"
   tools:ignore="MissingConstraints" />

</LinearLayout>
```

We can also create a fragment dynamically in our code. We can create it directly using its constructor:


```kotlin
lateinit var f1: LoginFragment
f1 = LoginFragment()
```

We can also do this using the Factory design pattern if the fragment implements it:

```kotlin
var f2:ListFragment? = null
f2 = ListFragment.newInstance("", "")
```

Once the instance of the fragment is created, we must add it to the fragment system represented by the supportFragmentManager.

To achieve this goal, the layout of our activity must have a parent node within which we will add the fragment in question. This parent node has to be a ViewGroup. For example, linear and relative layouts inherit from ViewGroup. In the example below, we give this parent node the identifier `"placeholder1"`.

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
   xmlns:app="http://schemas.android.com/apk/res-auto"
   xmlns:tools="http://schemas.android.com/tools"
   android:layout_width="match_parent"
   android:layout_height="match_parent"
   tools:context=".MainActivity">

   <LinearLayout
       android:id="@+id/placeholder1"
       android:layout_width="match_parent"
       android:layout_height="match_parent"
       android:orientation="vertical">

 </LinearLayout>
</androidx.constraintlayout.widget.ConstraintLayout>
```

We then add the fragment using a supportFragmentManager transaction.

With `add`, we add the fragment to the parent node of our layout. Similarly, we can delete nodes using `remove`. The `setTransition` method indicates the type of visual effect we want to display when drawing the fragment.

The transaction concept is important because we can make and undo many changes at once, all within a single transaction.

## 4.3.3. Handling the fragment stack

In the previous Section, we have already added the fragment to the system and we have already viewed it. However, if the app user uses the Android "back" button, the fragments will not behave like Activities. That is, they will not disappear and appear in the reverse order, in the form of a stack. Sometimes we want some fragments to behave using the back button just like if they were activities.

To achieve this, we included the `addToBackStack` method in the transaction that adds the transaction to the stack.


```kotlin
supportFragmentManager.commit {
    replace(R.id.placeholder1, f2!!)
    setTransition(FragmentTransaction.TRANSIT_FRAGMENT_FADE)
    addToBackStack(null)
}
```

`addToBackStack` adds the entire transaction to the stack. If we have added two fragments in the transaction, backing up with the button will make both fragments disappear.

`setReorderingAllowed` is used to enable optimization of operations within the fragment transaction. Optimizations may involve eliminating operations or rearranging them. This can create the problem of losing control of the states our fragments go through and therefore what callbacks are being called.

As an example, let us consider a transaction where we add a fragment A, then we replace fragment A with a fragment B, and then fragment B with a fragment C. If we allow optimization, fragments A and B will never start and the corresponding callbacks will not run.

```kotlin
supportFragmentManager.commit {
   setReorderingAllowed(true)
   add(R.id.placeholder1, A)
   replace(R.id.placeholder1, B)
   replace(R.id.placeholder1, C)
   setTransition(FragmentTransaction.TRANSIT_FRAGMENT_FADE)
}
```

`replace` differs from `add` in that the fragment occupied by the placeholder goes into a destroyed state before creating the new one.

Transactions can be extracted from the stack programmatically, if for some reason we are not interested in something added to the stack being accessible simply by pressing the "back" button. To do this we use:

```kotlin
supportFragmentManager.popBackStack()
```

## 4.3.4. Communication from a fragment to an activity

### Calling an activity method directly

We can access the activity using the fragment `activity` property. In this case, we call the LoginOK activity method.

```kotlin
(activity as MainActivity?)!!.LoginOK(loginResult.success!!.userId)
```

The code above works but forces us to perform the strong cast to the MainActivity type. This is a problem because we are forcing our fragment to always connect to the same type of Activity.

### Using an interface

To solve the above problem and make the connection in an elegant way we can use an interface (declared in our fragment) that must be implemented by each activity that wants to receive communications from the fragment.

In our fragment, we declare the interface:

```kotlin
class LoginFragment : Fragment() {

   lateinit var callback: OnFragmentLoginInteractionListener

   interface OnFragmentLoginInteractionListener {
       fun onFragmentLoginInteraction(mensaje: String)
   }

}
```

We override the `onAttach` method to assign the activity to the callback property if the activity implements the `OnFragmentLoginInteractionListener` interface: 

```kotlin
override fun onAttach(context: Context) {
    super.onAttach(context)
    if (context is OnFragmentLoginInteractionListener) {
        callback = context as OnFragmentLoginInteractionListener
    }
}
```

In the activity statement we first indicate that we implement the interface and then implement the interface method or methods:

```kotlin
MainActivity class : AppCompatActivity(), LoginFragment.OnFragmentLoginInteractionListener {

    override fun onFragmentLoginInteraction(message: String) {
        Log.d("Debug",message)
    }

}

```

### Sharing the model

As we have already said, fragments can have a ViewModel. The idea is to create the ViewModel in the activity.

To achieve this, we declare the model shared with the fragment as a property of the activity.  In the `onCreate` method of the activity, we create the fragment and assign it to the shared model. And finally, we can observe the changes to the property of the model that we are interested in. Let us take a look at an example below:

```kotlin
class MainActivity : AppCompatActivity() {

    private val sharedViewModel: LoginViewModel by viewModels<LoginViewModel>() {
        LoginViewModelFactory()
    }

    lateinit var f1: LoginFragment

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        binding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(binding.root)

        f1 = LoginFragment()
        f1.loginViewModel = sharedViewModel

        sharedViewModel.add_description.observe(this, Observer<String> {
            Log.d("DEBUG",it) 
        })
    }
}
```

After all these preparations, assigning the property inside the fragment invokes the observer in the activity.

## 4.3.5. Communication from an activity to a fragment

### Calling a method of a fragment directly

Since the activity has created the fragment, we can have a property with the reference to the fragment and use it directly. 

```kotlin
f = LoginFragment()
f.Hello(10)
```

> **Learn more:**
>
> Be careful, the fragment may be in a state where from that method we cannot access the properties of your view.

### Using supportFragmentManager

The supportFragmentManager enables us to locate the fragment from the parent node identifier it was inserted into the layout.

```kotlin
var f:LoginFragment = supportFragmentManager.findFragmentById(R.id.placeholder1) as LoginFragment
f.Hello(10)
```

We can also use supportFragmentManager to locate a fragment from the bottom of another fragment.

These communication techniques are simple and work in practice. Notice that if we try to use a more elegant programming and create an interface in the activity for the fragments to implement, the compiler will warn us about a circular inheritance error: this cannot be done if the fragment is already exposing an interface and the activity implements it.

### Sharing the model

As we have seen in the previous section, we are sharing the model between the activity and the fragment. So the changes that the activity makes will also be observable by the fragment.

Code in Activity:

```kotlin
f1.loginViewModel.add_description.value = “hello”

```

Fragment Code:

```kotlin
loginViewModel.add_description.observe(this, Observer<String> {
   Log.d(“DEBUG”,it)
})

```

This has a problem: when the activity makes a change to a property of a model, if she is observing the changes to that property she also receives the call.

Code in Activity:

```
sharedViewModel.add_description.observe(this, Observer<String> {
  Log.d(“DEBUG”,it)
})
```
A simple solution to this is to separate the properties of the model belonging to the activity and those belonging to the fragment so that each class only observes their own properties.



