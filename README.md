#### for automating the `Click on connect Button` in linkedin

###### Gets all the buttons with this tag
```kotlin
var list = document.getElementsByClassName("artdeco-button__text");
```

###### loops over them skipping get premium and ads buttons
```kotlin
for(i = 7 ; i < list.length ; i ++ ){
var item = list[i];
// uncheck the click after making the colors.
// item.click(); 
// marks them with color to check if they are right
item.style.backgroundColor = "red";
}
```
