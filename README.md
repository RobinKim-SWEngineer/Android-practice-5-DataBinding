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
   
 - The XML layout has to be wrapped by `<layout>` tag. Upon compiler seeing this tag, it will auto-generate the **binding class** whose name is PascalCase from that of layout file. The namespace declarations for a layout must also be in the `<layout>` tag. 
