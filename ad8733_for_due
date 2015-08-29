#include <SPI.h>

/*

FSYNC -> 31
SCLK -> SCK (SPI)
SDATA -> MOSI (SPI)
+Vin = 3,3V
GND -> GND

*/


#define FSYNC 31

long freq;

void setup()
{
  pinMode(FSYNC, OUTPUT);
  SPI.begin();
  Serial.begin(9600);
  digitalWrite(FSYNC, HIGH);
  SPI.setDataMode(SPI_MODE2);
  
  /* Poniżej definujemy częstotliwośćw Hz */
  freq = 1050;
  
  WriteFrequencyAD9833(freq);
  Serial.print("Frequency is ");
  Serial.print(freq);
  Serial.println("");
}

void loop()
{
  /* Pętla jest pusta */
}


void WriteFrequencyAD9833(long frequency)
{
  int MSB;
  int LSB;
  int phase = 0;
  long calculated_freq_word;
  float AD9833Val = 0.00000000;
  
  /* Tutaj jest zdefiniowana częstotliwość z jaką jest taktowany */
  /* AD983x lub AD9837, w przypadku AD9833 jest to 12500000 Hz
  /* dla AD9837 będzie to zapewne 16000000 Hz */
  AD9833Val = (((float)(frequency)) / 12500000);
  
  calculated_freq_word = AD9833Val * 0x10000000;
  MSB = (int)((calculated_freq_word & 0xFFFC000) >> 14);
  LSB = (int)(calculated_freq_word & 0x3FFF);
  LSB |= 0x4000;
  MSB |= 0x4000;
  phase &= 0xC000;
  WriteRegisterAD9833(0x2100);
  WriteRegisterAD9833(LSB);
  WriteRegisterAD9833(MSB);
  WriteRegisterAD9833(phase);
  /* Jedna z poniższych lini musi być zawsze odkomentowana */
  /* W zależności jaki chcemy mieć kształt sygnału, tę odkomentowujemy */
  WriteRegisterAD9833(0x2020);                           // Kwadrat
  //WriteRegisterAD9833(0x2000);                         // Sinus
  //WriteRegisterAD9837(0x2002);                         // Trójkąt
}

void WriteRegisterAD9833(int dat)
{
  digitalWrite(FSYNC, LOW);
  SPI.transfer(highByte(dat));
  SPI.transfer(lowByte(dat));
  digitalWrite(FSYNC, HIGH);
}
