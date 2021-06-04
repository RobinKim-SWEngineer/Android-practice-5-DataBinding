# Android-practice-5-DataBinding
Replacement of findViewById() with data binding object

When app has complex view hierarchies, `findViewById()` would slow down the app since it traverses them to find the desired view **during the runtime**.  And this happens whenever the view is recreated, while user is interacting with the application. 

Through a `Binding` object, we get a reference to the view and can access them directly. The traversing happens only once **at compile time** to get the view.

## Set up for data binding
 - Data binding has to be explicitly enabled in the Gradle file becuase it increases the compile time. Inside the android section we need to add the following :
   ```
   buildFeatures {
      dataBinding true
   }
   ```
   
 - The XML layout has to be wrapped by `<layout>` tag. Upon compiler seeing this tag, it will auto-generate the **binding class** whose name is derived as PascalCase from that of layout file. The namespace declarations must also be in the `<layout>` tag. 

## Creating a data binding object
 - Use `setContentView(..)` in `DataBindingUtil` class instead of auto-generated `setContentView(..)` method. This will set the activity's content view and also **return the binding object** to the given layout. 
 - Upon creation of binding object, **compiler generates the names** of the views **into the binding object** as camelCase from IDs of the views in the layout.

    ```
    private lateinit var binding: ActivityMainBinding
    
    override fun onCreate(savedInstanceState: Bundle?) {
        binding = DataBindingUtil.setContentView<ActivityMainBinding>(this, R.layout.activity_main)
        binding.doneButton.setOnClickListener { onButtonClicked() }
        ...
    }
    
    private fun onButtonClicked() {
        binding.apply {
            nicknameText.visibility = View.VISIBLE
            nicknameEdit.visibility = View.GONE
            doneButton.visibility = View.GONE
        }
        ...
    }
    ```

## Making data class available to a view 
 - Using data binding, we can make a data class directly available to the layout file and don't need to use the string resources.
 
    ```
    data class MyName(var name: String = "", var nickName: String = "")
    ```
    
 - Add the **data class reference** to the layout with <data> and <variable> tag as below. The `type` parameter should be the name of the data class with package name. Now we can reference the userName variable to set the text of view.

    ```
    <data>
        <variable
            name = "userName"
            type = "com.example.myapplication.MyName"/>
    <data/>
    ..
    <TextView
        ..
        android:text="@={userName.name}" />
    ```

 - In the activity after instantiation of a data class object, we assign it to userName through binding object.
    ```
    binding.userName = MyName("Robin", "Robinku")
    ```
    >> To have the updated UI make effect, call `binding.invalidateAll()`
    
    
