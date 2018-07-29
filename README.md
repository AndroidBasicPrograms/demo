# demo


package com.example.jayhind.threaddemo;

import android.animation.ObjectAnimator;
import android.os.CountDownTimer;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.util.Log;
import android.view.View;
import android.view.animation.DecelerateInterpolator;
import android.widget.Button;
import android.widget.ProgressBar;
import android.widget.TextView;

public class MainActivity extends AppCompatActivity implements View.OnClickListener {

    TextView tv;
    ObjectAnimator animation;
    int curstat;
    Button btn,stop;
    CountDownTimer ct;
    int remainTime;
    String tag="fight";
    int round=3;
    int c=1;
    ProgressBar pd;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        tv=findViewById(R.id.timer);
        btn=findViewById(R.id.btn);
        stop=findViewById(R.id.stopbtn);
//      stop.setOnClickListener(this);
        btn.setOnClickListener(this);
        pd=findViewById(R.id.pd);
        startCounter(21000);
    }

    private void startanimation() {
        int totsecond=20000+10000*3-10000+1000;
        animation=ObjectAnimator.ofInt(pd, "progress",10000,0);
        animation.setDuration(10000); // 0.5 second
        animation.setInterpolator(new DecelerateInterpolator());
        animation.start();
    }

    private void startCounter(int millis) {
        ct=new CountDownTimer(millis, 1000) {
            public void onTick(long millisUntilFinished) {
                tv.setText(tag + millisUntilFinished / 1000);
                remainTime= (int) millisUntilFinished-1000;
            }
            public void onFinish() {
                if(c==round && tag.equals("fight"))
                {
                    ct.cancel();
                    btn.setText("");
                }else {
                    if (tag.equals("fight")) {
                        Log.e("l fight : ", "complete");
                        c++;
                        Log.e("l complete counter:", String.valueOf(c));
                        tag = "rest";
                        startCounter(10000);
                    } else if (tag.equals("rest"))
                    {
                        Log.e("l rest : ", "complete");
                        tag = "fight";
                        startCounter(20000);
                    }
                }
            }

        }.start();
    }

    @Override
    public void onClick(View view) {
        startanimation();

//        if(view.getId()==R.id.btn){
//            if (ct != null)
//                ct.cancel();
//            startCounter(remainTime);
//        }
//        }
//        else {
//            ct.cancel();
//        }
        if(btn.getText().equals("stop"))
        {
            animation.cancel();
            curstat= (int) animation.getCurrentPlayTime();
            ct.cancel();
            btn.setText("start");
        }
        else
        {
            btn.setText("stop");
            if (ct != null) {
                ct.cancel();
                animation.setCurrentPlayTime(curstat);
                animation.start();
                startCounter(remainTime);
            }
        }
    }
}
