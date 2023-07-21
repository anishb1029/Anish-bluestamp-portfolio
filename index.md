# 3 Joint Robotic Arm with BSE
The 3 joint robotic arm, despite how simple it is to the eye, was a tough build because of all the challenges that I had to face to end up the the result that I currently have. Broken motors, faulty codes, and unstable bases have led me to the belief that this project was one of the more challenging ones of the level because of the modifications needed to make the arm as healthy as possible. 

| **Engineer** | **School** | **Area of Interest** | **Grade** |
|:--:|:--:|:--:|:--:|
| Anish B | Tesla Stem High | Computer Science | Incoming Freshman

![image](https://github.com/anishb1029/Anish-bluestamp-portfolio/assets/138526777/6ae93b9b-c191-4830-bcc4-0240843eb237)

  
# Final Milestone

My final milestone shows that the modifications that I intended to make took place and even though some coding errors impeded my progress, I still found a way to move on. With help, I solidified my understanding on analog and digital signals, as well as potentiometers and pulse width modulations. In conclusion, this program helped my engineering skills so that I would be prepared for my STEM highschool. 

<iframe width="560" height="315" src="https://www.youtube.com/embed/f2MrpMWmb3k" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>



# Second Milestone

My second milestone shows that I have completed my assembly and found out some other simple errors, like disorganized cables and how my base was unsturdy (the screw was too loose). I planned to make a lazy susan type foundation for my robotic arm, but the parts hadn't arrived yet, so I was stuck with the finished assembly and sample code. My goal for the third milestone is to find out what I can do to the code to better my project, and to add the mods needed to strengthen the base. 

<iframe width="560" height="315" src="https://www.youtube.com/embed/Kgr2sArB4cU" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

# First Milestone

My first milestone was building almost all of the 3 joint robotic arm and uploading the sample code to the Arduino UNO. Although this was a task I could have completed within the first 3 days, a broken servo motor set me back quite a bit. On top of that, my robotic arm was experiencing an extremely unsturdy base made me think about modifications to strengthen that part of the project. Overall, I thought of this as an awesome beginning to my robotic arm. 

<iframe width="560" height="315" src="https://www.youtube.com/embed/kQThEUMxvi0" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

# Schematics 
![image](https://github.com/anishb1029/Anish-bluestamp-portfolio/assets/138526777/6347cf3b-645a-4892-86f8-542131181dff)

# Code

Rollarm.ino
#include <Servo.h> 

Servo Servo_0;
Servo Servo_1;
Servo Servo_2;
Servo Servo_3;

int SensVal[4] = {0}; 

int Joint0[50] = {0}; 
int Joint1[50] = {0};
int Joint2[50] = {0};
int Joint3[50] = {0};

int Dif0[50] = {0}; 
int Dif1[50] = {0};
int Dif2[50] = {0};
int Dif3[50] = {0};

int KeyValue = 0;
int Time = 0;
int M0 = 0, M1 = 0, M2 = 0, M3 = 0;

void setup() 
{
  Serial.begin(9600);
  
  Servo_0.attach(4);
  Servo_1.attach(5);
  Servo_2.attach(6);
  Servo_3.attach(7);

  pinMode(3, INPUT);

  ReadPot();
  Mapping0();

  M0 = SensVal[0];
  M1 = SensVal[1];
  M2 = SensVal[2];
  M3 = SensVal[3];
}

void loop() 
{
 //Print the data.
#ifdef DataPrint
  while (1)
  {
    ReadPot();
    Serial.print("SensVal[0]:");
    Serial.println(SensVal[0]);
    Serial.print("SensVal[1]:");
    Serial.println(SensVal[1]);
    Serial.print("SensVal[2]:");
    Serial.println(SensVal[2]);
    Serial.print("SensVal[3]:");
    Serial.println(SensVal[3]);
    delay(200);
  }
#endif

  static int Flag = 1;
  Button();

  if ((KeyValue < 10) && (KeyValue > 0))
  {
    KeyValue = 0;
    Record();
    Mapping1(); 

  else if (KeyValue > 10)
  {
    if (Flag == 1)
    {
      Flag = 0;
      Calculate();
    }
    Drive_init();
    delay(3000);
    for (int i = 1; i < Time; i++)
    {
      Drive_repeat(i);
      delay(500);
    }
  }

  else
  {
    ReadPot();
    Mapping0();
    
    if ((SensVal[0] - M0) >= 0)
    {
      for (; M0 <= SensVal[0]; M0++)
      {
        Servo_0.write(M0); delay(2);
      }
    }
    else
    {
      for (; M0 > SensVal[0]; M0--)
      {
        Servo_0.write(M0);  delay(2);
      }
    }
  
    if ((SensVal[1] - M1) >= 0)
    {
      for (; M1 <= SensVal[1]; M1++)
      {
        Servo_1.write(M1);  delay(2);
      }
    }
    else
    {
      for (; M1 > SensVal[1]; M1--)
      {
        Servo_1.write(M1);  delay(2);
      }
    }
 
    if ((SensVal[2] - M2) >= 0)
    {
      for (; M2 <= SensVal[2]; M2++)
      {
        Servo_2.write(M2);  delay(2);
      }
    }
    else
    {
      for (; M2 > SensVal[2]; M2--)
      {
        Servo_2.write(M2);  delay(2);
      }
    }
    
    if ((SensVal[3] - M3) >= 0)
    {
      for (; M3 <= SensVal[3]; M3++)
      {
        Servo_3.write(M3);  delay(2);
      }
    }
    else
    {
      for (; M3 > SensVal[3]; M3--)
      {
        Servo_3.write(M3);  delay(2);
      }
    }
 
    M0 = SensVal[0];
    M1 = SensVal[1];
    M2 = SensVal[2];
    M3 = SensVal[3];
    delay(10);
  }
}

Rollarm.ino sets up all the motors and potentiometers so that they are ready to function properly. 

Record.ino

void Record()
{
  Joint0[Time] = analogRead(A0);
  Joint1[Time] = analogRead(A1);
  Joint2[Time] = analogRead(A2);
  Joint3[Time] = analogRead(A3);
}

void ReadPot()
{
  SensVal[0] = 0;
  SensVal[1] = 0;
  SensVal[2] = 0;
  SensVal[3] = 0;
  
  SensVal[0] = analogRead(A0);
  SensVal[1] = analogRead(A1);
  SensVal[2] = analogRead(A2);
  SensVal[3] = analogRead(A3);
}

void Mapping0()
{
  SensVal[0] = map(SensVal[0], 0, 1023, 10, 170); 
  SensVal[1] = map(SensVal[1], 0, 1023, 10, 170);
  SensVal[2] = map(SensVal[2], 0, 1023, 10, 170); 
  SensVal[3] = map(SensVal[3], 0, 1023, 100, 180);
}

void Mapping1()
{
  Joint0[Time] = map(Joint0[Time], 0, 1024, 0, 170); 
  Joint1[Time] = map(Joint1[Time], 0, 1024, 0, 170); 
  Joint2[Time] = map(Joint2[Time], 0, 1024, 0, 170); 
  Joint3[Time] = map(Joint3[Time], 0, 1024, 100, 175); 
  Time++;
}

void Calculate()
{
  for (int i = 0; i < Time; i++)
  {
    if (i == 0)
    {
      Dif0[0] = Joint0[0] - Joint0[Time - 1];         
      Dif1[0] = Joint1[0] - Joint1[Time - 1];
      Dif2[0] = Joint2[0] - Joint2[Time - 1];
      Dif3[0] = Joint3[0] - Joint3[Time - 1];
    }
    else
    {
      Dif0[i] = Joint0[i] - Joint0[i - 1];
      Dif1[i] = Joint1[i] - Joint1[i - 1];
      Dif2[i] = Joint2[i] - Joint2[i - 1];
      Dif3[i] = Joint3[i] - Joint3[i - 1];
    }
  }
}

Record.ino records the values of all the potentiometers and assigns them an amount of degrees that act as their range of movement. 

# Bill of Materials

| **Part** | **Note** | **Price** | **Link** |
|:--:|:--:|:--:|:--:|
| M1.5 x 5 Self-tapping screw | Mostly used for screwing on the servo arms| $0.39 per screw, 10 screws | <a href="https://www.metricscrews.us/index.php?main_page=product_info&products_id=1044"> Link </a> |
| M2 x 8 Screw | Mostly used for screwing in the servo motors | $0.82 per screw, 10 screws | <a href="https://accu-components.com/us/cap-head-screws/3954-SSCF-M2-8-A4?google_shopping=1&c=2"> Link </a> |
| M2.5 x 12 Screw | Used for screwing in the Arduino UNO | $0.69 per screw, 6 screws | <a href="https://accu-components.com/us/cap-head-screws/3807-SSCF-M2-5-12-A2?google_shopping=1&c=2"> Link </a> |
| M3 x 10 | Used for screwing in plates | $0.59 per screw, 11 screws | <a href="https://accu-components.com/us/cap-head-screws/3819-SSCF-M3-10-A2?google_shopping=1&c=2"> Link </a> |
| M2 Self-locking Nut | Used for securing servo motors | $5.99 for 10 nuts | <a href="https://shop.mikadousa.com/04803-Self-locking-nut-M2-_p_1274.html"> Link </a> |
| M2.5 Nut | Used for securing the Arduino UNO | $0.46 per nut, 6 nuts | <a href="https://www.homedepot.com/b/Hardware-Fasteners-Nuts/M25/N-5yc1vZc2elZ1z0sfxi"> Link </a> |
| M3 Nut | Used for securing the plates | $0.06 per nut, 12 nuts | <a href="https://www.amazon.com/M3-0-5-Nylon-Insert-Stainless-100pcs/dp/B0BHQMP9R8/ref=sr_1_1_sspa?keywords=M3%2Bnuts&qid=1688671503&sr=8-1-spons&sp_csd=d2lkZ2V0TmFtZT1zcF9hdGY&th=1"> Link </a> |
| M3 Self-locking Nut | Used for securing smaller plates | $0.07 per nut, 4 nuts | <a href="https://www.amazon.com/100Pcs-Stainless-Self-Lock-Inserted-Clinching/dp/B075ZZW7VL"> Link </a> |
| M7 Thin Nut | Used for securing the potentiometers | $0.62 per nut, 4 nuts | <a href="https://www.homedepot.com/b/Hardware-Fasteners-Nuts/M7/N-5yc1vZc2elZ1z1bvfq"> Link </a> |
| Spring Washer | Used for securing one of the claws | $0.08 per washer, 3 washers | <a href="https://www.amazon.com/FASTENER-TREE-Spring-Washers-Stainless/dp/B09W892MTD/ref=sr_1_2_sspa?crid=1NZ0XK458T0A1&keywords=small%2BSpring%2BWashers&qid=1688676564&sprefix=small%2Bspring%2Bwashers%2Caps%2C145&sr=8-2-spons&sp_csd=d2lkZ2V0TmFtZT1zcF9hdGY&th=1"> Link </a> |
| Metal Washer | Used for securing one of the claws | $0.25 per washer, 1 washer | <a href="https://www.homedepot.com/b/Hardware-Fasteners-Washers-Flat-Washers/N-5yc1vZc2ck"> Link </a> |
| PVC Washer | Used as a lazy susan for the base of the arm | $2 per washer, 1 washer | <a href="https://www.minosaver.com/pvc-washer"> Link </a> |
| Bearing | Used for securing one of the claws | $10 per bearing, 1 bearing | <a href="https://www.ebay.com/p/1349206302?iid=394354426952"> Link </a> |
