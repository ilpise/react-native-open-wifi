## react-native-open-wifi

Connect to unsecured, open WiFi networks and get the status of the WiFi connection on the device.

Based on the work of [skierkowski/react-native-open-wifi-manager](https://github.com/skierkowski/react-native-wifi-manager)

**Android only**. Programatically connecting to WiFi networks on iOS is not possible. You should show instructions
telling your user to connect manually.

## Installation

First you need to install react-native-open-wifi:

```javascript
npm install react-native-open-wifi --save
```

* In `android/setting.gradle`

```gradle
...
include ':OpenWifi', ':app'
project(':OpenWifi').projectDir = new File(rootProject.projectDir, '../node_modules/react-native-open-wifi/android')
```

* In `android/app/build.gradle`

```gradle
...
dependencies {
    ...
    compile project(':OpenWifi')
}
```

* register module (in MainActivity.java or MainApplication.java)

On newer versions of React Native (0.18+):

```java
import com.skierkowski.OpenWifi.*;  // <--- import

public class MainActivity extends ReactActivity {
  ......
  
  /**
   * A list of packages used by the app. If the app uses additional views
   * or modules besides the default ones, add more packages here.
   */
    @Override
    protected List<ReactPackage> getPackages() {
      return Arrays.<ReactPackage>asList(
        new OpenWifi(), // <------ add here
        new MainReactPackage());
    }
}
```

On older versions of React Native:

```java
import com.skierkowski.OpenWifi.*;  // <--- import

public class MainActivity extends Activity implements DefaultHardwareBackBtnHandler {
  ......

  @Override
  protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    mReactRootView = new ReactRootView(this);

    mReactInstanceManager = ReactInstanceManager.builder()
      .setApplication(getApplication())
      .setBundleAssetName("index.android.bundle")
      .setJSMainModuleName("index.android")
      .addPackage(new MainReactPackage())
      .addPackage(new OpenWifi())              // <------ add here
      .setUseDeveloperSupport(BuildConfig.DEBUG)
      .setInitialLifecycleState(LifecycleState.RESUMED)
      .build();

    mReactRootView.startReactApplication(mReactInstanceManager, "ExampleRN", null);

    setContentView(mReactRootView);
  }

  ......

}
```

## Example

### Load module
```javascript
var OpenWifi = require('react-native-open-wifi');
```

### Connect to a new network (connect)
```javascript
// Attempts to connect to the network specified. This is an async call. Listen to connectionStatus for status
OpenWifi.connect(ssid,password);
```

### Get status of connection (status)
```javascript
// Possible States: 'CONNECTED', 'CONNECTING', 'DISCONNECTED', 'DISCONNECTING', 'SUSPENDED', 'UNKOWN'
// from: http://developer.android.com/reference/android/net/NetworkInfo.State.html
OpenWifi.status((status) => {
  if (status == 'CONNECTED') {
    this.navigateToActivation();
  }
});
```
