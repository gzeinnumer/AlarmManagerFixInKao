# AlarmManagerFixInKao

- Manifest
```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest>

    <uses-permission android:name="android.permission.WAKE_LOCK" />
    <uses-permission android:name="android.permission.SYSTEM_ALERT_WINDOW" />
    <uses-permission android:name="android.permission.FOREGROUND_SERVICE" />

    <application>

        ...

        <receiver
            android:name=".service.AlarmReceiver"
            android:directBootAware="true"
            android:enabled="true"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.BOOT_COMPLETED" />
                <action android:name="android.intent.action.LOCKED_BOOT_COMPLETED" />
            </intent-filter>
        </receiver>

        ...

    </application>

</manifest>
```

- AlarmReceiver
```java
public class AlarmReceiver extends BroadcastReceiver {

    private static final String TAG = "AlarmReceiver_";

    @Override
    public void onReceive(Context context, Intent intent) {
        Log.d(TAG, "onReceive: Alarm Terpanggil");
    }
}
```

- MainActivity
```java
public class MainActivity extends AppCompatActivity {

    private static final String TAG = "MainActivity_";

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        ...

        startAlarm();
    }

    private void startAlarm() {
        Calendar calendar = Calendar.getInstance();
        calendar.setTimeInMillis(System.currentTimeMillis());

        //TRIGGER Alaram Pada
        calendar.set(Calendar.HOUR_OF_DAY,23);
        //calendar.set(Calendar.MINUTE, 12);
        //calendar.set(Calendar.SECOND, 59);

        AlarmManager alarmMgr = (AlarmManager) getApplicationContext().getSystemService(Context.ALARM_SERVICE);
        Intent intent = new Intent(getApplicationContext(), AlarmReceiver.class);
        PendingIntent alarmIntent = PendingIntent.getBroadcast(getApplicationContext(), 0, intent, 0);
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.M) {
            if (alarmMgr!=null){
                alarmMgr.setInexactRepeating(AlarmManager.RTC_WAKEUP, calendar.getTimeInMillis(), AlarmManager.INTERVAL_DAY, alarmIntent);
            } else {
                Log.d(TAG, "startAlarm: alarmMgr null");
            }
        }
    }
}
```

---

```
Copyright 2020 M. Fadli Zein
```