# networkinfo
判断网络是否连接（广播机制）

//广播接收器
    class NetworkChangeReceiver extends BroadcastReceiver {
        @Override
        public void onReceive(Context context, Intent intent) {
            //系统服务类,专门用于管理网络连接的。
            ConnectivityManager connectivityManager = (ConnectivityManager) getSystemService(Context.CONNECTIVITY_SERVICE);
            NetworkInfo networkInfo = connectivityManager.getActiveNetworkInfo();
            if (networkInfo != null && networkInfo.isAvailable()) {
                Toast.makeText(MainActivity.this, "网络已连接", Toast.LENGTH_SHORT).show();
            } else {
                Toast.makeText(MainActivity.this, "没有网络", Toast.LENGTH_SHORT).show();
            }
        }
    }


//1、广播接收器
    private IntentFilter intentFilter;
    private NetworkChangeReceiver networkChangeReceiver;
    
//2、在onCreate中动态注册
    @Override
    protected void onCreate(Bundle savedInstanceState) {
      ……
      intentFilter = new IntentFilter();
      intentFilter.addAction("android.net.conn.CONNECTIVITY_CHANGE");
      networkChangeReceiver = new NetworkChangeReceiver();
      registerReceiver(networkChangeReceiver, intentFilter);
    }
//3、在onDestroy中注销 
    @Override
    protected void onDestroy() {
        super.onDestroy();
        unregisterReceiver(networkChangeReceiver);
    }    
    
Android系统为了保证应用程序的安全性做了规定，如果程序需要访问一些系统的关键性信息，必须在配置文件中声明权限才可以，否则程序将会直接崩溃，比如这里查询系统的网络状态就是需要声明权限的。
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>

（以上内容摘自《第一行代码》）
    
    
    
