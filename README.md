#include <Servo.h>

int D5=5,D6=6,D10=10,D9=9;

int L,R; 

const int s0 = 2; 
const int s1 = 4; 
const int led = 7;
const int out = 8; 
const int s2 = 11; 
const int s3 = 13; 

int red = 0; 
int green = 0; 
int blue = 0;

int p;//储存颜色值
int q;//气球颜色
int t=0;

Servo myservo2;

void setup()
{ myservo2.attach(3); 
  Serial.begin(9600);
   pinMode(5,OUTPUT); 
   pinMode(6,OUTPUT);
   pinMode(10,OUTPUT);
   pinMode(9,OUTPUT);
   pinMode(s0, OUTPUT); 
   pinMode(s1, OUTPUT); 
   pinMode(s2, OUTPUT); 
   pinMode(s3, OUTPUT); 
   pinMode(out, INPUT); 
   pinMode(led, OUTPUT);
   digitalWrite(s0, HIGH); 
   digitalWrite(s1, HIGH); 
}

void color()
{
  digitalWrite(s2, LOW); 
  digitalWrite(s3, LOW); 
  red = pulseIn(out, digitalRead(out) == HIGH ? LOW : HIGH); 
  digitalWrite(s3, HIGH); 
  blue = pulseIn(out, digitalRead(out) == HIGH ? LOW : HIGH); 
  digitalWrite(s2, HIGH); 
  green = pulseIn(out, digitalRead(out) == HIGH ? LOW : HIGH); 
}

void color1() 
{ 
  stop1();
  digitalWrite(led, HIGH);
  delay(100);
  color();
  color();
  if (red < blue && red < green)              
  { 
    t = 1;
     Serial.println("red");
  }
  else if (green < red && green < blue)               
  { 
    t = 2;
     Serial.println("green");
  }
  else if (blue < red && blue < green)      
  { 
    t = 3;
     Serial.println("blue");
  } 
}
void qiqiu()
{
  stop2();
    while(1)
    {
    digitalWrite(led, HIGH);
    stop2();
        color ();
        color1();
         if(t==1||t==2||t==3)
          {p=t;
           break;
           }
    }
while(1)
{
    goo1(10000);
    stop3(50);
    color();
    color1();
    if(t==1||t==2||t==3)
    {
      q=t;
       Serial.println(q);
      if(q==p)
        {
       Serial.println("xiangtong");
       myservo2.write(0);
       delay(700);
       myservo2.write(85);
       delay(1);
       digitalWrite(led, LOW);
       break;
         }   
     }
     delay(50);
  }
}


void goo1(int time)
{
  while(1)
  {
        analogWrite(D5,251);
        analogWrite(D6,0);
        analogWrite(D10,0);
        analogWrite(D9,100);
  delay(1);
  time-=10;
   if(time <= 0)
  {break;}
  }
}

void stop1()
{
  analogWrite(D5,0);
  analogWrite(D6,0);      
  analogWrite(D10,0);
  analogWrite(D9,0);
  delay(1000);
}
void stop2()
{
  analogWrite(D5,0);
  analogWrite(D6,0);      
  analogWrite(D10,0);
  analogWrite(D9,0);
  delay(1);
}

void stop3(int time)
{
  analogWrite(5,0);
  analogWrite(6,0);      
  analogWrite(10,0);
  analogWrite(9,0);
  delay(time);
}

void straight()
{
        analogWrite(D5,255);
        analogWrite(D6,0);
        analogWrite(D10,0);
        analogWrite(D9,245);//120

}

void straightturnleft()
{
        analogWrite(D5,200);
        analogWrite(D6,0);
        analogWrite(D10,200);
        analogWrite(D9,0);

}
void straightturnright()
{
        analogWrite(D5,0);
        analogWrite(D6,235);
        analogWrite(D10,0);
        analogWrite(D9,235);
}
void papo()
{
        analogWrite(D5,225);
        analogWrite(D6,0);
        analogWrite(D10,0);
        analogWrite(D9,112);//115
  
}

void xunjixun(int time) 
{
while(1)
{ 
  delay(2);
  if(digitalRead(A0)==LOW && digitalRead(A1)==LOW)//均未检测到黑线
         {straight();
        // delay(80);
         }
  if(digitalRead(A0)==HIGH && digitalRead(A1)==LOW )//左侧没有,右侧检测到,所以要左转
         {straightturnleft();
         delay(23);
         }
  if(digitalRead(A0)==LOW && digitalRead(A1)==HIGH )//右侧没有,左侧检测到,所以右转
        {straightturnright();
        delay(18);
         }
  if(digitalRead(A0)==HIGH && digitalRead(A1)==HIGH )//全黑定时走
        {papo();
        delay(200);
         }
           Serial.println(time);
   time -= 1;
   if(time <= 0)
   break;
}
  }

void loop()
{    

       myservo2.write(55);
        xunjixun(2800);
        stop3(20); 
        while(1)
        {
          if(digitalRead(A0)==HIGH && digitalRead(A1)==LOW )
          {
        analogWrite(D5,255);
        analogWrite(D6,0);
        analogWrite(D10,155);
        analogWrite(D9,0);

}
if(digitalRead(A0)==LOW && digitalRead(A1)==HIGH )
{
        analogWrite(D5,0);
        analogWrite(D6,255);
        analogWrite(D10,0);
        analogWrite(D9,255);
}
  if(digitalRead(A0)==HIGH && digitalRead(A1)==HIGH )
  {
       stop2();
           if (digitalRead(A0)==HIGH && digitalRead(A1)==HIGH)
           {
              analogWrite(led,0);
              delay(100);
              qiqiu();  
               myservo2.write(55);
              xunjixun(5000);
              stop1();
              delay(100000);
           }
}
if(digitalRead(A0)==LOW && digitalRead(A1)==LOW)
{
        analogWrite(D5,255);
        analogWrite(D6,0);
        analogWrite(D10,0);
        analogWrite(D9,120);

}
        }
}
