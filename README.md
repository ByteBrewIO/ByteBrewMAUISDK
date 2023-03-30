# ByteBrewMAUISDK

## Getting Started

1. Download these packages.
2. In Visual Studio, right click on your project and click "Manage NuGet Packages..."
3. Click the cog wheel / settings button in the top right, next to "Package source:".
4. Add a package source and point it to wherever you have your downloaded packages.
5. Set "Package source" to the one you just made.
6. Go to "Browse" and install the ByteBrewPlugin package. The other two will be included automatically as dependencies, but it's important they're in the folder too.

## Using the SDK

You can add a reference to ByteBrew in your project:
```using ByteBrewPlugin;```

And call functions from the SDK:
```ByteBrew.InitializeByteBrew("key", "secret", "engine");```

# Available Functions

### Below is a working example script that utilizes all available functions from the plugin.

```cs
using ByteBrewPlugin;

namespace ByteBrewMAUITestApp {
    public partial class MainPage : ContentPage {

        private string consoleLog = string.Empty;

        public MainPage() {
            InitializeComponent();
        }

        private void LogToConsole(string message) {
            consoleLog = consoleLog.Insert(0, "\n");
            consoleLog = consoleLog.Insert(0, message);
            ConsoleOutput.Text = consoleLog;
        }

        private void Initialize_Clicked(object sender, EventArgs e) {
#if ANDROID
            ByteBrew.InitializeByteBrew("ANDROID_GAME_KEY", "ANDROID_SECRET_KEY", "MAUI Android");
#elif IOS
            ByteBrew.InitializeByteBrew("iOS_GAME_KEY", "iOS_SECRET_KEY", "MAUI iOS");
#endif
            LogToConsole("Initializing ByteBrew...");
        }

        private void CheckInitialized_Clicked(object sender, EventArgs e) {
            LogToConsole("ByteBrew Initialized: " + ByteBrew.IsByteBrewInitialized.ToString());
        }

        private void StartPushNotifications_Clicked(object sender, EventArgs e) {
            ByteBrew.StartPushNotifications();
            LogToConsole("Starting push notifications...");
        }

        private void SetCustomDataString_Clicked(object sender, EventArgs e) {
            ByteBrew.SetCustomData("TestString", "Hello");
            LogToConsole("Setting custom data TestString: Hello");
        }

        private void SetCustomDataBool_Clicked(object sender, EventArgs e) {
            ByteBrew.SetCustomData("TestBool", true);
            LogToConsole("Setting custom data TestBool: true");
        }

        private void SetCustomDataInt_Clicked(object sender, EventArgs e) {
            ByteBrew.SetCustomData("TestInt", 10);
            LogToConsole("Setting custom data TestInt: 10");
        }

        private void SetCustomDataDouble_Clicked(object sender, EventArgs e) {
            ByteBrew.SetCustomData("TestDouble", 15.5d);
            LogToConsole("Setting custom data TestDouble: 15.5");
        }

        private void SendCustomEvent_Clicked(object sender, EventArgs e) {
            ByteBrew.NewCustomEvent("TestEvent");
            LogToConsole("Sending custom event TestEvent");
        }

        private void SendCustomEventString_Clicked(object sender, EventArgs e) {
            ByteBrew.NewCustomEvent("TestEventString", "Hello");
            LogToConsole("Sending custom event TestEventString: Hello");
        }

        private void SendCustomEventFloat_Clicked(object sender, EventArgs e) {
            ByteBrew.NewCustomEvent("TestEventFloat", 5.5f);
            LogToConsole("Sending custom event TestEventFloat: 5.5");
        }

        private void SendProgressionEvent_Clicked(object sender, EventArgs e) {
            ByteBrew.NewProgressionEvent(ByteBrew.ByteBrewProgressionType.Started, "main_env", "main_stage");
            LogToConsole("Sending progression event Started: main_env, main_stage");
        }

        private void SendProgressionEventString_Clicked(object sender, EventArgs e) {
            ByteBrew.NewProgressionEvent(ByteBrew.ByteBrewProgressionType.Started, "main_env", "main_stage", "hello");
            LogToConsole("Sending progression event Started: main_env, main_stage, hello");
        }

        private void SendProgressionEventFloat_Clicked(object sender, EventArgs e) {
            ByteBrew.NewProgressionEvent(ByteBrew.ByteBrewProgressionType.Started, "main_env", "main_stage", 8.5f);
            LogToConsole("Sending progression event Started: main_env, main_stage, 8.5");
        }

        private void SendAdEvent_Clicked(object sender, EventArgs e) {
            ByteBrew.TrackAdEvent(ByteBrew.ByteBrewAdType.Reward, "main_rv");
            LogToConsole("Sending ad event Reward, main_rv");
        }

        private void SendAdEventOneParam_Clicked(object sender, EventArgs e) {
            ByteBrew.TrackAdEvent(ByteBrew.ByteBrewAdType.Reward, "main_rv", "test_id");
            LogToConsole("Sending ad event Reward, main_rv, test_id");
        }

        private void SendAdEventTwoParam_Clicked(object sender, EventArgs e) {
            ByteBrew.TrackAdEvent(ByteBrew.ByteBrewAdType.Reward, "main_rv", "test_id", "test_provider");
            LogToConsole("Sending ad event Reward, main_rv, test_id, test_provider");
        }

        private void TrackIAPEvent_Clicked(object sender, EventArgs e) {
            ByteBrew.TrackInAppPurchaseEvent("MAUI_TEST", "USD", 4.99f, "test_product", "test_category");
            LogToConsole("Sending IAP event: 4.99 USD");
        }

        private void TrackGoogleIAP_Clicked(object sender, EventArgs e) {
#if ANDROID
            ByteBrew.TrackGoogleInAppPurchaseEvent("MAUI_TEST_GOOGLE", "USD", 8.99f, "test_google_product", "test_google_category", "receipt", "signature");
            LogToConsole("Sending Google IAP event: 8.99 USD");
#endif
        }

        private void ValidateGoogleIAP_Clicked(object sender, EventArgs e) {
#if ANDROID
            Action<Dictionary<string, object>> onValidated = new Action<Dictionary<string, object>>((result) => {
                MainThread.BeginInvokeOnMainThread(() => {
                    LogToConsole("message: " + result["message"].ToString());
                    LogToConsole("validation time: " + result["validationTime"].ToString());
                    LogToConsole("item id: " + result["itemID"].ToString());
                    LogToConsole("purchase processed: " + result["purchaseProcessed"].ToString());
                    LogToConsole("purchase valid: " + result["purchaseValid"].ToString());
                    LogToConsole("Checked IAP (2.99 USD, no receipt)");
                });
            });
            ByteBrew.ValidateGoogleInAppPurchaseEvent("MAUI_TEST_VAL", "USD", 2.99f, "test_valid_product", "test_valid_category", "receipt", "signature", onValidated);
            LogToConsole("Validating IAP event: 2.99 USD");
#endif
        }

        private void TrackiOSIAP_Clicked(object sender, EventArgs e)
        {
#if IOS
            ByteBrew.TrackiOSInAppPurchaseEvent("MAUI_TEST_GOOGLE", "USD", 8.99f, "test_google_product", "test_google_category", "receipt");
            LogToConsole("Sending Google IAP event: 8.99 USD");
#endif
        }

        private void ValidateiOSIAP_Clicked(object sender, EventArgs e)
        {
#if IOS
            Action<Dictionary<string, object>> onValidated = new Action<Dictionary<string, object>>((result) => {
                MainThread.BeginInvokeOnMainThread(() => {
                    LogToConsole("message: " + result["message"].ToString());
                    LogToConsole("validation time: " + result["timestamp"].ToString());
                    LogToConsole("item id: " + result["itemID"].ToString());
                    LogToConsole("purchase processed: " + result["purchaseProcessed"].ToString());
                    LogToConsole("purchase valid: " + result["isValid"].ToString());
                    LogToConsole("Checked IAP (2.99 USD, no receipt)");
                });
            });
            ByteBrew.ValidateiOSInAppPurchaseEvent("MAUI_TEST_VAL", "USD", 2.99f, "test_valid_product", "test_valid_category", "receipt", onValidated);
            LogToConsole("Validating IAP event: 2.99 USD");
#endif
        }

        private void LoadRemoteConfigs_Clicked(object sender, EventArgs e) {
            ByteBrew.LoadRemoteConfigs(new Action<bool>((x) => {
                MainThread.BeginInvokeOnMainThread(() => {
                    LogToConsole("Remote configs loaded: " + x.ToString());
                });
            }));
            LogToConsole("Loading remote configs...");
        }

        private void HasRemoteConfigs_Clicked(object sender, EventArgs e) {
            LogToConsole("Has remote configs: " + ByteBrew.HasRemoteConfigsBeenSet.ToString());
        }

        private void RetrieveRemoteConfig_Clicked(object sender, EventArgs e) {
            LogToConsole("Got remote config value Test: " + ByteBrew.RetrieveRemoteConfigValue("Test", "-1"));
        }

        private void RestartTracking_Clicked(object sender, EventArgs e) {
            ByteBrew.RestartTracking();
            LogToConsole("Restarting tracking...");
        }

        private void StopTracking_Clicked(object sender, EventArgs e) {
            ByteBrew.StopTracking();
            LogToConsole("Stopping tracking...");
        }

        private void GetUserID_Clicked(object sender, EventArgs e) {
            LogToConsole("User ID: " + ByteBrew.GetUserID);
        }

    }
}
```
