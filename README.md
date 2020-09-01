# ScheduledNotificationUnity
With this plugin, it is possible to shedule an android notification, even if the application is closed.

## Copy files to the asset
First, you have to copy the Assets directory from this repo to the Assets directory in your unity project.

## Setup the scheduled notification class
You have to add this in your c# class to be able to call the scheduled notification:

```
    const string pluginName = "com.example.schedulednotification.ScheduledNotification";

    static AndroidJavaClass _pluginClass;
    static AndroidJavaObject _pluginInstance;


    public static AndroidJavaClass PluginClass
    {
        get
        {
            if(_pluginClass == null)
            {
                _pluginClass = new AndroidJavaClass(pluginName);
            }
            return _pluginClass;
        }
    }

    public static AndroidJavaObject PluginInstance
    {
        get
        {
            if(_pluginInstance == null)
            {
                _pluginInstance = PluginClass.CallStatic<AndroidJavaObject>("getInstance");
            }
            return _pluginInstance;
        }
    }

    // Start is called before the first frame update
    void Start()
    {
        AndroidJavaClass unityClass = new AndroidJavaClass("com.unity3d.player.UnityPlayer");
        AndroidJavaObject unityActivity = unityClass.GetStatic<AndroidJavaObject>("currentActivity");

        PluginInstance.Call("startMyService", unityActivity);
    }
```


## Call the notification
You can call the following methods:

### Create a notification
```
//Configure to launch 2 hours after quitting the application
DateTime date = DateTime.Now.AddHours(2);
PluginInstance.Call("scheduleNotification", date.ToString("s"), "title", "text content");
```
Note: The date is a string, so you have to push the good date format, like in this example.


### Delete planned notifications (if they are not already displayed)
```
PluginInstance.Call("deleteAllScheduleNotifications");
```

