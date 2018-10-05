# software PWM
  The purpose of this code is to generate a PWM signal that can be incremented using a button by a factor of 10% per press. For this task, software PWM requires no instance of OUTMOD to be used, so the use of timer interrupts and setting LED's manually according to the values in the CCR0 and CCR1 registers was done instead. This code can be utilized with the MSP430G2553 and the MSP430FR2311.

# Functionality
This code utilizes two interrupts; a Timer A0 interrupt and a Timer A1 interrupt (in the case of the MSP430FR2311, a Timer0 B0 interrupt and a Timer0 B1 interrupt). Each interrupt would perform a particular task when the certain value in the CCRX register was met. For example, in the case of the MSP430G2553, once the value in CCR0 was met, the LED bit (PIN 1.0) would be SET through the use of the Timer A0 interrupt, and would be RESET once the value in the CCR1 was met by the timer. In order to increment the duty cycle of the PWM signal, a Port 1 interrupt was added to record a button press and check the value in the CCR1 register. If the value was read to be below the value in CCR0, it would increment the signal by 100HZ (10%). However, if the value in CCR1 was greater or equal to the value in the CCR0 regeister, it will set the value in the CCR0 register to 0.
    
 # Differences between MSP430G2553 and MSP430FR2311
 Although mostly the same, below are the primary changes that can be observed:
 
 - All Timer A instances become Timer B
 - All instaces of CCR0, CCR1, CCTL0, CCTL1, and TACTL become TB0CCR0, TB0CCR1, TB0CCTL0, TB0CCTL1, and TBCTL
 - TASSEL becomes TBSSEL
 - Added PM5CTL0 &= ~LOCKLPM5 to unlock GPIO functionality
    
 # Results
It can be observed on both boards that pressing the button down will cause the LED's to brighten up (incrementing by 10% duty cycle on each press), until an overflow occurs and shuts off the LED, effectivly causing it to have a 0% duty cycle.
