# HAPPY

An AI based little desk companion that acts as a remainder and motivator to maintain discipline and consistency and grows along with me, deployed on STM32F407VG DISC-1.


## OVERVIEW

One month of sem holidays and one small step or learning everyday can make huge changes. I wanted to build something that help me learn and apply ECE concepts integrating AI and build confidence. I wanted it to be interesting and useful for me so I would be motivated to work on it every single day.


## FEATURES

- Wake everyday and remind me of the scheduled tasks until turned off manually or from the mobile application, going back to sleep mode

- Log the completed tasks (Audio Keyword Sptting based on a tinyML) and respective xp leading to growth of a virtual pet in a mobile application, setting the next state of OLED happy

- In case of incomplete task, motivate me to finish it the next time it wakes up and also prompt to log the reason for incompletion (flagged after 3 hrs of scheduled time) in the app, setting the next state of OLED sad

- Apart from this, it also acts as a motivator with an enthusiastic face when interrupted externally (asynchronously)

- The app also contains a section to enter a small note for each day along with a "PICTURE OF THE DAY"

- The app also shows a progress/streak tracker

 

## TOOLS

- STM32F407VG Discovery board (main MCU)

- INMP441 I2S MEMS microphone (audio capture for keyword spotting)
  
- X-CUBE-AI (TinyML deployment — on-device keyword spotting model)
  
- OLED display, I2C (facial expressions)

- Audio DAC / I2S output (voice reminders)

- HC 05 Bluetooth Module (sync with mobile app)

- STM32CubeIDE (firmware dev)

- KiCad (PCB design, later stage)



## ROADMAP

- [x] GPIO \& timer interrupt foundations

- [ ] I2C + OLED display, state-based face animations

- [ ] RTC alarms, low-power Stop/Standby modes

- [ ] I2S mic capture + audio framing
     
- [ ] Feature extraction (MFCC) + KWS model training
      
- [ ] X-CUBE-AI conversion + on-device deployment
      
- [ ] Real-time inference integrated into habit-logging state machine

- [ ] Audio DAC/I2S output

- [ ] Full state machine integration (all subsystems)

- [ ] Bluetooth UART comms, JSON data protocol

- [ ] KiCad PCB schematic + layout



 (App development runs in parallel throughout)



## STATUS

Day 1: Toolchain setup — switched to STM32CubeIDE v19 after install issues with standalone CubeMX. LED blink not yet started.

