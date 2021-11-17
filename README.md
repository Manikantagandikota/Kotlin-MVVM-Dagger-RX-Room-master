# MVVM-Dagger-RX-Room
1.	Download Android Studio
   https://developer.android.com/studio/index.html

Click “Download Android Studio”, accept the terms and conditions, and proceed with the download.
The download might take a while, as it is about 700 MB. Run the installer after the download is finished.
2.	Install Android Studio
When running the installer, we recommend using the default values, as shown below. Once installed, Android Studio will run, and launch a setup wizard to download additional components.




3.	Run Android Studio Setup Wizard
When you first start Android Studio, it will offer to import any previous settings. Since this is likely your first installation, there are no settings to import. Again, just accept the default option and continue.

 
Next, select the UI theme. Choose whichever one you prefer -- this is simply a matter of preference. On the next screen, select the option to install an Android Virtual Device. You will need an actual Android device for this course, but this virtual device can be useful.


Accept the recommended RAM size for the emulator, and click “Next” to see a summary of the components to be downloaded. The additional components total about 1.8 GB in size.


When the download begins, the progress bar might appear to freeze, but do not worry -- the download is progressing. Once installation is complete, click “Finish”.

 
4.	Begin Developing
The Create New Project appears. The first option is to select the type of activity. Choose Empty, which is the default and click Next.
 

Configure your project is the next screen. It will ask for the name of the Application, Package Name, Project path, language, and API Level. Keep the defaults and click on Finish

 	 

Wait for it to finish. Once everything is downloaded and installed, the new project is created and you are taken to the Android workspace.
Create New Virtual Device
If you are starting the AVD Manager for the first time, you will see the following screen.  Else you will see the list of AVDs created.

 	 

In the left-hand panel displays a list of the Category of the device.  It includes TV, Phone, Wear & Tablet. Select the category.
The middle pane displays the list of devices available. Select one based on the requirement of your app. After this click on the Next button.
Note that phones with larger resolution Choose the pixels resolution according to your requirements as it will take huge RAM in large pixels resolution device. If your computer has low RAM, then prefer to choose less resolution device. Click Next to continue
 	 
Choose the system image based on the API level targeted by your App. The app won’t run if you choose lower API than the one target by the App,Select the image and click on Next to continue.
Here you can name your AVD, change startup orientation and few other hardware properties. Click on Show Advanced Settings to show more settings.
Click on Finish to create the AVD.
 
Under the action column, click on the   icon to run the AVD. The Android Emulator uses the AVD to mimic the device.  You can then use the control panel to manage the device. The Extend control button at the bottom gives you more options



What is RxJava?
RxJava is a JVM library for doing asynchronous and executing event-based programs by using observable sequences. It's main building blocks are triple O's, Operator, Observer, and Observables. And using them we perform asynchronous
tasks in our project. It makes multithreading very easy in our project. It helps us to decide on which thread we want to run the task.
What is RxAndroid?
RxAndroid is an extension of RxJava for Android which is used only in Android application.
RxAndroid introduced the Main Thread required for Android.
To work with the multithreading in Android, we will need the Looper and Handler for Main Thread execution.
RxAndroid provides AndroidSchedulers.mainThread() which returns a scheduler and that helps in performing the task on the main UI thread that is mainly used in the Android project. So, here AndroidSchedulers.mainThread()
 is used to provide us access to the main thread of the application to perform actions like updating the UI.
In Android, updating UI from background thread is technically not possible, so using AndroidSchedulers.mainThread() we can update anything on the main thread. Internally it utilizes the concept of Handler and Looper to 
perform the action on the main thread.

Use-Cases in Android
RxJava has the power of operators and as the saying goes by, "RxJava has an operator for almost everything".
Case 1:
Consider an example, where we want to do an API call and save it to some storage/file. It would be a long-running task and doing a long-running task on the main thread might lead to unexpected behaviour like App Not Responding.
So, to do the above-mentioned task we might think to use AsyncTask as our goto solution. But with Android R, AsyncTask is going to be deprecated, and then libraries like RxJava will be the solution for it.
Using RxJava over AsyncTask helps us to write less code. It provides better management of the code as using AsyncTask might make the code lengthy and hard to manage.



Case 2:
Consider a use-case where we might want to fetch user details from an API and from the user's ID which we got from the previous API we will call another API and fetch the user's friend list.
Doing it using AsyncTask we might have to do use multiple Asynctask and manage the results in was way where we want to combine all the AsyncTask to return the result as a single response.
But using RxJava we can use the power of zip operator to combine the result of multiple different API calls and return a single response.
Case 3:
Consider an example of doing an API call and getting a list of users and from that, we want only the data which matches the given current condition.
A general approach is to do the API call, and from the Collection, we can then filter the content of that specific user based on the condition and then return the data.
But using RxJava we can directly filter out the data while returning the API response by using the filter operator and we do all of this by doing the thread management.
These are a few use cases to understand RxJava for Android and why we need RxAndroid in our project.
Room 🔗 RxJava
Doing queries in Room with RxJava
Less boilerplate code, compile-time checked SQL queries, and on top of this, the power of asynchronous and observable queries — how does that sound? All of these are possible with Room, the persistence library from the Architecture Components. Async queries return LiveData or RxJava’s Maybe, Single or Flowable. The queries returning LiveData and Flowable are observable queries. They allow you to get automatic updates whenever the data changes to make sure your UI reflects the latest values from your database. If you’re already working with RxJava 2 in your app, then using Room together with Maybe, Single and Flowable will be a breeze.
Later edit: starting with `2.0.0-beta01`, Room also supports Observable
Later edit 2: starting with Room 2.1.0-alpha01, DAO methods annotated with @Insert, @Delete or @Update support Rx return types Completable, Single<T> and Maybe<T>.
Let’s consider the following UI: the user is able to see and edit their username. This, together with other info about the user, is saved in the database. Here’s how to insert, update, delete and query the user.
Insert
The Room integration with RxJava allows the following corresponding return types for insert:
Completable — where onComplete is called as soon as the insertion was done
Single<Long> or Maybe<Long> — where the value emitted on onSuccess is the row id of the item inserted
Single<List<Long>> or Maybe<List<Long>> — where the value emitted on onSuccess is the list of row ids of the items inserted
In case of error inserting the data, Completable, Single and Maybe will emit the exception in onError.
@Insert
Completable insert(User user);
// or
@Insert
Maybe<Long> insert(User user);
// or
@Insert
Single<Long> insert(User[] user);
// or
@Insert
Maybe<List<Long>> insert(User[] user);
// or
@Insert
Single<List<Long>> insert(User[] user);
Use the observeOn operator to specify the Scheduler on which an Observer will observe the Observable and subscribeOn to specify the Scheduler on which the Observable will operate.
Update/Delete
The Room integration with RxJava allows the following corresponding return types for update/delete:
Completable — where onComplete is called as soon as the update/delete was done
Single<Integer> or Maybe<Integer> — where the value emitted on onSuccess is the number of rows affected by update/delete
@Update
Completable update(User user);
// or
@Update
Single<Integer> update(User user);
// or
@Update
Single<Integer> updateAll(User[] user);
// or
@Delete
Single<Integer> deleteAll(User[] user);
// or
@Delete
Single<Integer> deleteAll(User[] user);
Use the observeOn operator to specify the Scheduler on which an Observer will observe the Observable and subscribeOn to specify the Scheduler on which the Observable will operate.
Query
To get the user from the database, we could write the following query in the data access object class (UserDao):
@Query(“SELECT * FROM Users WHERE id = :userId”)
User getUserById(String userId);
This approach has two disadvantages:
It is a blocking, synchronous call
We need to manually call this method every time our user data is modified
Room provides the option of observing the data in the database and performing asynchronous queries with the help of RxJava Maybe, Single and Flowable objects.
If you’re worried about threads, Room keeps you at ease and ensures that observable queries are done off the main thread. It’s up to you to decide on which thread the events are emitted downstream, by setting the Scheduler in the observeOn method.
For queries that return Maybe or Single, make sure you’re calling subscribeOn with a different Scheduler than AndroidSchedulers.mainThread().
To start using Room with RxJava 2, just add the following dependencies to your build.gradle file:
// RxJava support for Room
implementation “android.arch.persistence.room:rxjava2:1.0.0-alpha5”
// Testing support
androidTestImplementation “android.arch.core:core-testing:1.0.0-alpha5”
Maybe
@Query(“SELECT * FROM Users WHERE id = :userId”)
Maybe<User> getUserById(String userId);
Here’s what happens:
When there is no user in the database and the query returns no rows, Maybe will complete.
When there is a user in the database, Maybe will trigger onSuccess and it will complete.
If the user is updated after Maybe was completed, nothing happens.
Single
@Query(“SELECT * FROM Users WHERE id = :userId”)
Single<User> getUserById(String userId);
Here are some scenarios:
When there is no user in the database and the query returns no rows, Single will trigger onError(EmptyResultSetException.class)
When there is a user in the database, Single will trigger onSuccess.
If the user is updated after Single was completed, nothing happens.
Flowable/Observable
@Query(“SELECT * FROM Users WHERE id = :userId”)
Flowable<User> getUserById(String userId);
Here’s how the Flowable/Observable behaves:
When there is no user in the database and the query returns no rows, the Flowable will not emit, neither onNext, nor onError.
When there is a user in the database, the Flowable will trigger onNext.
Every time the user data is updated, the Flowable object will emit automatically, allowing you to update the UI based on the latest data.
Testing
Testing a query that returns a Maybe/Single/Flowable is not very different from testing its synchronous equivalent. In the UserDaoTest, we make sure that we use an in-memory database, since the information stored here is automatically cleared when the process is killed.
@RunWith(AndroidJUnit4.class)
public class UserDaoTest {
…
private UsersDatabase mDatabase;
@Before
public void initDb() throws Exception {
    mDatabase = Room.inMemoryDatabaseBuilder(
                     InstrumentationRegistry.getContext(),
                     UsersDatabase.class)
            // allowing main thread queries, just for testing
            .allowMainThreadQueries()
            .build();
}

@After
public void closeDb() throws Exception {
    mDatabase.close();
}
Add the InstantTaskExecutorRule rule to your test, to make sure that Room executes all the database operations instantly.
@Rule
public InstantTaskExecutorRule instantTaskExecutorRule = 
                                      new InstantTaskExecutorRule();
Let’s implement a test that subscribes to the emissions of getUserById and checks that indeed when the user was inserted, the correct data is emitted by the Flowable.
@Test
public void insertAndGetUserById() {
    // Given that we have a user in the data source
    mDatabase.userDao().insertUser(USER);
    // When subscribing to the emissions of user
    mDatabase.userDao()
             .getUserById(USER.getId())
             .test()
             // assertValue asserts that there was only one emission
             .assertValue(new Predicate<User>() {
                @Override
                public boolean test(User user) throws Exception {
                    // The emitted user is the expected one
                    return user.getId().equals(USER.getId()) &&
                      user.getUserName().equals(USER.getUserName());
                }
            });
}
Dagger 
1.Module : This is used on the class that does the work of constructing objects that’ll be eventually provided as dependencies
2.Provides : This is used on the methods inside the Module class that’ll return the object.
3.Inject : This is used upon a constructor, field or a method and indicates that dependency has been requested.
4.Component : The Module class doesn’t provide the dependency directly to the class that’s requesting it. For this, a Component interface is used that acts as a bridge between @Module and @Inject.
5.singleton : This indicates that only a single instance of the dependency object would be created.



1.Add dependency in build.gradle

implementation 'com.google.dagger:dagger:2.28.3'
annotationProcessor 'com.google.dagger:dagger-compiler:2.13'

2.Creating Modules

A) AppModule
   This module will provide the Context. You already know that we need Context everywhere, and in Retrofit as well we need the context object. 
And as the DI rule says we need an outsider to supply the objects, so here we will create this module that will give us the Context.

  @Module
         public class AppModule {
         private Application mApplication;
         public AppModule(Application mApplication) {
         this.mApplication = mApplication;
          }
         @Provides
         @Singleton
         Application provideApplication() {
         return mApplication;
         }
         }

B)NetWork Module

We need many objects in this Module. You might already know that for Retrofit fit we need a bunch of things.
We need Cache, Gson, OkHttpClient and the Retrofit itself. So we will define the providers for these objects here in this module.

@Module
public class NetworkModule {
String baseUrl;
public NetworkModule(String baseUrl) 
{
this.baseUrl = baseUrl;
}
@Provides
@Singleton
Gson provideGson() 
{
GsonBuilder gsonBuilder = new GsonBuilder();
gsonBuilder.setFieldNamingPolicy
 (FieldNamingPolicy.LOWER_CASE_WITH_UNDERSCORES);
  return gsonBuilder.create();
}
@Provides
@Singleton
Retrofit provideRetrofit(Gson gson) {
return new Retrofit.Builder()
.addConverterFactory(GsonConverterFactory.create(gson))
.baseUrl(baseUrl)
.build();
}






3.Building Component

 @Singleton
 @Component(modules = {AppModule.class, NetworkModule.class})
public interface AppComponent {
 void inject(MainActivity mainActivity);
 }

So you can see we will inject in the MainActivity. We also define 
all the modules  using  the @Component annotation as you can see in the code.
Now create a class named BaseApplication. In this class we will build the ApiComponent.

private AppComponent appComponent;
  @Override
  public void onCreate() {
    super.onCreate();
    appComponent = DaggerAppComponent.builder()
            .appModule(new AppModule(this))
            .networkModule(new NetworkModule(“URL”))
            .build();
  }

public AppComponent getNetworkComponent() {
    return appComponent;
}


we have our API component, but we need to instantiate this class when our application launches. 
And for this, we need to define it inside our App Manifest file. So open your AndroidManifest.
xml and modify it as shown below

<application
    android:name=".BaseApplication"
     .
     .
     .
</application>

Now finally, we can inject the dependency.
Come inside MainActivity.java and modify it as below

 ((BaseApplication) getApplication()).getNetworkComponent().inject(this);




