<h1 align=center>
Workshop - AndroidStudio(Java)
</h1>

# Summary
- [Install](#0.)
- [Install](#install)
- [Discovery](#discovery)
- [Install](#install)
- [First Part : Python Basis](#first-part--python-basis)
- [Second Part : Scraping Basis](#second-part--scraping-basis)
- [Third Part : Advanced Scraping](#third-part--advanced-scraping)


# 0. Install (Before the Workshop please !!!! *IONIS connection Approved*):

- Android Studio https://developer.android.com/studio or https://developer.android.com/studio#downloads

- sudo snap install android-studio --classic (How I use it on Fedora)

- you can install Android Studio using InteliJ tools

# 1. Application

- 1.1 Android Studio
    - 1.1.0 Open Android Studio
    - 1.1.1 Create Android Application
        - File -> New -> New Project
        - (Phone & Tablet) -> Empty Application
        - Project Info (set Language on Java, set Minimum sdk on a hight version)
        - Android App -> Edit/Create Configurations...
            - Module = Your project
            - Deploy = Default APK
            - Launch = Default Activity
        - Click on 'Apply' then on 'OK' (to make sure you have saved your changes)

    - 1.1.2 Device Manager -> (Find your *Android* phone (ReScan Device)) Next ->  Next -> (Restart ADB) -> Finish  (Should not work imediately)


- 1.2 Choose 1.2.1(Physical Android Phone) or 1.2.2(Virtual Android Phone)
    - 1.2.1 Your Android Phone (Iphone are not alowed..) 
        - Enable Developer Mode: Settings->About Phone->Software Information->Build number (Multiple click on the row)
        - (Optional) If your not redirected on Developper options, go back to Settings and swipe to the bottom, Developer Options is now there.
        - Click on it and allow 'USB debugging'
        - When you plug your phone with your computer your phone should write a message
            - check 'Allow each time' (if it's possible. it's to avoid problems of connection)
            - click on 'Allow'


    - 1.2.2 Create Virtual Android Phone
        - Click on Tools->Device Manager->Create Device
            - Choose a device (Phone is the best option for this workshop) and press 'Next'
            - Download and Select min API level 30 (I use api lvl30) SystemImage (it's like window.iso or fedora.iso) for your device

- 1.3 Android Application
    - 1.3.0 Open your new Application

    - 1.3.1 Browse your new Application ((View->Tool-Window->Project) to open your Project folder on the right side)        *Tips: on the right left corner click on the 'drop-down menu' and use the android View to sort files properly*
        - manifest/
            - Android Manifest
        - com.example.projectName/
            - MainActivity.java
        - res/
            - drawable/activity_main.xml
            - themes/
                - Themes.xml
                - Themes.xml(night)
        - Gradle Scripts/build.gradle(Module:*/app)

# 2. UI/View

- 2.1 Preset
    - 2.1.1 Color
        - add blue
        - add green
        - add red
        - add white
        - add black
        - add transparent

    - 2.1.2 Themes
        - remove actionbar in (value & night) themes
        - set statusbar to white in (value & night) themes
        - set windowLightSatusBar false or true (folowing the statusbar color if it's light/dark) it's use to see info
        - **(Extra)** set  your color theme to prettify your app (you can remove unused color (in color.xml) that are not listed above for clarity but if the color is used you will probably get warning to ask you to edit usage)

- 2.2 Activity (AuthActivity)
    - 2.2.0 Add onBackPressed() { moveTaskToBack(true); } <!-- handle -->
    - 2.2.1 Create a second activity name authActivity
    - 2.2.2 Right click on folder , click on New, and search 'empty activity' in Activty

    - 2.2.3 Use UserName Password
        - Add 2 editText for username & password
        - Add button on authActivity to switch to mainActivity
        - Add buttonListener to start new activity when username and password are good (hard code the username & password in your project..)

    - 2.2.4 Use Biometric (If you are on virtual device check the usage of biometric on setting before using this)
        - Add button2 on authActivity to switch to mainActivity
        - Add button2Listener to start new activity when username and password are good (use this https://developer.android.com/training/sign-in/biometric-auth#java)

    - 2.2.5 When you build and run your app the first activity shown is mainActivity and not authActivity you have to change it 
        - Manifest can do that

- 2.3 Fragment (MainActivity)
    - 2.3.0 Add onBackPressed() { moveTaskToBack(true); }

    - 2.3.1 Add ViewPager2

    - 2.3.2 create 3 Fragment named: LocationFragment, WeatherFragment, HistoryFragment (as you create activity but now it's fragment...)
        - rename "Hello Blank fragment" with the correct Fragment Name

    - 2.3.3 create class MainFragmentAdapter to handle fragment using 'LifeCycle' in your viewPager2 (MainFragmentAdapter)
        - new Class(Java)
        - extend FragmentStateAdapter
            - add Methods
            - add constructor with lyfecycle (the last choice)

    - 2.3.4 Connect your ViewPager2 With a FragmentStateAdapter
        - FragmentStateAdapter fragmentAdapter = new MainFragmentAdapter(getSupportFragmentManager(), getLifecycle());
        - ViewPager2 viewPager2 = findViewById(R.id.viewPager2);
        - viewPager2.setAdapter(fragmentAdapter);

    - 2.3.5 TabLayout (or DrawerLayout) 
        - create tabLayout on bottom of viewPager2
            - add Methods to sync toolbar with pagerView2
        - (Extra) create DrawerLayout and NavigationDrawer
            - rearrange your mainActivty
    - 2.3.6 Toolbar
        - Add menu/ folder in res/ folder
        - create toolbar on top of viewPager2
            - toolbar should use toolbar_menu.xml (create it) in your new folder
            - add two item: github, exit
            - in MainActivity edit import androidx.appcompat.widget.Toolbar;
            - supportActionBar(toolbar)
            - add onCreateOptionsMenu
                - getMenuInflater().inflate(R.menu.menu_toolbar, menu);
            - add onOptionsItemSelected
                - switch(item.getItemId())
                    - case R.id.github
                    - case R.id.exit

# 3.Volley

- 3.1 Request -  VolleyClass (Display)
    - 3.1.1 using RequestQueue & StringRequest you must retrieve your (1)"location data" and (2)"weather data" using metaweather API
            (https://www.metaweather.com/fr/).
    - 3.1.2 store the response in your sessionManager (the constructor of your VolleyClass should take as argument the Context)
    - 3.1.3 To handle asynchronous response create a list of Boolean in volleyClass to know when data have been retrieve !

# 4. Thread

- 4.1 MainActivity
    - 4.1.1 Create a thread in your MainActivity to loop during 30sec waiting for data. if your thread timeout you should go back to your AuthActivity
            https://developer.android.com/reference/java/lang/Thread#Thread()
        - if you want to edit a Ui component in a Thread, find a way that work
            example: view.post(() -> {}) ( https://stackoverflow.com/questions/13840007/what-exactly-does-the-post-method-do )

# 5. Session Manager

- 5.1.1 create a class SessionManager

- 5.2.2 you need to use SharedPreference to store all of needed stuff on your app phone permanently 
        (https://developer.android.com/reference/android/content/SharedPreferences).
- 5.2.3 create synchronized getter/setter to store (1)location.json-stringified (2)weather.json-stringified (Extra (3)history.json-stringified)

# 6. UI/Data

- 6.0 Call your volleyfunction in your MainActivity

- 6.1 Location ()
    - 6.1.1 Display Location, Longitude and Latitude (SessionManager->location)

- 6.2 Weather ()
    - 6.2.1 create tableRowView(Layout) with tableRow(View) in it, and add tableLayout in the WeatherFragment layout
    - 6.2.2 Inflate new tableRowView with tableRowView(Layout) (https://developer.android.com/reference/android/view/LayoutInflater)
    - 6.2.3 combine all
    - 6.2.4 add data in each tableRow (SessionManager->weather)
        - display:
            - Date (refactor date into that format Lundi 1 Avril 2022)
            -  (refactor date into that format Lundi 1 Avril 2022)

- 6.3 **(Extra)** History ()
    - 6.3.1 (SessionManager->history)
    - 6.3.2 Display an historic of 10 last day using https://www.metaweather.com/api/ and using the same process that you use above.
    - 6.3.3 **(Extra)** Recode your groundhog on a 4th fragment, and display the result using past weather data..

# 7. **(Extra)**

- 7.1 Icon
    - add Image as PNG/XML/SVG
    - Change Icon on manifest
    -  when you call your image, you should ignore and try to launch your app, even if the text is red.

- 7.2 Drawable
    - add your style to the app
        - drawable
            - color, corner, animated button

    - make the tableRow color match with weather or use image(using request on https://www.metaweather.com/api/)