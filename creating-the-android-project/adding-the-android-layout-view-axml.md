# Adding the Android Layout View \(AXML\)

If you are familiar with `XML` or `XAML`, there is nothing significant differences between `AXML` and them. You need to learn some new syntax. For more details, you can view the doc: [http://developer.android.com/guide/topics/ui/declaring-layout.html](http://developer.android.com/guide/topics/ui/declaring-layout.html). Fortunately, VS 2017 provides us with Intellisense.

Now create an `AXML` item called `SplashScreen.axml` in the Resources/layout folder.

![](../.gitbook/assets/image%20%289%29.png)

Oftentimes the Design view crashes so open it with Source view. Replace the content with the following code:

```markup
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <TextView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Loading...." />
</LinearLayout>
```

This View is used to correspond to the `SplashScreen` created in the previous step, which will show a loading text when the app starts. Find the source code here: [https://github.com/MvvmCross/MvvmCross/blob/develop/ContentFiles/Android/SplashScreen.axml.pp](https://github.com/MvvmCross/MvvmCross/blob/develop/ContentFiles/Android/SplashScreen.axml.pp) .

Then create an `AXML` item called `FirstView.axml` in the same place. Replace the content with the following code:

```markup
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
              xmlns:local="http://schemas.android.com/apk/res-auto"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
  <TextView
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:text="Your Name" />
  <EditText
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    local:MvxBind="Text UserName" />
  <Button 
    android:layout_width="match_parent"
android:layout_height="wrap_content"
android:text="Click Me!"
    local:MvxBind="Click GetGreetingCommand"/>
  <TextView
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    local:MvxBind="Text Greeting" />
</LinearLayout>
```

We use an `EditText` control to accept the inputs from users and use a Button control to respond the click event, then show the message by the other `TextView` control.

