DYNAMIC FRAGMENT VÀ TRUYỀN DỮ LIỆU

- trong phần này ta sẽ thực hiện việc hiển thị 1 Dynamic Fragment (Fragment động) bằng code java lên Activity, đồng thời sử dụng Bundle truyền dữ liệu từ Activity lên Dynamic Fragmnet đó

___

THIẾT KẾ DYNAMIC FRAGMENT

- Dynamic Fragment này sẽ có 1 TextView dùng để hiển thị data được truyền từ Activity thông qua __Fragment.setAgrument(Bundle)__

- __fragment_a.xml__
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:background="#0ff"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <TextView
        android:id="@+id/textviewNoiDung"
        android:textSize="40sp"
        android:text="FRAGMENT A"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"/>

</LinearLayout>
```

- __FragmentA.java__
```java
package com.hienqp.fragmentbundle;

import android.app.Fragment;
import android.os.Bundle;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.TextView;

import androidx.annotation.Nullable;

public class FragmentA extends Fragment {

    TextView txtNoiDung;
    
    @Nullable
    @Override
    public View onCreateView(LayoutInflater inflater, @Nullable ViewGroup container, Bundle savedInstanceState) {
        View view = inflater.inflate(R.layout.fragment_a, container, false);

        txtNoiDung = view.findViewById(R.id.textviewNoiDung);

        return view;
    }
}
```

___

THIẾT KẾ LAYOUT CHO MAIN ACTIVITY

- đối với Layout của MainActivity ta sẽ sử dụng ViewGroup là LinearLayout với orientation là vertical
- layout Main Activity gồm các thành phần theo thứ tự sau
	- TextView
	- Button (dùng để add và truyền data cho Fragment)
- khi user click vào Button thì FragmentA sẽ được thêm tiếp tục vào sau Button, lúc này cả màn hình chính là nơi chứa Fragment, nên Layout của MainActivity cần có 1 id dùng cho câu lệnh add Fragment của FragmentTransaction

- __activity_main.xml__
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/myLinearLayout"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="#ff0"
    android:orientation="vertical"
    tools:context=".MainActivity">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Main Activity !"
        android:textSize="50sp" />

    <Button
        android:id="@+id/buttonAdd"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Add Fragment" />

</LinearLayout>
```

___

MAIN ACTIVITY - THÊM FRAGMENT

- __MainActivity.java__
```java
package com.hienqp.fragmentbundle;

import androidx.appcompat.app.AppCompatActivity;

import android.app.FragmentManager;
import android.app.FragmentTransaction;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;

public class MainActivity extends AppCompatActivity {

    Button btnAdd;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        btnAdd = findViewById(R.id.buttonAdd);

        FragmentManager fragmentManager = getFragmentManager();
        FragmentTransaction fragmentTransaction = fragmentManager.beginTransaction();

        btnAdd.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                FragmentA fragmentA = new FragmentA();
                fragmentTransaction.add(R.id.myLinearLayout, fragmentA);
                fragmentTransaction.commit();
            }
        });
    }
}
```

___

TRUYỀN DATA TỪ ACTIVITY SANG FRAGMENT

- ta có thể truyền bất kỳ kiểu data sang Fragment từ Activity, từ chuỗi, số, object, ...
- trong phần này ta sẽ thực hiện đơn giản là truyền 1 chuỗi khi click vào Button của Activity sẽ gọi Fragment đồng thời truyền 1 chuỗi để hiển thị lên Fragment
- ở MainActivity sau khi khởi tạo Fragment nhận data
```
FragmentA fragmentA = new FragmentA();
```
- ta đóng gói data (trong ví dụ này là chuỗi) vào trong Bundle
```
Bundle bundle = new Bundle();
bundle.putString("hotenSinhView", "Nguyễn Văn A");
```
- sau khi có Bundle, ta gọi method Fragment.setArguments(Bundle) từ Activity để gửi đi data
```
fragmentA.setArguments(bundle);
```
- như vậy ta đã hoàn tất việc đóng gói và truyền data từ Activity sang Fragment
```
        btnAdd.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                FragmentA fragmentA = new FragmentA();

                Bundle bundle = new Bundle();
                bundle.putString("hotenSinhView", "Nguyễn Văn A");
                fragmentA.setArguments(bundle);

                fragmentTransaction.add(R.id.myLinearLayout, fragmentA);
                fragmentTransaction.commit();
            }
        });
```

___

NHẬN DATA Ở FRAGMENT ĐƯỢC GỬI TỪ ACTIVITY        

- bên Activity đã thực hiện gửi 1 Bundle, thì bên Fragment nhận phải thực hiện nhận Bundle đó
- đối với nhận data từ Activity ở Fragment thì ta sử dụng __getArguments()__
```
Bundle bundle = getArguments();
```
- sau khi cos Bundle chứa data, ta kiểm tra Bundle có != null hay không, nếu != null thì tiến hành lấy ra data đã truyền tương ứng
```
        if (bundle != null) {
            txtNoiDung.setText(bundle.getString("hotenSinhView"));
        }
```
- như vậy ta đã hoàn tất việc nhận data được truyền từ Activity ở Fragment
```
    public View onCreateView(LayoutInflater inflater, @Nullable ViewGroup container, Bundle savedInstanceState) {
        View view = inflater.inflate(R.layout.fragment_a, container, false);

        txtNoiDung = view.findViewById(R.id.textviewNoiDung);

        Bundle bundle = getArguments();

        if (bundle != null) {
            txtNoiDung.setText(bundle.getString("hotenSinhView"));
        }

        return view;
    }
```         
