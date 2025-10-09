---
title: "Android Development Notes"
date: 2018-12-14 12:42:30
updated_at: 2020-05-26 12:59:07
tags:
- programming
---

stub

## Android boilerplates

### recyclerview 
- click listener, data update, swipe refresh, empty view etc)
- horizontal, vertical, staggeredgrid
- snaplayout (https://github.com/rubensousa/RecyclerViewSnap/)
### coordinatorlayout with basic custom child behavior 
### fragment adding in activity
- fragmentmanager add/replace framelayout id
- using baseactivity
- using newinstance (passing data)
- handling backstacks
- adding child fragment
### viewpager 
- fragment with tabs
- basic (pageradapter)
- basic (fragmentstatepageradapter)
### preferencefragment/activity 
### passing intents (generic, eventbus, parcelable, serializable)
### threading, asynctask, delay (simple show/hide animation) 
- http://stackoverflow.com/questions/3264383/difference-between-service-async-task-thread
- http://www.vogella.com/tutorials/AndroidBackgroundProcessing/article.html
- http://stackoverflow.com/questions/13954611/android-when-should-i-use-a-handler-and-when-should-i-use-a-thread
- http://stackoverflow.com/questions/9671546/asynctask-android-example
### Passing data through app 
### basic custom view (onDraw, removing, behavior)
### fab button (behavior) **X**
### enum **X**
### lifecycle (activity, fragment, adapter, androidannotations)
### persisting data (sharedpreference, savedinstancestate, db)
- https://inthecheesefactory.com/blog/fragment-state-saving-best-practices/en
### custom camera layout (camera2 api)
- https://github.com/googlesamples/android-Camera2Basic
- https://github.com/aimanbaharum/custom-camera-layout
### runtime permission
### optimizing bitmaps (when decodeFile) 
### observing data changes (contentobserver, asyntaskloader, service, localbroadcastmanager)
- https://www.websmithing.com/2011/02/01/how-to-update-the-ui-in-an-android-activity-using-data-from-a-background-service/
- http://code.tutsplus.com/tutorials/android-from-scratch-understanding-android-broadcasts--cms-27026
- http://stackoverflow.com/questions/14695537/android-update-activity-ui-from-service
### app deep linking
### notifications
- display notification
### push notification using FCM
### job schedulers
- Demo code http://pastebin.com/bytVqz87
- https://catinean.com/2014/10/19/smart-background-tasks-with-jobscheduler/
### adding menu item into fragment/activity
### prettify gradle.build
- https://github.com/bluelinelabs/Conductor/blob/develop/dependencies.gradle
- http://gmariotti.blogspot.com/2015/07/how-to-centralize-support-libraries.html
### Android common utils
- https://github.com/Blankj/AndroidUtilCode
- Logging utility
### DiffUtils
- Using DiffUtil in Android RecyclerView - https://medium.com/@iammert/using-diffutil-in-android-recyclerview-bdca8e4fbb00#.wxoffctpj
- DiffUtil is a must - https://medium.com/@nullthemall/diffutil-is-a-must-797502bc1149#.fzzzae5pa
### General knowledge for the curious
- How they build Android without Fragment/Activity - https://realm.io/news/sf-fabien-davos-modern-android-ditching-activities-fragments/
- Learn to context - https://possiblemobile.com/2013/06/context/
### Annotation over enums
- Java Enum and Android IntDef/StringDef annotation - https://noobcoderblog.wordpress.com/2015/04/12/java-enum-and-android-intdefstringdef-annotation/

## Vector drawables
 - http://blog.sqisland.com/2014/10/first-look-at-animated-vector-drawable.html
 - http://blog.nkdroidsolutions.com/android-vector-drawable-example-using-appcompat-v23-2/
 - http://code.tutsplus.com/articles/using-androids-vectordrawable-class--cms-23948
 - Drawing and animating on a canvas - http://www.alvinashcraft.com/2012/06/18/making-animations-over-the-canvas-excerpt-from-60-android-hacks/
 - https://blog.stylingandroid.com/animatedvectordrawable-bundles/

## Architecture

### MVP
 - http://engineering.remind.com/android-code-that-scales/
 - http://antonioleiva.com/mvp-android/
 - https://medium.com/@czyrux/presenter-surviving-orientation-changes-with-loaders-6da6d86ffbbf#.z6xozfyte
 - http://code.tutsplus.com/tutorials/how-to-adopt-model-view-presenter-on-android--cms-26206
 - http://www.tinmegali.com/en/model-view-presenter-android-part-1/
 - https://www.youtube.com/watch?v=Asc4hU1iSTU
 - https://www.novoda.com/blog/better-class-naming/
 - http://www.singhajit.com/mvp-in-android/
 - http://stackoverflow.com/questions/30704349/how-to-apply-mvp-pattern-to-android-project
 - Retrofit + RxJava: https://kmangutov.wordpress.com/2015/03/28/android-mvp-consuming-restful-apis/
 - http://hannesdorfmann.com/mosby/mvp/
 - https://upday.github.io/blog/model-view-presenter/
 - MVP/TDD - http://manabreak.eu/android/2016/10/26/mvp-tdd.html
 - MVP without Dagger/RxJava - https://android.jlelse.eu/avenging-android-mvp-23461aebe9b5#.qf5t6bzdj
 - https://medium.com/@m_mirhoseini/yet-another-mvp-article-part-1-lets-get-to-know-the-project-d3fd553b3e21#.48o68njtj
 - Refactoring an Android App - #1 - Intro to the MVP pattern - https://www.youtube.com/watch?v=ZWYOy8E4jWo
 - Structuring project example - https://github.com/futurice/freesound-android/tree/master/app/src/main/java/com/futurice/freesound
 - Another MVP project - https://github.com/athkalia/Just-Another-Android-App
 - Essential guide for designing your android app architecture mvp part 1 - https://blog.mindorks.com/essential-guide-for-designing-your-android-app-architecture-mvp-part-1-74efaf1cda40#.w5ly19ato ([source code](https://github.com/MindorksOpenSource/android-mvp-architecture))
 - MVP and RxJava - https://lorentzos.com/mvp-and-rxjava-9dfcbe79bf77#.dx17wrltc
 - Handling api calls using retrofit 2 and rxjava - https://medium.com/3xplore/handling-api-calls-using-retrofit-2-and-rxjava-2-1871c891b6ae
 - MVP vs MVVM - https://antonioleiva.com/mvvm-vs-mvp/

### MVVM

- twitter api to search tweets (MVVM, Rxjava 2, Retrofit 2, Databinding) - https://github.com/r7v/Tweetz/

### MVI

- MVI on Android - https://medium.com/@fnberta/mvi-on-android-20677f80df55

## Dependency Containers

### Dagger

- Dagger 2 + Retrofit + OkHttp + Gson - https://adityaladwa.wordpress.com/2016/05/09/dagger-2-with-retrofit-and-okhttp-and-gson/
- Dagger 2 + Realm test - https://github.com/niqdev/dagger-realm-test
- Dagger 2 + MVP - https://adityaladwa.wordpress.com/2016/05/11/dagger-2-and-mvp-architecture/

## Design pattern
 - https://github.com/dbacinski/Design-Patterns-In-Kotlin/
 - Repository pattern - https://adityaladwa.wordpress.com/2016/10/25/offline-first-reactive-android-apps-repository-pattern-mvp-dagger-2-rxjava-contentprovider/
 - Common design pattern on Android - https://www.raywenderlich.com/109843/common-design-patterns-for-android
 - MVC, MVP, MVVM Design Patterns with Godfrey Nolan - https://www.youtube.com/watch?v=JV63czrUpbI&feature=share
 - Design patterns for human - https://github.com/kamranahmedse/design-patterns-for-humans
 - Offline support - https://medium.com/@yonatanvlevin/offline-support-try-again-later-no-more-afc33eba79dc#.vlmdsz3ne
 - Syncing changes in Trello app - http://tech.trello.com/syncing-changes/
 - Building a sync adapter and using it on Android - http://josiassena.com/building-a-sync-adapter-and-using-it-on-android
 - Repository pattern, generally - https://medium.com/@krzychukosobudzki/repository-design-pattern-bc490b256006#.b8uer86v2
 - Anti-If: The missing patterns - https://code.joejag.com/2016/anti-if-the-missing-patterns.html

## Reactive Programming Android
 - RxJava + MVVM - https://www.youtube.com/watch?v=h25FDyGTLso
 - RxAndroid tutorial - https://www.raywenderlich.com/141980/rxandroid-tutorial
 - Reactive Apps with MVI - http://hannesdorfmann.com/android/mosby3-mvi-1
 - RxJava example - https://github.com/politrons/reactive
 - Essenstial RxJava Guide for Android dev - http://blog.jimbaca.com/essential-rxjava-guide-for-android-developers/

## Functional programming

 - Functional programming for android developers (part 2) - https://medium.freecodecamp.com/functional-programming-for-android-developers-part-2-5c0834669d1a#.ldji44tas

## Android Data Binding library
 - http://www.androidauthority.com/data-binding-in-android-709747/

## Realm

 - https://dzone.com/articles/realm-practical-use-in-android (+ best practice on design pattern)
 - Realm example with MVP pattern and clean architecture for Android - https://github.com/asanchezyu/RealmExample
 - Realm safe/deep integration - https://medium.com/@Viraj.Tank/realm-integration-in-android-best-practices-449919d25f2f#.8segcttyg
 - Realm in general - https://android.jlelse.eu/realm-its-all-about-the-choices-we-make-7c2fe380ecd8#.cm2sitlpy
 - Example of Realm with MVP and Dagger - https://www.thedroidsonroids.com/blog/android/example-realm-mvp-dagger/
 - RealmDb : Clean architecture in Android - http://stackoverflow.com/questions/37085259/realmdb-clean-architecture-in-android

## Android guidelines

 - https://github.com/ribot/android-guidelines
 - https://github.com/ribot/android-boilerplate
 - Applying Clean Architecture on Android - http://five.agency/android-architecture-part-4-applying-clean-architecture-on-android-hands-on/

## Offline-first apps

 - [Android Data Sync](http://www.dmytrodanylyk.com/android-data-sync-part-1/)
 - [Offline App Architecture](https://medium.com/@arunsasidharan/so-you-want-to-develop-for-the-next-billion-9eb072c26bc8#.klpm0joym)
 - [Using Couchbase](https://medium.com/@vlado.atanasov/very-cool-article-4ee8b43f184)
 - [Using Firebase](https://firebase.google.com/docs/database/android/offline-capabilities)

## Kotlin

 - Kotling lang - https://antonioleiva.com/variables-kotlin/
 - Living Android without Kotlin - https://hackernoon.com/living-android-without-kotlin-db7391a2b170#.hx713yiy6
 - Kotlin For Android: An Introduction - https://www.raywenderlich.com/132381/kotlin-for-android-an-introduction

## Open source Android resources

 - [Kickstarter](https://github.com/kickstarter/android-oss)
 - [DuckDuckGo](https://github.com/duckduckgo/android)
 - [Forecastie](https://github.com/martykan/forecastie)
 - [This repo](https://github.com/pcqpcq/open-source-android-apps/)
 - [Public APIs](https://github.com/abhishekbanthia/Public-APIs)
 - [twitter-birdhouse](https://github.com/juliabrezmen/twitter-birdhouse/) - Data sync implementation
 - [Workcation app](https://www.thedroidsonroids.com/blog/android/workcation-app-part-1-fragments-custom-transition/) (Map based app)
 - [MVP Sample](https://github.com/nglauber/mvp-sample) - MVP (Model View Presenter) pattern using Kotlin, RXJava, Retrofit, Dagger and DataBinding 
 - [Dagger2 MVP](https://github.com/erikcaffrey/Kata-Dagger2-MarioKart) - MVP pattern with Dagger2 and RxJava/Android
 - https://medium.com/@dmytrodanylyk/google-io-2017-useful-android-links-e756077f8895 - Google IO 2017 Useful Android links
 - [Cewlrency](https://paramsen.github.io/cewlrency-ux-overhaul/) - Currency converter app with RxJava

## Android Things

 - Writing your first Android Things - https://www.novoda.com/blog/writing-your-first-android-things-driver-p1

## Android Animation

- How to animate on Android - https://proandroiddev.com/how-to-animate-on-android-f8d227135613

## Android Tutorials

- Implement a login screen - https://medium.com/@vpaliy/do-you-dare-me-to-implement-this-login-screen-bf29b72d9e39

## TDD in Android Development
 - https://codelabs.developers.google.com/codelabs/android-testing/index.html
 - http://mayojava.github.io/android/android-ui-instrumentation-test-with-espresso/
 - https://realm.io/news/ellen-shapiro-android-testing/
 - https://medium.com/mobility/how-to-do-tdd-in-android-90f013d91d7f#.k52b4kvcb
 - http://trickyandroid.com/android-test-tricks-sharing-code-between-unit-ui-tests/
 - https://medium.com/android-testing-daily/writing-your-first-test-57ce98dc22c6#.5ntu91iop
 - [What makes android apps testable](http://www.philosophicalhacker.com/post/what-makes-android-apps-testable/)
 - https://github.com/Karumi/KataScreenshotAndroid
 - [Testing Sqlite on Android](https://medium.com/@MAFI8919/testing-sqlite-on-android-bfa0733e11e7#.upv8807o9)
 - [SnackBuilder lib with Test Suite](https://github.com/andrewlord1990/SnackbarBuilder/tree/master/snackbarbuilder/src/main/java/com/github/andrewlord1990/snackbarbuilder)
 -  Structuring Android app for testability - http://blog.marcinbudny.com/2015/04/structuring-android-app-for-testability.html
 - Continuous integration with android - https://medium.com/appaloosa-store-engineering/continuous-integration-with-android-14047096f3de#.47tdh580c
 - TDD your UI layer - http://www.donnfelker.com/tdd-your-ui-layer/
 - Mocking HTTP responses using Retrofit 2 - https://riggaroo.co.za/retrofit-2-mocking-http-responses/
 - TDD mindset story - http://panavtec.me/approaching-tdd-outside-android
 - Android Testing Guide - https://github.com/ravidsrk/android-testing-guide
 - Why Android Testing is so Hard: Historical Edition - https://www.philosophicalhacker.com/2015/04/17/why-android-unit-testing-is-so-hard-pt-1/
 - Story code - https://publicobject.com/2017/02/06/story-code
 - Building Android with BitBucket Pipelines- https://medium.com/@_tiwiz/building-android-with-bitbucket-pipelines-83cd62dbde33#.gebakdkn0
 - Android Travis CI config - https://github.com/niqdev/dagger-realm-test/blob/master/.travis.yml
 - Testing MVP using Espresso and Mockito - https://josiassena.com/testing-mvp-using-espresso-and-mockito/