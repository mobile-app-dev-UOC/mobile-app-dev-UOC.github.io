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

> ![A login fragment](/images/05/login.png){:style="display:block; margin-left:auto; margin-right:auto"}
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



