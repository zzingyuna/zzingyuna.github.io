---
layout: post
---

# 안드로이드 두더지 잡기 게임

layout, activity_main.xml
```
<?xml version="1.0" encoding="utf-8"?>
<android.support.constraint.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <LinearLayout
        android:layout_width="368dp"
        android:layout_height="495dp"
        android:orientation="vertical"
        tools:layout_editor_absoluteX="8dp"
        tools:layout_editor_absoluteY="8dp">

        <TextView
            android:id="@+id/textView"
            android:layout_width="match_parent"
            android:layout_height="wrap_content" />

        <Button
            android:id="@+id/button"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="재시작" />

        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="30dp"
            android:orientation="horizontal">

            <TextView
                android:id="@+id/textView2"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_weight="1"
                android:text="Time:" />

            <TextView
                android:id="@+id/txtTime"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_weight="1"
                android:text="30" />
        </LinearLayout>

        <TableLayout
            android:layout_width="match_parent"
            android:layout_height="match_parent">

            <TableRow
                android:layout_width="match_parent"
                android:layout_height="match_parent" >

                <ImageButton
                    android:id="@+id/img1"
                    android:layout_width="50dp"
                    android:layout_height="80dp"
                    app:srcCompat="@drawable/ic_launcher_background" />

                <ImageButton
                    android:id="@+id/img2"
                    android:layout_width="50dp"
                    android:layout_height="80dp"
                    app:srcCompat="@drawable/ic_launcher_background" />

                <ImageButton
                    android:id="@+id/img3"
                    android:layout_width="50dp"
                    android:layout_height="80dp"
                    app:srcCompat="@drawable/ic_launcher_background" />

            </TableRow>

            <TableRow
                android:layout_width="match_parent"
                android:layout_height="match_parent" >

                <ImageButton
                    android:id="@+id/img4"
                    android:layout_width="match_parent"
                    android:layout_height="wrap_content"
                    app:srcCompat="@drawable/ic_launcher_background" />

                <ImageButton
                    android:id="@+id/img5"
                    android:layout_width="match_parent"
                    android:layout_height="wrap_content"
                    app:srcCompat="@drawable/ic_launcher_background" />

                <ImageButton
                    android:id="@+id/img6"
                    android:layout_width="match_parent"
                    android:layout_height="wrap_content"
                    app:srcCompat="@drawable/ic_launcher_background" />
            </TableRow>

            <TableRow
                android:layout_width="match_parent"
                android:layout_height="match_parent" >

                <ImageButton
                    android:id="@+id/img7"
                    android:layout_width="match_parent"
                    android:layout_height="wrap_content"
                    app:srcCompat="@drawable/ic_launcher_background" />

                <ImageButton
                    android:id="@+id/img8"
                    android:layout_width="match_parent"
                    android:layout_height="wrap_content"
                    app:srcCompat="@drawable/ic_launcher_background" />

                <ImageButton
                    android:id="@+id/img9"
                    android:layout_width="match_parent"
                    android:layout_height="wrap_content"
                    app:srcCompat="@drawable/ic_launcher_background" />
            </TableRow>

            <TableRow
                android:layout_width="match_parent"
                android:layout_height="match_parent" >

                <LinearLayout
                    android:layout_width="match_parent"
                    android:layout_height="match_parent"
                    android:orientation="horizontal">

                    <TextView
                        android:id="@+id/score"
                        android:layout_width="match_parent"
                        android:layout_height="43dp"
                        android:text="0"
                        android:textSize="30sp" />
                </LinearLayout>
            </TableRow>
        </TableLayout>

    </LinearLayout>
</android.support.constraint.ConstraintLayout>
```

MainActivity.java  
```
package com.example.yunak.game1;

import android.annotation.SuppressLint;
import android.os.Handler;
import android.os.Message;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.ImageButton;
import android.widget.TextView;
import android.widget.Toast;

public class MainActivity extends AppCompatActivity {

    int score = 0;
    int time = 5;
    int[] imgValue = new int[9];
    ImageButton[] imageModel = new ImageButton[9];

    private Handler mHandler =null;

    @SuppressLint("HandlerLeak")
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        final TextView txtTime = (TextView)findViewById(R.id.txtTime);
        final TextView txtScore = (TextView)findViewById(R.id.score);

        imageModel[0] = (ImageButton)findViewById(R.id.img1);
        imageModel[1] = (ImageButton)findViewById(R.id.img2);
        imageModel[2] = (ImageButton)findViewById(R.id.img3);
        imageModel[3] = (ImageButton)findViewById(R.id.img4);
        imageModel[4] = (ImageButton)findViewById(R.id.img5);
        imageModel[5] = (ImageButton)findViewById(R.id.img6);
        imageModel[6] = (ImageButton)findViewById(R.id.img7);
        imageModel[7] = (ImageButton)findViewById(R.id.img8);
        imageModel[8] = (ImageButton)findViewById(R.id.img9);

        Button btn = (Button)findViewById(R.id.button);
        btn.setOnClickListener(new View.OnClickListener(){
            public void onClick(View v){
                time = 30;
                mHandler.sendEmptyMessageDelayed(0, 1000);
            }
        });

        for (int i=0; i<9; i++){
            final int arrayI = i;
            imageModel[i].setOnClickListener(new View.OnClickListener(){
                @Override
                public void onClick(View v) {
                    imageModel[arrayI].setImageResource(R.drawable.ic_launcher_background);
                    if (imgValue[arrayI]==1){
                        score++;
                        imgValue[arrayI] = 0;
                    }
                }
            });
        }

        mHandler = new Handler(){
            @Override
            public void handleMessage(Message msg){
                txtTime.setText(Integer.toString(time));
                txtScore.setText(Integer.toString(score));
                if (time==0){
                    Toast toast = Toast.makeText(getBaseContext(), "종료", Toast.LENGTH_SHORT);
                    toast.show();
                    return;
                }
                mHandler.sendEmptyMessageDelayed(0, 1000);
                time--;
                double rValue = Math.random();
                int selectedindex = (int)(rValue*10);
                if (selectedindex==9)
                    selectedindex=4;
                for (int i=0; i<9 ; i++){
                    imageModel[i].setImageResource(R.drawable.ic_launcher_background);
                    imgValue[i] = 0;
                }
                imageModel[selectedindex].setImageResource(R.drawable.viewicon);
                imgValue[selectedindex] = 1;
            }
        };

    }

}
```
