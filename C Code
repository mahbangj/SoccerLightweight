
/*****************************************************
This program was produced by the
CodeWizardAVR V2.04.4a Advanced
Automatic Program Generator
© Copyright 1998-2009 Pavel Haiduc, HP InfoTech s.r.l.
http://www.hpinfotech.com

Project : 
Version : 
Date    : 3/27/2016
Author  : NeVaDa
Company : 
Comments: 


Chip type               : ATmega16
Program type            : Application
AVR Core Clock frequency: 12.000000 MHz
Memory model            : Small
External RAM size       : 0
Data Stack size         : 256
*****************************************************/

#include <mega16.h>

#include <delay.h>

// I2C Bus functions
#asm
   .equ __i2c_port=0x18 ;PORTB
   .equ __sda_bit=0
   .equ __scl_bit=1
#endasm
#include <i2c.h>
//////////////////////////////////////////////////////////////////cmp
 int cmp,c;
 
 #define EEPROM_BUS_ADDRESS 0xc0
/* read a byte from the EEPROM */
unsigned char compass_read(unsigned char address) {
unsigned char data;
i2c_start();
i2c_write(EEPROM_BUS_ADDRESS);
i2c_write(address);
i2c_start();
i2c_write(EEPROM_BUS_ADDRESS | 1);
data=i2c_read(0);
i2c_stop();
return data;
}

/////////////////////////////////////////////////////////////////////

// Alphanumeric LCD Module functions
#asm
   .equ __lcd_port=0x15 ;PORTC
#endasm
#include <lcd.h>

#define ADC_VREF_TYPE 0x40

// Read the AD conversion result
unsigned int read_adc(unsigned char adc_input)
{
ADMUX=adc_input | (ADC_VREF_TYPE & 0xff);
// Delay needed for the stabilization of the ADC input voltage
delay_us(10);
// Start the AD conversion
ADCSRA|=0x40;
// Wait for the AD conversion to complete
while ((ADCSRA & 0x10)==0);
ADCSRA|=0x10;
return ADCW;
}


int adc_min, adc[16],i,shb,shr,shl,m,n,cmd;
void sn (void)
{
for (i=0;i<16;i++)
{
PORTB.7=(i/8)%2;
PORTA.5=(i/4)%2;
PORTA.6=(i/2)%2;
PORTA.7=(i/1)%2;
adc[i]=read_adc(0);
if (adc[adc_min]>adc[i])
adc_min=i;
}
lcd_gotoxy(0,0);
lcd_putchar((adc_min/10)%10+'0');
lcd_putchar((adc_min/1)%10+'0');
lcd_putchar('=');
lcd_putchar((adc[adc_min]/1000)%10+'0');
lcd_putchar((adc[adc_min]/100)%10+'0');
lcd_putchar((adc[adc_min]/10)%10+'0');
lcd_putchar((adc[adc_min]/1)%10+'0');

shb=read_adc(3);
lcd_gotoxy(0,1);
lcd_putchar('B');
lcd_putchar((shb/100)%10+'0');
lcd_putchar((shb/10)%10+'0');
lcd_putchar((shb/1)%10+'0');

shr=read_adc(2);
lcd_gotoxy(5,1);
lcd_putchar('R');
lcd_putchar((shr/100)%10+'0');
lcd_putchar((shr/10)%10+'0');
lcd_putchar((shr/1)%10+'0');

shl=read_adc(4);
lcd_gotoxy(11,1);
lcd_putchar('L');
lcd_putchar((shl/100)%10+'0');
lcd_putchar((shl/10)%10+'0');
lcd_putchar((shl/1)%10+'0');
}
void motor(int ml1,int ml2,int mr2,int mr1)
{

if(ml1>255)     ml1=255;
  if(ml2>255)     ml2=255;
  if(mr1>255)     mr1=255;
  if(mr2>255)     mr2=255;
  
  if(ml1<-255)    ml1=-255;
  if(ml2<-255)    ml2=-255;
  if(mr1<-255)    mr1=-255;
  if(mr2<-255)    mr2=-255;
if(ml1>0)
{
PORTD.6=1;
PORTC.3=0;
OCR2=ml1;
}
else
{
PORTD.6=0;
PORTC.3=1;
OCR2=-ml1;
}
if(ml2>0)
{
PORTD.2=1;
PORTD.3=0;
OCR1A=ml2;
}
else
{
PORTD.2=0;
PORTD.3=1;
OCR1A=-ml2;
}
if(mr2>0)
{
PORTD.0=0;
PORTD.1=1;
OCR1B=mr2;
}
else
{
PORTD.0=1;
PORTD.1=0;
OCR1B=-mr2;
}
if(mr1>0)
{
PORTB.2=0;
PORTB.4=1;
OCR0=mr1;
}
else
{
PORTB.2=1;
PORTB.4=0;
OCR0=-mr1;
}
}   


void main(void)
{
// Declare your local variables here

// Input/Output Ports initialization
// Port A initialization
// Func7=Out Func6=Out Func5=Out Func4=In Func3=In Func2=In Func1=In Func0=In 
// State7=0 State6=0 State5=0 State4=T State3=T State2=T State1=T State0=T 
PORTA=0x00;
DDRA=0xE0;

// Port B initialization
// Func7=Out Func6=Out Func5=Out Func4=Out Func3=Out Func2=Out Func1=In Func0=In 
// State7=0 State6=0 State5=0 State4=0 State3=0 State2=0 State1=T State0=T 
PORTB=0x00;
DDRB=0xFC;

// Port C initialization
// Func7=In Func6=In Func5=In Func4=In Func3=Out Func2=In Func1=In Func0=In 
// State7=T State6=T State5=T State4=T State3=0 State2=T State1=T State0=T 
PORTC=0x00;
DDRC=0x08;

// Port D initialization
// Func7=Out Func6=Out Func5=Out Func4=Out Func3=Out Func2=Out Func1=Out Func0=Out 
// State7=0 State6=0 State5=0 State4=0 State3=0 State2=0 State1=0 State0=0 
PORTD=0x00;
DDRD=0xFF;

// Timer/Counter 0 initialization
// Clock source: System Clock
// Clock value: 15.625 kHz
// Mode: Fast PWM top=FFh
// OC0 output: Non-Inverted PWM
TCCR0=0x6B;
TCNT0=0x00;
OCR0=0x00;

// Timer/Counter 1 initialization
// Clock source: System Clock
// Clock value: 15.625 kHz
// Mode: Fast PWM top=00FFh
// OC1A output: Non-Inv.
// OC1B output: Non-Inv.
// Noise Canceler: Off
// Input Capture on Falling Edge
// Timer 1 Overflow Interrupt: Off
// Input Capture Interrupt: Off
// Compare A Match Interrupt: Off
// Compare B Match Interrupt: Off
TCCR1A=0xA1;
TCCR1B=0x0B;
TCNT1H=0x00;
TCNT1L=0x00;
ICR1H=0x00;
ICR1L=0x00;
OCR1AH=0x00;
OCR1AL=0x00;
OCR1BH=0x00;
OCR1BL=0x00;

// Timer/Counter 2 initialization
// Clock source: System Clock
// Clock value: 15.625 kHz
// Mode: Fast PWM top=FFh
// OC2 output: Non-Inverted PWM
ASSR=0x00;
TCCR2=0x6C;
TCNT2=0x00;
OCR2=0x00;

// External Interrupt(s) initialization
// INT0: Off
// INT1: Off
// INT2: Off
MCUCR=0x00;
MCUCSR=0x00;

// Timer(s)/Counter(s) Interrupt(s) initialization
TIMSK=0x00;

// Analog Comparator initialization
// Analog Comparator: Off
// Analog Comparator Input Capture by Timer/Counter 1: Off
ACSR=0x80;
SFIOR=0x00;

// ADC initialization
// ADC Clock frequency: 15.625 kHz
// ADC Voltage Reference: AVCC pin
// ADC Auto Trigger Source: None
ADMUX=ADC_VREF_TYPE & 0xff;
ADCSRA=0x86;

// I2C Bus initialization
i2c_init();

// LCD module initialization
lcd_init(16);
     delay_ms(1000);
     c=compass_read(1);
while (1)
      { 
       sn(); 
      cmp=compass_read(1)-c;
       if (cmp<0)      cmp=cmp+255;
       if (cmp<128)    cmp=cmp;
       if (cmp>128)    cmp=cmp-255;

       if(cmp>=0) 
       {
       lcd_gotoxy(9,0);
       lcd_putchar('+');
       lcd_putchar((cmp/100)%10+'0') ;
       lcd_putchar((cmp/10)%10+'0') ;
       lcd_putchar((cmp/1)%10+'0') ; 
       }
       if(cmp<0)
       {
       lcd_gotoxy(9,0);
       lcd_putchar('-') ;
       lcd_putchar((-cmp/100)%10+'0') ;
       lcd_putchar((-cmp/10)%10+'0') ;
       lcd_putchar((-cmp/1)%10+'0') ; 
       } 

      cmp=-1*cmp;
      m=shr-shl;
      cmd=cmp+(m/-1.5); 

        if(adc[adc_min]<400)
         {
         if(adc_min==0)  
          {if(shb<60)
           {      
             motor(255+cmd,255+cmd,-255+cmd,-255+cmd);///////////0
           }
           else   motor(255+cmp,255+cmp,-255+cmp,-255+cmp);///////////0
          }
         else if(adc_min==1) 
          {
          if(shb<60)
           {
            motor(255+cmd,100+cmd,-255+cmd,-100+cmd);/////////1.5      motor(255+cmp,128+cmp,-255+cmp,-128+cmp);///////////1
           }
           else   motor(255+cmp,40+cmp,-255+cmp,-40+cmp);/////////1.3
          }
         else if(adc_min==2)    motor(255+cmp,-128+cmp,-255+cmp,128+cmp);///////////3      motor(255+cmp,0+cmp,-255+cmp,0+cmp);////////////////2    

         else if(adc_min==3)    motor(128+cmp,-255+cmp,-128+cmp,255+cmp);///////////5      motor(255+cmp,-128+cmp,-255+cmp,128+cmp);///////////3     

         else if(adc_min==4)    motor(0+cmp,-255+cmp,0+cmp,255+cmp);////////////////6      motor(255+cmp,-255+cmp,-255+cmp,255+cmp);///////////4      
              
         else if(adc_min==5)    motor(-128+cmp,-255+cmp,128+cmp,255+cmp);///////////7      motor(128+cmp,-255+cmp,-128+cmp,255+cmp);///////////5       

         else if(adc_min==6)    motor(-255+cmp,-255+cmp,255+cmp,255+cmp);///////////8      motor(0+cmp,-255+cmp,0+cmp,255+cmp);////////////////6

         else if(adc_min==7)    motor(-255+cmp,-255+cmp,255+cmp,255+cmp);///////////8      motor(-128+cmp,-255+cmp,128+cmp,255+cmp);///////////7       

         else if(adc_min==8)    motor(0+cmp,-255+cmp,0+cmp,255+cmp);////////////////6      motor(-255+cmp,-255+cmp,255+cmp,255+cmp);///////////8

         else if(adc_min==9)    motor(-128+cmp,-255+cmp,128+cmp,255+cmp);///////////7      motor(-255+cmp,-128+cmp,255+cmp,128+cmp);///////////9  

         else if(adc_min==10)   motor(-255+cmp,-255+cmp,255+cmp,255+cmp);///////////8      motor(-255+cmp,0+cmp,255+cmp,0+cmp);///////////////10

         else if(adc_min==11)   motor(-255+cmp,-128+cmp,255+cmp,128+cmp);///////////9      motor(-255+cmp,128+cmp,255+cmp,-128+cmp);//////////11 

         else if(adc_min==12)   motor(-255+cmp,0+cmp,255+cmp,0+cmp);///////////////10      motor(-255+cmp,255+cmp,255+cmp,-255+cmp);//////////12   

         else if(adc_min==13)   motor(-255+cmp,255+cmp,255+cmp,-255+cmp);//////////12      motor(-128+cmp,255+cmp,128+cmp,-255+cmp);//////////13

         else if(adc_min==14)   motor(-128+cmp,255+cmp,128+cmp,-255+cmp);//////////13      motor(0+cmp,255+cmp,0+cmp,-255+cmp);///////////////14

         else if(adc_min==15)
          {
           if(shb<60)
           {
           cmd=cmp+((shr-shl)/3);
            motor(64+cmd,255+cmd,-64+cmd,-255+cmd);//////////14.5      motor(128+cmp,255+cmp,-128+cmp,-255+cmp);//////////15
           } 
           else   motor(64+cmp,255+cmp,-64+cmp,-255+cmp);//////////14.5  
          }
         }
        else if ((shb>180 && shb<240) && ((shr-shl<=40)||(shl-shr<=80)))        motor(0+cmp,0+cmp,0+cmp,0+cmp);
        else
        {    
        n=(300-shb)/1.5;
        motor(-m-n+cmp,m-n+cmp,m+n+cmp,-m+n+cmp);
        }    
           
       //////////////out 
       if(shr>240 || shl<40)
        {    
         motor(-m+cmp,m+cmp,m+cmp,-m+cmp);
        }
        if(shl>230 || shr<57)
        {    
          motor(-m+cmp,m+cmp,m+cmp,-m+cmp);
        } 
        
        if(shl>240 && shr>240)
         {
         motor(255+cmp,255+cmp,-255+cmp,-255+cmp);
         }   
        if(shr>260 && shl>160)
         {
         motor(255+cmp,255+cmp,-255+cmp,-255+cmp);
         }  

//           if(shb>340 && (shr>290 || shl>290))
//          {
//           n=280-shb; 
//          motor(-m-n+cmp,m-n+cmp,m+n+cmp,-m+n+cmp);
//          }
      };
}
