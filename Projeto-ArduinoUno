// Projeto de Conclusão de curso de Técnico em Automação Industrial

// DOCTYPE C++
// C++ lang="eng" 

#include<SerialGSM.h>
#include <SoftwareSerial.h>
SerialGSM cel(2,3);4

String SMS;
String Remetente;
String Permitido="011986651827";
boolean sendonce=true;
int BO=4;
int BZ=5;
int LEDS=6;
int RELE=7;
int ONOFF=8;
int i;

void setup()
{
 cel.begin(9600);
 delay(25000);
 Serial.begin(9600);
 cel.Verbose(true);
 cel.Boot();
 cel.DeleteAllSMS();
 cel.FwdSMS2Serial();
 pinMode (BO,INPUT);
 pinMode (ONOFF,INPUT_PULLUP);
 pinMode (BZ,OUTPUT);
 pinMode (LEDS,OUTPUT);
 pinMode (RELE,OUTPUT);
 digitalWrite(BZ,LOW);
 digitalWrite(LEDS,LOW);
 digitalWrite(RELE,LOW);
}
void loop()
{
 if (digitalRead(ONOFF)==LOW)
 {
  digitalWrite(LEDS,HIGH);
  delay (5000);
  digitalWrite(LEDS,LOW);
  while (digitalRead(BO)==LOW){}
  digitalWrite(LEDS,HIGH);
  delay (5000);
  digitalWrite(LEDS,LOW);
  while (digitalRead(ONOFF)==LOW)
  {
   digitalWrite(RELE,HIGH);
   cel.println("ATD986651827");
   cel.Rcpt("011986651827");
   cel.Message("ALARME ACIONADO, FAVOR VERIFICAR!!");
   cel.SendSMS();
   for (i=0;i<30;i++)
   {
    digitalWrite(LEDS,HIGH);
    digitalWrite(BZ,HIGH);
    if(digitalRead(ONOFF)==HIGH){break;}
    delay (500);
    digitalWrite(LEDS,LOW);
    digitalWrite(BZ,LOW);
    if(digitalRead(ONOFF)==HIGH){break;}
    delay (500);
   }
   cel.println("ATH");
  }
  digitalWrite(LEDS,LOW); 
  digitalWrite(BZ,LOW);
  digitalWrite(RELE,LOW);
 }
 if (cel.ReceiveSMS())
 {
  Remetente=cel.Sender();
  if (Remetente==Permitido)
  {
   SMS=cel.Message();
   if (SMS[0]!='0')
   {
    cel.Rcpt("011986651827");
    cel.Message("PARTE ELETRICA/MOTOR TRAVADO");
    cel.SendSMS();
    digitalWrite(RELE,HIGH);
    digitalWrite(LEDS,LOW);
    digitalWrite(BZ,LOW);
    delay (500);
    digitalWrite(LEDS,HIGH);
    digitalWrite(BZ,HIGH);
    delay (500);
   }
   else
   {
    digitalWrite(RELE,LOW);
    digitalWrite(LEDS,LOW);
    digitalWrite(BZ,LOW);
   }
  }
 }
}
