# Android-DefenseCrash
为了方便中国同学,提供了翻译文档如下:中文文档

What's this
This’s a Crash Defense library in Android to help you catch the Java exceptions which you don’t expected.

Integration
Step 1 Find the build.gradle file in your project and add the code into it as follow.
allprojects {
  repositories {
    //other mavens
    maven(){
      url "https://dl.bintray.com/xuuhaoo/maven"
    }
  }
}
Step 2 Find the build.gradle file in the module that you want to integration
dependencies {
    implementation 'com.tonystark.android:defense-crash:last.version’
}
Attentions: last.versionis a substitute word, the real version will be found in Download

Use
Initialize should be more earlier in application create, we suggest you put the init code in Application attachBaseContext(base:Context)

Sample
override fun attachBaseContext(base: Context) {
 super.attachBaseContext(base)
 DefenseCrash.initialize(this)
 ...
}
Install Defense library after initialize.

Sample
override fun attachBaseContext(base: Context) {
 super.attachBaseContext(base)
 DefenseCrash.initialize(this)
 DefenseCrash.install { thread, throwable, isSafeMode, isCrashInChoreographer ->
   //thread: The crash happened’s thread.
   //throwable: The Exception exactly is.
   //isSafeMode: If application is allready crashed and we saved it that is mean you are in safe mode,
   //that happens most of the time is your Main Looper is compromised by some errors and not going to normal,and we keep it runing that’s called safe mode.
   //isCrashInChoreographer: If crash happend in OnMeasure/OnLayout/OnDraw it will case screen blank or some view not draw successfully
   //If you got this true, we suggest you restart or finish the current Activity for good

   //You can throw some throwables here and if you do that, will case VM got this throwable and shutdown your process.
   //And you should do somting here such as:
   Log.i("Exceptionhandler",
     "thread:${thread.name} " +
     "exception:${throwable.message} " +
     "isCrashInChoreographer:$isCrashInChoreographer " +
     "isSafeMode:$isSafeMode")
   throwable.printStackTrace()
   FirebaseCrashlytics.getInstance().recordException(throwable);
 }
}
Uninstall Defense library if you don’t need this.

Sample
 DefenseCrash.unInstall()
