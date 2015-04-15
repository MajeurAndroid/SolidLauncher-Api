# SolidLauncher-Api
Solid Launcher public Api

## Set icon pack
Solid launcher supports icon pack based on Apex scheme.

To apply an icon pack, create intent with following action, and put icon pack's package name as extra.
Then you have to startActivityForResult if you want to know if icon pack was correctly applied, or startActivity if not.
If package name is not valid nor apex based, or if user is not allowed to set icon pack, user will not be able to confirm change and result will be Activity.RESULT_CANCELED.
If user accept and package exists and is valid, result will be Activity.RESULT_OK obviously.
If user decline, result will be Activity.RESULT_CANCELED.
```java
private static final String INTENT_ACTION = "com.majeur.launcher.intent.action.CHANGE_ICON_PACK";
private static final String EXTRA_PACKAGENAME = "com.majeur.launcher.intent.extra.ICON_PACK_PACKAGE";
private final int REQUEST_CHANGE_SOLID_ICON_CODE = 5677;
/* -----------//--------------- */
void changeIconPack(String packageName) {
      Intent intent = new Intent(INTENT_ACTION);
      intent.putExtra(EXTRA_PACKAGENAME, packageName);
      
      startActivityForResult(intent, REQUEST_CHANGE_SOLID_ICON_CODE);
      //Or
      startActivity(intent);
}
```

## Notification badge count
Solid launcher supports applications badge with notification count (such as unread email ..)
To update badge count of your app first you need to declare permission :
```xml
<uses-permission android:name="com.majeur.launcher.permission.UPDATE_BADGE" />
```

Then just send a broadcast when you want to update badge count, as following.
To remove badge, just send 0 value.
```java
private static final String INTENT_ACTION = "com.majeur.launcher.intent.action.UPDATE_BADGE";
private static final String EXTRA_PACKAGENAME = "com.majeur.launcher.intent.extra.BADGE_PACKAGE";
private static final String EXTRA_COUNT = "com.majeur.launcher.intent.extra.BADGE_COUNT";
private static final String EXTRA_CLASS = "com.majeur.launcher.intent.extra.BADGE_CLASS";
/* -----------//--------------- */
void updateNotificationCount(int count) {
      String packageName = getPackageName();
      String className = getPackageManager().getLaunchIntentForPackage(packageName)
            .getComponent().getClassName();
      
      Intent intent = new Intent(INTENT_ACTION);
      intent.putExtra(EXTRA_PACKAGENAME, packageName);
      intent.putExtra(EXTRA_CLASS, className);
      intent.putExtra(EXTRA_COUNT, count);
      sendBroadcast(intent);
}
```
