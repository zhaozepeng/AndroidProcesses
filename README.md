# AndroidProcesses
A small library to get the current running processes on Android
___

Why would I need this?
----------------------

As of Android 5.0, it has become increasingly difficult to get a list of running apps. [`getRunningTasks(int)`](http://developer.android.com/intl/zh-cn/reference/android/app/ActivityManager.html#getRunningTasks(int)) is now deprecated. Android 5.1.1+ killed [`getRunningAppProcesses()`](http://developer.android.com/intl/zh-cn/reference/android/app/ActivityManager.html#getRunningAppProcesses()) (as of Android 5.1.1+ it only returns your app). The documentation hasn't changed and Google is ignoring requests to either update the documentation or restore the original implementation. 

Using [UsageStatsManager](https://developer.android.com/reference/android/app/usage/UsageStatsManager.html), it is possible to get a list of running apps. However, this requires the user to grant your application special permissions in Settings. It has been reported that some OEMs have removed this setting.

This library gets a list of running apps and doesn't require any permissions. See the [sample](https://github.com/jaredrummler/AndroidProcesses/blob/master/sample/src/main/java/com/jaredrummler/android/processes/sample/MainActivity.java) application for details. Download the sample [APK](https://github.com/jaredrummler/AndroidProcesses/blob/master/sample-apk/sample.apk?raw=true).

Download
--------

Download [the latest AAR](https://repo1.maven.org/maven2/com/jaredrummler/android-processes/1.0.1/android-processes-1.0.1.aar) or grab via Gradle:

```groovy
compile 'com.jaredrummler:android-processes:1.0.1'
```
or Maven:
```xml
<dependency>
  <groupId>com.jaredrummler</groupId>
  <artifactId>android-processes</artifactId>
  <version>1.0.1</version>
</dependency>
```

Examples
--------

**Get a list of [RunningAppProcessInfo](http://developer.android.com/reference/android/app/ActivityManager.RunningAppProcessInfo.html):**

```java
List<ActivityManager.RunningAppProcessInfo> appProcesses = ProcessManager.getRunningAppProcessInfo(context);
```

**Check if your app is in the foreground:**

```java
if (ProcessManager.isMyProcessInTheForeground()) {
  // do stuff
}
```

**Get running apps and some information about them:**

```java
List<AndroidAppProcess> processes = ProcessManager.getRunningAppProcesses();
for (AndroidAppProcess process : processes) {
  String processName = process.name;
  
  Stat stat = process.stat();
  int pid = stat.getPid();
  int parentProcessId = stat.ppid();
  long startTime = stat.stime();
  int policy = stat.policy();
  char state = stat.state();

  Statm statm = process.statm();
  long totalSizeOfProcess = statm.getSize();
  long residentSetSize = statm.getResidentSetSize();
}
```

