# Explain
---
This library is built for gorgeous display of web contents within your application. Specifically, it helps
you launch the Chrome custom tabs when it's available, quote:
>As of Chrome 45, Chrome custom tabs is now generally available to all users of Chrome, 
>on all of Chrome's supported Android versions (Jellybean onwards).
[Chrome Custom Tabs](https://developer.chrome.com/multidevice/android/customtabs)

When Chrome custom tabs is not available, it helps you launch the FinestWebView which is
a beautiful and customizable Android Activity that shows web pages within an app.

You can say this library is a hybrid of the following two projects:
[custom-tabs-client](https://github.com/GoogleChrome/custom-tabs-client)
[FinestWebView-Android](https://github.com/TheFinestArtist/FinestWebView-Android)

#Add gradle dependency:
---
This library is available on both JCenter and MavenCentral, so you just need
add this to your build.gradle
```
dependencies {
    compile 'com.github.allenxuan:chrome-custom-tabs-utilities:0.2.0'
    }
```

#Basic Usage:
---
##Create CustomTabsIntent Builder
```java
CustomTabsIntent.Builder intentBuilder = new CustomTabsIntent.Builder();
```
##Customize toolbar color
```java
intentBuilder.setToolbarColor(color); //color is an int variable here.
```
or
```java
intentBuilder.setToolbarColor(Color.parseColor(color_string);  // color_string is a string of RGB color code here, e.g., #FF0000 for red.
```
##Set action button
```java
String shareLabel = getString(R.string.label_action_share);
Bitmap icon = BitmapFactory.decodeResource(getResources(),
                    android.R.drawable.ic_menu_share);     //Generally you do not want to decode bitmaps in the UI thread.
PendingIntent pendingIntent = createPendingIntent();
intentBuilder.setActionButton(icon, shareLabel, pendingIntent);
```
###createPendingIntent() can be this:
```java
private PendingIntent createPendingIntent() {
        Intent actionIntent = new Intent(
                this.getApplicationContext(), ShareBroadcastReceiver.class);
        return PendingIntent.getBroadcast(getApplicationContext(), 0, actionIntent, 0);
    }
```
or you may write your own createPendingIntent().
##Add menu item
```java
String menuItemTitle = getString(R.string.menu_item_title);
PendingIntent menuItemPendingIntent = createPendingIntent();
intentBuilder.addMenuItem(menuItemTitle, menuItemPendingIntent);
```
##Show web title
```java
intentBuilder.setShowTitle(true);
```
##Enable Url bar hiding
```java
intentBuilder.enableUrlBarHiding();
```
##Set close button icon
```java
 intentBuilder.setCloseButtonIcon(
                    BitmapFactory.decodeResource(getResources(), R.drawable.ic_arrow_back)); //Generally you do not want to decode bitmaps in the UI thread.
```
##Set animations
```java
intentBuilder.setStartAnimations(this, R.anim.slide_in_right, R.anim.slide_out_left);
intentBuilder.setExitAnimations(this, android.R.anim.slide_in_left,android.R.anim.slide_out_right);
```
##Launch Chrome custom tabs
```java
CustomTabActivityHelper.openCustomTab(
                this, intentBuilder.build(), Uri.parse(url), new WebviewFallback());
```

---
If Chrome custom tabs is not available, a basic FinestWebView is launched then. Building your own WebviewFallback
is recommended, like this
```java
public class MyOwnWebviewFallback implements CustomTabActivityHelper.CustomTabFallback {
    @Override
    public void openUri(Activity activity, Uri uri) {

        new FinestWebView.Builder(activity)   //Please refer to https://github.com/TheFinestArtist/FinestWebView-Android
            .toolbarScrollFlags(AppBarLayout.LayoutParams.SCROLL_FLAG_SCROLL | AppBarLayout.LayoutParams.SCROLL_FLAG_ENTER_ALWAYS)
            .gradientDivider(false)
            .dividerHeight(2)
            .toolbarColorRes(R.color.accent)
            .dividerColorRes(R.color.black_30)
            .iconDefaultColorRes(R.color.accent)
            .progressBarHeight(DipPixelHelper.getPixel(context, 3))
            .progressBarColorRes(R.color.accent)
            .backPressToClose(false)
            .setCustomAnimations(R.anim.slide_left_in, R.anim.hold, R.anim.hold, R.anim.slide_right_out)
            .show(uri.toString);
    }
}
```

#For more customizations and information, please refer to these documents and code:
[Chrome Custom Tabs](https://developer.chrome.com/multidevice/android/customtabs)
[custom-tabs-client](https://github.com/GoogleChrome/custom-tabs-client)
[FinestWebView-Android](https://github.com/TheFinestArtist/FinestWebView-Android)





