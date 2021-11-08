# Lab 7: TEODOR COSMA

Link to this file in your GitHub repository:

[https://github.com/teodorcosma/Digital-electronics-2/edit/main/Homeworks/Lab7.md](https://github.com/teodorcosma/Digital-electronics-2/edit/main/Homeworks/Lab7.md)


### Analog-to-Digital Conversion

1. Complete table with voltage divider, calculated, and measured ADC values for all five push buttons.

   | **Push button** | **PC0[A0] voltage** | **ADC value (calculated)** | **ADC value (measured)** |
   | :-: | :-: | :-: | :-: |
   | Right  | 0&nbsp;V | 0   | 0 |
   | Up     | 0.495&nbsp;V | 101 | 99 |
   | Down   |   0.856&nbsp;V    |  175.13   | 257 |
   | Left   |    1.25&nbsp;V   |   255.75  | 409 |
   | Select |    2.5&nbsp;V   |  511.5   | 640 |
   | none   |    5&nbsp;V   |  1023   | 1023 |

2. Code listing of ACD interrupt service routine for sending data to the LCD/UART and identification of the pressed button. Always use syntax highlighting and meaningful comments:

```c
/**********************************************************************
 * Function: ADC complete interrupt
 * Purpose:  Display value on LCD and send it to UART.
 **********************************************************************/
ISR(ADC_vect)
{
    uint16_t value = 0;
    char lcd_string[4] = "0000";

    value = ADC;	// Copy ADC result to 16-bit variable
    itoa(value, lcd_string, 10);	// Convert decimal value to string

   	// WRITE YOUR CODE HERE
    lcd_gotoxy(8, 0);
    lcd_puts(lcd_string);
    uart_puts(lcd_string);
    uart_puts(" ");

   	//transforms value in hexa
    itoa(value, lcd_string, 16);
    lcd_gotoxy(13, 0);
    lcd_puts(lcd_string);
    lcd_gotoxy(8, 1);

   	//clears c
    lcd_puts("        ");

   	//shows what button is pressed
    lcd_gotoxy(8, 1);

    if (value == 1023)
    {
        lcd_puts("none");
    }
    else if (value == 99)
    {
        uart_puts("up ");
        lcd_puts("up");
    }
    else if (value == 0)
    {
        uart_puts("right ");
        lcd_puts("right");
    }
    else if (value == 257)
    {
        uart_puts("down ");
        lcd_puts("down");
    }
    else if (value == 409)
    {
        uart_puts("left ");
        lcd_puts("left");
    }
    else if (value == 640)
    {
        uart_puts("select ");
        lcd_puts("select");
    }
}
```


### UART communication

1. (Hand-drawn) picture of UART signal when transmitting three character data `De2` in 4800 7O2 mode (7 data bits, odd parity, 2 stop bits, 4800&nbsp;Bd).

   ![your figure]()

2. Flowchart figure for function `get_parity(uint8_t data, uint8_t type)` which calculates a parity bit of input 8-bit `data` according to parameter `type`. The image can be drawn on a computer or by hand. Use clear descriptions of the individual steps of the algorithms.

   ![your figure]()


### Temperature meter

Consider an application for temperature measurement and display. Use temperature sensor [TC1046](http://ww1.microchip.com/downloads/en/DeviceDoc/21496C.pdf), LCD, one LED and a push button. After pressing the button, the temperature is measured, its value is displayed on the LCD and data is sent to the UART. When the temperature is too high, the LED will start blinking.

1. Scheme of temperature meter. The image can be drawn on a computer or by hand. Always name all components and their values.

   ![your figure]()
