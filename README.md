# Keypad-Lock-System-with-EEPROM-and-LCD
A Simple lock system using arduino uno
This project implements a simple electronic lock system using a keypad, an LCD display, and an EEPROM for storing passwords. The system can be locked and unlocked with a 4-digit password and allows the user to change the password. <br>

## Components <br>
LiquidCrystal: library to control an LCD display.
Keypad: library to handle keypad input.
EEPROM: library for reading and writing data to the EEPROM. <br>

## How It Works <br>
Initial Setup:
The system checks for a saved password in the EEPROM.
If no password is found, a default password (0000) is set.<br>

## Modes: <br>

### Locked Mode (mode = 0): <br>
The system is locked, and the user must enter the correct password to unlock it.
If the correct password is entered, a green LED lights up, and a welcome message is displayed.
If the wrong password is entered, a red LED lights up, and an "INCORRECT PASSWORD" message is displayed. <br>
### Set New Password Mode (mode = 1): <br>
Allows the user to set a new password.
The user is prompted to enter a new password twice for confirmation.<br>
### Confirm New Password Mode (mode = 2): <br>
The system checks if the new password matches the confirmation.
If they match, the new password is saved to the EEPROM, and a success message is displayed.
If they don't match, an error message is displayed. <br>
### Unlocked Mode (mode = 3): <br>
The system is unlocked, and the user can choose to either change the password (by pressing #) or lock the system (by pressing '*'). <br>

## Notes <br>
The password length is set to 4 digits.

