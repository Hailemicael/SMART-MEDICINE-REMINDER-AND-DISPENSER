# Medicine Reminder System

This project is a medicine reminder system built using an Arduino, an LCD screen, a keypad, a buzzer, and a servo motor. It helps patients or users remember to take their medication at specific intervals during the day by setting alarms that notify them through an LCD display and buzzer. The system allows users to set the number of reminders per day, the start time, and alarms for each medicine intake.

## Features
- **User Input via Keypad**: The user can specify how many times per day they need to take their medicine (1, 2, 3, or 4 times a day).
- **Customizable Alarm Times**: The user can set the starting time, and the system will calculate the subsequent alarm times.
- **Buzzer Alarm**: A buzzer will sound at the set time to remind the user to take their medicine.
- **Servo Motor**: A servo motor moves when the alarm goes off, which can be used to release the medicine or perform another action.
- **LCD Display**: Displays the current time and reminders for the next medicine intake.

## Components
- **Arduino**
- **LCD Display** (16x2)
- **4x3 Keypad**
- **Buzzer**
- **Servo Motor**
- **RTC DS3232 Module** (for real-time clock functionality)

## How It Works
1. When the system starts, it prompts the user to input how many times a day they want to be reminded to take their medicine.
2. The user sets the start time (hour) for the first reminder.
3. Based on the number of reminders selected, the system calculates the subsequent times for each dose.
4. At the specified time, the system triggers the buzzer and moves the servo motor to alert the user.
5. The LCD screen displays the current time and the next reminder time.

## Code Overview

The main parts of the code:
- **Keypad Input**: Users can set the number of reminders per day and input the time for the first medicine intake using the keypad.
- **RTC Module**: The DS3232RTC library is used to get real-time clock data, which is displayed on the LCD and used to trigger reminders.
- **Buzzer and Servo**: The buzzer will sound when it’s time to take the medicine, and the servo can be programmed to move when the alarm goes off.

### Dependencies
- **LiquidCrystal Library**: To control the LCD display.
- **Keypad Library**: For keypad input handling.
- **DS3232RTC Library**: To handle real-time clock functionality.
- **Servo Library**: To control the servo motor.

### Installation

1. **Clone or download this repository**.
2. **Install the required libraries** in your Arduino IDE:
   - LiquidCrystal
   - Keypad
   - DS3232RTC
   - Servo
3. **Upload the code** to your Arduino board.
4. **Connect the components** as per the code and circuit diagram.

### Circuit Connections
- **LCD**: Connected to pins 12, 11, 5, 4, 3, 2.
- **Keypad**: Connected to pins 6, 7, 8, 9, 10, 1, 0.
- **Buzzer**: Connected to pin 13.
- **Servo Motor**: Connected to pin A3.
- **RTC Module**: Connected via I2C.

### Usage

1. Upon startup, the system will display a welcome message and ask the user to input how many times they want to be reminded (1, 2, 3, or 4 times a day).
2. Then the user will input the starting hour for the first reminder.
3. The system will calculate the times for the subsequent reminders and display the current time along with the next reminder time.
4. When it is time to take the medicine, the system will sound the buzzer and move the servo motor, indicating it is time to take the medication.

### Future Improvements
- Add an interface to adjust the reminder time for each intake.
- Implement a battery backup system for the RTC in case of power failure.
- Extend the system to send SMS or app notifications for remote reminders.

### License
This project is licensed under the MIT License. See the `LICENSE` file for more details.

