# STM32-Timer-Interrupt-UART-Logger
This project demonstrates how to use a timer interrupt (TIM6) in STM32 to execute tasks periodically. Every 1 second, an interrupt is triggered, and a message is sent over UART.
# STM32 Timer Interrupt + UART

## About this project

This is a simple project where I used a timer interrupt to send messages over UART.

Instead of writing code inside the main loop, I used TIM6 interrupt so that a task runs automatically at fixed intervals (every 1 second).

Whenever the timer overflows, STM32 sends a message to the serial terminal.

---

## What it does

- Uses TIM6 timer in interrupt mode
- Generates interrupt every 1 second
- Sends message through UART
- Message appears in PuTTY (or any serial terminal)

---

## How it works (simple)

1. Timer is configured with prescaler and period
2. Timer starts in interrupt mode
3. Every 1 second → interrupt triggers
4. Inside interrupt → UART sends message

So basically:Timer → Interrupt → UART Print

---

## Output

In PuTTY, you will see:
Hello from Timer Interrupt
Hello from Timer Interrupt
Hello from Timer Interrupt

(repeats every 1 second)

---

## Pin Configuration

### UART (for serial communication)

- TX → PA2  
- RX → PA3  
- Baud rate → 115200  

---

### Onboard LED (optional)

- LD2 → PA5 (can be used for testing)

---

## Timer Configuration

- Timer Used → TIM6  
- Prescaler → 8399  
- Period → 9999  

This gives approximately:1 interrupt per second

---

## Important Code Part

### Starting Timer Interrupt
```c
HAL_TIM_Base_Start_IT(&htim6);
void HAL_TIM_PeriodElapsedCallback(TIM_HandleTypeDef *htim)
{
    if(htim->Instance == TIM6)
    {
        char msg[] = "Hello from Timer Interrupt\r\n";
        HAL_UART_Transmit(&huart2 ,(uint8_t*)msg, strlen(msg),HAL_MAX_DELAY);
    }
}

```
Why this project is useful

This is a basic example to understand:

Timer interrupts
Periodic task execution
UART communication

Instead of using delays, interrupts make the system more efficient.

Limitations
Uses blocking UART (HAL_MAX_DELAY)
No multiple tasks handling
Fixed timing (1 second)
Future improvements
Use non-blocking UART (interrupt/DMA)
Toggle LED along with message
Add multiple timers
Send different data dynamically
Final note

This project helped me understand how interrupts actually work in STM32.

Once you understand this, you can build real-time applications like:

sensor reading
periodic logging
communication systems
