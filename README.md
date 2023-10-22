# EX-07 SQUARE WAVE GENERATION AT THE OUTPUT PIN USING TIMER
### Aim:
To generate a PWM wave at the timer pin output and  simuate it on  proteus using an virtual oscilloscope  
### Components required:
STM32 CUBE IDE, Proteus 8 simulator .
### Theory:
The timer modules can operate a variety of modes one of which is the PWM mode. Where the timer gets clocked from an internal source and counts up to the auto-reload register value, then the output channel pin is driven HIGH. And it remains until the timer counts reach the CCRx register value, the match event causes the output channel pin to be driven LOW. And it remains until the timer counts up to the auto-reload register value, and so on.

The resulting waveform is called PWM (pulse-width modulated) signal. Whose frequency is determined by the internal clock, the Prescaler, and the ARRx register. And its duty cycle is defined by the channel CCRx register value. The PWM doesn’t always have to be following this exact same procedure for PWM generation, however, it’s the very basic one and the easier to understand the concept. It’s called the up-counting PWM mode. We’ll discuss further advanced PWM generation techniques as we go on in this series of tutorials.

The following diagram shows you how the ARR value affects the period (frequency) of the PWM signal. And how the CCRx value affects the corresponding PWM signal’s duty cycle. And illustrates the whole process of PWM signal generation in the up-counting normal mode.

STM32 Timers – PWM Output Channels

Each Capture/Compare channel is built around a capture/compare register (including a shadow register), an input stage for capture (with a digital filter, multiplexing, and Prescaler) and an output stage (with comparator and output control). The output stage generates an intermediate waveform which is then used for reference: OCxRef (active high). The polarity acts at the end of the chain.
<img height=20% width=60% src="https://github.com/vasanthkumarch/EXPERIMENT--07-SQUARE-WAVE-GENERATION-AT-THE-OUTPUT-PIN-USING-TIMER/assets/36288975/87457b57-4311-440b-8cbe-a9d78db4335a">

STM32 Timers In PWM Mode

Pulse width modulation mode allows generating a signal with a frequency determined by the value of the TIMx_ARR register and a duty cycle determined by the value of the TIMx_CCRx register. The PWM mode can be selected independently on each channel (one PWM per OCx output) by writing 110 (PWM mode 1) or ‘111 (PWM mode 2) in the OCxM bits in the TIMx_CCMRx register. The user must enable the corresponding preload register by setting the OCxPE bit in the TIMx_CCMRx register, and eventually the auto-reload preload register by setting the ARPE bit in the TIMx_CR1 register.

OCx polarity is software programmable using the CCxP bit in the TIMx_CCER register. It can be programmed as active high or active low. For applications where you need to generate complementary PWM signals, this option will be suitable for you.

In PWM mode (1 or 2), TIMx_CNT and TIMx_CCRx are always compared to determine whether TIMx_CCRx≤TIMx_CNT or TIMx_CNT≤TIMx_CCRx (depending on the direction of the counter).

The timer is able to generate PWM in edge-aligned mode or center-aligned mode depending on the CMS bits in the TIMx_CR1 register.
STM32 PWM Frequency

In various applications, you’ll be in need to generate a PWM signal with a specific frequency. In servo motor control, LED drivers, motor drivers, and many more situations where you’ll be in need to set your desired frequency for the output PWM signal.

The PWM period (1/FPWM) is defined by the following parameters: ARR value, the Prescaler value, and the internal clock itself which drives the timer module FCLK. The formula down below is to be used for calculating the FPWM for the output. You can set the clock you’re using, the Prescaler, and solve for the ARR value in order to control the FPWM and get what you want.

STM32 PWM Frequency Formula - STM32 PWM Frequency Equation
![image](https://github.com/vasanthkumarch/EXPERIMENT--07-SQUARE-WAVE-GENERATION-AT-THE-OUTPUT-PIN-USING-TIMER/assets/36288975/aca8a20e-9b99-40c1-bada-f31accaa2ae9)

STM32 PWM Duty Cycle

In normal settings, assuming you’re using the timer module in PWM mode and generating PWM signal in edge-aligned mode up-counting configuration. The duty cycle percentage is controlled by changing the value of the CCRx register. And the duty cycle equals (CCRx/ARR) [%].
![image](https://github.com/vasanthkumarch/EXPERIMENT--07-SQUARE-WAVE-GENERATION-AT-THE-OUTPUT-PIN-USING-TIMER/assets/36288975/58ce0807-331e-49f7-bc8d-373f82592a92)

### Procedure:
<table>
	<tr>
		<td width=50%>
        1. Open CubeMX & Create New Project.
		</td>
 		<td>
			
 ![image](https://user-images.githubusercontent.com/36288975/226189166-ac10578c-c059-40e7-8b80-9f84f64bf088.png)
  		</td>
	</tr>
 	<tr>
		<td width=50%>
			2. Choose The Target MCU & Double-Click Its Name select the target to be programmed  as shown below and click on next.
		</td>
 		<td>
			
![image](https://user-images.githubusercontent.com/36288975/226189215-2d13ebfb-507f-44fc-b772-02232e97c0e3.png)
![image](https://user-images.githubusercontent.com/36288975/226189230-bf2d90dd-9695-4aaf-b2a6-6d66454e81fc.png)
![image](https://user-images.githubusercontent.com/36288975/226189280-ed5dcf1d-dd8d-43ae-815d-491085f4863b.png)
  		</td>
	</tr>
 	<tr>
		<td width=50%>
			3. Configure Timer2 Peripheral To Operate In PWM Mode With CH1 Output.
		</td>
 		<td>
			
![image](https://github.com/vasanthkumarch/EXPERIMENT--07-SQUARE-WAVE-GENERATION-AT-THE-OUTPUT-PIN-USING-TIMER/assets/36288975/682c851a-7dfe-4089-8395-f76088d43896)
  		</td>
	</tr>
 	<tr>
		<td width=50%>
			4. Set The RCC External Clock Source.
		</td>
 		<td>
			
![image](https://github.com/vasanthkumarch/EXPERIMENT--07-SQUARE-WAVE-GENERATION-AT-THE-OUTPUT-PIN-USING-TIMER/assets/36288975/8888af3b-63e2-4760-a51b-17b477763941)
  		</td>
	</tr>
 	<tr>
		<td width=50%>
			
   STM32 RCC External Clock Selection CubeMX<br>
   5. Go To The Clock Configuration.<br>
   6. Set The System Clock To Be 72MHz.<br>
   
</td>
 		<td>
			
![image](https://github.com/vasanthkumarch/EXPERIMENT--07-SQUARE-WAVE-GENERATION-AT-THE-OUTPUT-PIN-USING-TIMER/assets/36288975/4ea03faa-fb90-4b31-8079-3db5f959f2c3)
  		</td>
	</tr>
 	<tr>
		<td width=50%>
			7. Name & Generate The Project Initialization Code For CubeIDE or The IDE You’re Using.<br>
   8.  Creating Proteus project and running the simulation.<br>
   We are now at the last part of step by step guide on how to simulate STM32 project in Proteus.<br>
   9. Create a new Proteus project and place STM32F40xx i.e. the same MCU for which the project was created in STM32Cube IDE.<br>
   After creation of the circuit as per requirement as shown below 
		</td>
 		<td>
			
<img height=10% width=80% src="https://github.com/vasanthkumarch/EXPERIMENT--07-SQUARE-WAVE-GENERATION-AT-THE-OUTPUT-PIN-USING-TIMER/assets/36288975/4f377f5e-bdda-489e-a416-c712c893831d">
  		</td>
	</tr>
 	<tr>
		<td width=50%>
			10. Double click on the the MCU part to open settings. Next to the Program File option, give full path to the Hex file generated using STM32Cube IDE. Then set the external crystal frequency to 8M (i.e. 8 MHz). Click OK to save the changes.<br>
   11. click on debug and simulate using simulation as shown below.
		</td>
 		<td>
			
![image](https://github.com/vasanthkumarch/EXPERIMENT--07-SQUARE-WAVE-GENERATION-AT-THE-OUTPUT-PIN-USING-TIMER/assets/36288975/b8efbfc2-f0c5-4106-8117-3a6e7ac87f6c)
  		</td>
	</tr>
</table>

```
Developed By: ROHIT JAIN D
Registration No: 212222230120
```
<table>
	<tr>
		<td width=50%>

   ### STM 32 CUBE PROGRAM :

```C
HAL_TIM_Base_Start(&htim2);
HAL_TIM_PWM_Init(&htim2);
HAL_TIM_PWM_Start(&htim2,TIM_CHANNEL_1);
```
</td>
 		<td>
			
### Output screen shots of proteus  :
<img src="https://github.com/ROHITJAIND/EX-07-SQUARE-WAVE-GENERATION-AT-THE-OUTPUT-PIN-USING-TIMER/assets/118707073/3ffabdd0-faaf-4749-9433-396b2f0d2f50">
  		</td>
	</tr>
</table>

### DUTY CYCLE AND FREQUENCY CALCULATION :

<table>
	<tr>
		<td width=30%>

  
   ### FOR PULSE AT 500:

 ```C
   TON  = 2ms
   TOFF = 2ms
   TOTAL TIME = 4 
   FREQUENCY = 1/(TOTAL TIME)
             = 1/(410^-3)
             = 250Hz
  ```

</td>
 	 <td>
			
![242550962-173bbe4a-0cb5-4545-a5d4-48e39982a341](https://github.com/ROHITJAIND/EX-07-SQUARE-WAVE-GENERATION-AT-THE-OUTPUT-PIN-USING-TIMER/assets/118707073/e5d9cc90-2ce5-4115-bd06-c6edbf11f314)
  		</td>
	</tr>
<td width=30%>

   ### FOR PULSE AT 700:
   
```C
TON  = 2.17ms
TOFF = 0.93ms
TOTAL TIME = 3.1 
FREQUENCY = 1/(TOTAL TIME)
          = 1/(3.110^-3)
          = 322.58Hz
```
   
</td>
 		<td>
			
![242550863-82dd7207-afda-4540-a3b1-eb07b8347e8b](https://github.com/ROHITJAIND/EX-07-SQUARE-WAVE-GENERATION-AT-THE-OUTPUT-PIN-USING-TIMER/assets/118707073/63f1ddc1-3da5-4d76-ba92-fb3020fe7877)
  		</td>
	</tr>
 <td width=30%>

   ### FOR PULSE AT 900:   
```C
TON  = 2.88ms
TOFF = 0.32ms
TOTAL TIME = 3.2 
FREQUENCY = 1/(TOTAL TIME)
          = 1/(3.210^-30
          = 312.5Hz
```
</td>
 	<td>
			
![242551047-875d48ea-868e-48d1-8080-d8cc544d58a1](https://github.com/ROHITJAIND/EX-07-SQUARE-WAVE-GENERATION-AT-THE-OUTPUT-PIN-USING-TIMER/assets/118707073/d4590fcb-0d3b-4970-9c41-05a320834e36)
  		</td>
	</tr>
</table>

### Result :
A PWM Signal is generated using the following frequency and various duty cycles are simulated.
