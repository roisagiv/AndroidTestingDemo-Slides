## Android Testing

---

## Roi Sagiv

<img src="Gett-vector.png" width="320" style="border:0px;box-shadow:none;margin:0px"/>
##### roisagiv@gmail.com
##### https://github.com/roisagiv/AndroidTestingDemo

---

## Agenda

-   Hello world
-   Real world examples
-   Espresso & Robotium
-   The new unit test plugin
-   Robolectic
-   JUnit 4


---

What is testing?
----------------

[Testing Fundamentals (developer.android.com)](https://developer.android.com/tools/testing/testing_android.html)
<!-- .element: class="fragment" data-fragment-index="1" -->

_"Provides an architecture and powerful tools that help you test every aspect of your application at every level from unit to framework."_
<!-- .element: class="fragment" data-fragment-index="1" -->

** In simple words: **
<!-- .element: class="fragment" data-fragment-index="2" -->

-   Run your app and interact with it, without touching your device/emulator.
    <!-- .element: class="fragment" data-fragment-index="3" -->

---

Let's see a demo
----------------

If you want to follow along
```bash
git checkout hello-world
# git clean is recommended
git clean -df
```

---

### Android Testing Framework

-   Based on JUnit 3
-   Each test method must start with `test`
-   Test class need to extend one of the `android.test.*TestCase`

---

`ActivityInstrumentationTestCase2<T>`

-   Launches new activity before each test
-   Finishes the activity after each test
-   `getActivity()`

---

`@UiThreadTest`

-   Runs the test method in the target app's main thread.
-   Allows you to interact with UI elements (TextView, EditText, ...)

---

### Instrumentation

-   Separated app, separated package name
-   AndroidManifest.xml with `<instrumentation />`
-   Loaded into the same process
-   Instrumentation provides access to target's `Context`
  -   Start activities
  -   Get resources
  -   Main thread
-   Instrumentation provides access to target's main thread

---

Few real world examples
-----------------------

-   *Display list of users*
  -   AsyncTaskLoader
  -   ["Random User Generator"](https://randomuser.me) for the backend
  -   RecyclerView

---

### LocalFileUsersAPIClient


```bash
git checkout LocalFileUsersAPIClientTest
```

-   Hide network calls behind an interface (UsersAPIClient)
-   One implementation uses local files
-   Another implementation actually performs HTTP requests

---

### ListUsersRecyclerViewAdapterTest

##### Part 1

```bash
git checkout ListUsersRecyclerViewAdapterTest-part1
```

-   Keep implementation simple
-   Focus on testing the interface methods:
  - `getItemCount()`
  - `onCreateViewHolder()`
  - `onBindViewHolder()`

---

### ListUsersRecyclerViewAdapterTest

##### Part 2

```bash
git checkout ListUsersRecyclerViewAdapterTest-part2
```

-   Feed the adapter with data as part of the test.
-   `RecyclerView.AdapterDataObserver`

---

### ListUsersRecyclerViewAdapterTest

##### Part 3

```bash
git checkout ListUsersRecyclerViewAdapterTest-part3
```

-   `ImageDownloader` interface
-   `PicassoImageDownloader`

---

### GetUsersLoaderTest

##### Part 1

```bash
git checkout GetUsersLoaderTest
```

-   Call `loadInBackground()` directly to avoid difficult async testing
-   `TestableUsersAPIClient` - verify interaction with components

---

### GetUsersLoaderTest

##### Part 2

```bash
git checkout GetUsersLoaderTest_mockito
```

-   [Mockito](http://mockito.org/)
-   Quick way to get `Testable***` implementations
-   Use version 1.9.5 & [dexmaker](https://github.com/crittercism/dexmaker)
-   Add to `setUp()` method: `System.setProperty("dexmaker.dexcache", getContext().getCacheDir().getPath());`

---

### UsersListActivityEspressoTest

```bash
git checkout UsersListActivityEspressoTest
```

-   [Espresso](https://developer.android.com/tools/testing-support-library/index.html)
  - UI interactions
  - Waits for `AsyncTasks` & `Loaders` to finish.

---

### UsersListActivityRobotiumTest

```bash
git checkout UsersListActivityRobotiumTest
```

-   [Robotium](https://github.com/RobotiumTech/robotium)
  - UI interactions
  - WebView / Mobile Web automation capabilities
  - Slower than Espresso and less feature rich

---

### Android "Unit Tests"

```bash
git checkout LocalFileUsersAPIClientTest_unittests
```

-   Runs on the JVM -\> no device is required
-   Special version of Android (mockable-android-XX.jar)
-   Useful for non-Android related code


---

## [Robolectric](http://robolectric.org/)

```
git checkout LocalFileUsersAPIClient_robolectric
```

```
git checkout ListUsersRecyclerViewAdapterTest_robolectric
```

-   Special "Android SDK", tailored to testing
-   Runs on the JVM -\> no device is required
-   A Lot of gotchas & quirks!

---

## [MockWebServer](https://github.com/square/okhttp/tree/master/mockwebserver)

```
git checkout HttpUsersAPIClientTest
```

-   Local HTTP Server, tailored to testing
-   Can simulate different network behaviors
  - HTTP codes (200, 404, 500, ...)
  - Timeouts
  - Throttling
  - Delays

---

## Thank you!
