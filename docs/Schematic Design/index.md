## MPLabX Code
Attached is a file of the HMI subsystem [code](HMI_Subsystem.zip).

## Raw Code
----
* EGR 314
  * HMI Subsystem for Team 202
  * By: Alex Dooley
  *
  *
  *
  *
*/
#include <stdbool.h>
#include <stdint.h>
#include <stdio.h>
#include "mcc_generated_files/system/system.h"
#include "mcc_generated_files/timer/tmr1.h"
#include "mcc_generated_files/uart/eusart1.h"
#include "ssd1306.h"

#define RPM_VALUE_COUNT (sizeof(RPM_values) / sizeof(RPM_values[0]))

char TEXT[32];

uint16_t ms=0;
uint16_t sec=0;
uint8_t RPM;

uint8_t motor_menu_state = 0;      // 0: Stay, 1: Open, 2: Close
bool motor_selection_mode = false; // Menu mode for Motor State

uint8_t RPM_menu_value = 0;       // Start
uint8_t RPM_STEP = 10;            // Step Size
const uint8_t RPM_MIN = 0;
const uint8_t RPM_MAX = 100;
bool RPM_selection_mode = false; // Menu mode for RPM


// Callback function to increment the timer to have milliseconds and seconds and blink the LED every second
void timer_callback(void)
{
    ms++;
    if (ms>1000)
    {
        ms -= 1000;
        sec++;
        //IO_RD6_Toggle();
    }
}

void eusart_callback(void)
{
    //ms++;
}
// Send the message function
uint8_t send_message(char * my_message){
    printf("%s",my_message);
    return 1;
}

#define BUFSIZE 16
#define MSGSIZE 64
#define TEAMSIZE 5
#define MSGTESTSIZE 64
#define MSGTESTCHAR 0

const char my_id='a';
const char team_ids[TEAMSIZE+1]="abcdX";
char buffer_in[BUFSIZE+1];
char message_in[MSGSIZE+1];
char message_out[MSGSIZE+1];
unsigned int message_me=0;
unsigned int processing = 0;


void fill_string(char * mystring,char value,unsigned int size){
    for (int ii=0;ii<size;ii++){
        mystring[ii]=value;
    }
}

unsigned int find_char(char * mystring, char value,unsigned int size){
    char c=0;
    for (int ii=0;ii<size;ii++){
        c= mystring[ii];
        printf("%c,%c;",value,mystring[ii]);
        if (c==value){
            return 1;
        }
    }
    return 0;
}

void handle_message(unsigned int ii){
    message_in[ii]=0;
    char receiver_id = message_in[3];
    char sender_id = message_in[2];
    uint8_t message_input = message_in[5];
//    printf("Receiver= %c\n", receiver_id);
//    printf("Sender= %c\n", sender_id);
    if (receiver_id == 'a' || receiver_id == 'X') {
        if (sender_id == 'c'){
            RPM = message_in[5];
//            ssd1306_init(0x3C);
//            ssd1306_string_pos(10, 0, RPM);
            sprintf(TEXT, "RPM = %d", RPM); // Format the number into the string\

            IO_RC2_Toggle();                // Indicator
            IO_RC2_SetHigh();
        }
        if (receiver_id == 'X'){
            send_message(message_in);
        }
    }
   
    else {
        send_message(message_in);
    }
   
}

int main(void)
{
    uint8_t IO_RA0_LastValue;
    uint8_t IO_RA1_LastValue;
    uint8_t IO_RA2_LastValue;
    uint8_t IO_RA3_LastValue;
    uint8_t IO_RA4_LastValue;
    uint8_t IO_RA5_LastValue;
   
    uint8_t IO_RA0_CurrentValue;
    uint8_t IO_RA1_CurrentValue;
    uint8_t IO_RA2_CurrentValue;
    uint8_t IO_RA3_CurrentValue;
    uint8_t IO_RA4_CurrentValue;
    uint8_t IO_RA5_CurrentValue;
   
    char c = 0;
    unsigned int buffer_ii=0;
    unsigned int buffer_last_ii = 0;
    unsigned int message_ii=0;
    unsigned int message_last_ii=0;
    buffer_in[BUFSIZE]=0;
    message_in[MSGSIZE]=0;
    unsigned int message_incoming=0;
    unsigned int message_me=0;
    unsigned int processing = 0;

    fill_string(buffer_in,'a',BUFSIZE);
    fill_string(message_in,'_',MSGSIZE);
    message_in[MSGTESTSIZE]=MSGTESTCHAR;
   
    // Sets my_id as a char so I can compare incoming messages
    char me = 0;
    me = my_id;

    uint16_t ms_last=0;
    uint16_t sec_last=0;
    SYSTEM_Initialize();
    ADC_Initialize();
    ssd1306_init(0x3C);

    // Enable the Global Interrupts
    INTERRUPT_GlobalInterruptEnable();

    // Disable the Global Interrupts
    //INTERRUPT_GlobalInterruptDisable();

    // Enable the Peripheral Interrupts
    INTERRUPT_PeripheralInterruptEnable();

    // Disable the Peripheral Interrupts
    //INTERRUPT_PeripheralInterruptDisable();
    Timer1_Initialize();
    Timer1_Start();
    TMR1_TMRInterruptEnable();

    TMR1_OverflowCallbackRegister(timer_callback);

    UART1_Initialize();

    while(1)
    {
        IO_RA0_CurrentValue=IO_RA0_GetValue();
        __delay_ms(10);
        IO_RA1_CurrentValue=IO_RA1_GetValue();
        __delay_ms(10);
        IO_RA2_CurrentValue=IO_RA2_GetValue();
        __delay_ms(10);
        IO_RA3_CurrentValue=IO_RA3_GetValue();
        __delay_ms(10);
        IO_RA4_CurrentValue=IO_RA4_GetValue();
        __delay_ms(10);
        IO_RA5_CurrentValue=IO_RA5_GetValue();
        __delay_ms(10);
       
        if(EUSART1_IsRxReady())
        {
            c= EUSART1_Read();
            //printf("%c", c);
            buffer_in[buffer_ii]=c;
            if (buffer_in[buffer_last_ii]=='A' & buffer_in[buffer_ii]=='Z'){
                fill_string(message_in,'_',MSGSIZE);
                message_in[MSGTESTSIZE]=MSGTESTCHAR;
                message_incoming=1;
                message_in[0] = buffer_in[buffer_last_ii];
                message_ii=1;
                //printf("message_alex_start");
            }
            if (buffer_in[buffer_last_ii]=='Y' & buffer_in[buffer_ii]=='B'){
                message_incoming=0;
                message_in[message_ii] = buffer_in[buffer_ii];
                message_last_ii= message_ii;
                message_ii = message_ii+1;
                handle_message(message_ii);
                //printf("message_end");
                //printf("PIC_alex; message received", message_in);
            }
            if (message_incoming!=0){
                message_in[message_ii] = buffer_in[buffer_ii];

                if (message_ii==2){
                    unsigned result = 0;
                    char d=0;
                    d = message_in[message_ii];
                    result= find_char(team_ids,d,TEAMSIZE);
                    if (result==0){
                        //printf("AZabPIC: sender not in teamYB\n");
                        message_incoming = 0;
                        message_ii=0;
                    } else {
                    }
                }

                if (message_ii==2){
                    unsigned result = 0;
                    char d=0;
                    d = message_in[message_ii];
                    if (d=='a'){
                        //printf("AZabPIC: sender is yourselfYB\n");
                        message_incoming = 0;
                        message_ii=0;
                    } else {
                    }
                }
                if (message_ii==3){
                    unsigned result = 0;
                    char d=0;
                    d = message_in[message_ii];
                    result= find_char(team_ids,d,TEAMSIZE);
                    if (result==0){
                        //printf("AZabPIC: receiver not in teamYB\n");
                        message_incoming = 0;
                        message_ii=0;
                    } else {
                    }
                }

                message_last_ii= message_ii;
                message_ii = message_ii+1;
                if (message_ii<MSGTESTSIZE){} else{
                    //printf("AZabPIC: message too large. deletingYB\n");
                    message_incoming=0;
                    message_ii=0;
                }
            }
            buffer_last_ii= buffer_ii;
            buffer_ii = (buffer_ii+1)%BUFSIZE;
        }

        if (IO_RA0_CurrentValue == 0 && IO_RA0_LastValue == 1)
        {
            sprintf(TEXT, " Current RPM = %d", RPM); // Format the number into the string

            IO_RC0_Toggle();                // Indicator
            ssd1306_clear();                // Clear the display
            ssd1306_setscale(0);            // Default font size
            ssd1306_string_pos(0, 0, TEXT); // Draw the string on screen
            IO_RC0_SetLow();
        }
        if (IO_RA1_CurrentValue == 0 && IO_RA1_LastValue == 1)
        {
            RPM_menu_value += RPM_STEP;
            if (RPM_menu_value > RPM_MAX)
                RPM_menu_value = RPM_MIN;

            RPM_selection_mode = true;

            // Display the current RPM selection
            ssd1306_clear();
            ssd1306_setscale(0);
            ssd1306_string_pos(0, 0, "Select RPM:");

            char rpm_text[32];
            sprintf(rpm_text, "%d RPM", RPM_menu_value);
            ssd1306_string_pos(0, 2, rpm_text);

            IO_RC0_Toggle(); // Feedback
        }
       
        if (IO_RA2_CurrentValue == 0 && IO_RA2_LastValue == 1)
        {
            // Cycle through motor states (0 -> 1 -> 2 -> 0 ...)
            motor_menu_state = (motor_menu_state + 1) % 3;
            motor_selection_mode = true;

            ssd1306_clear();
            ssd1306_setscale(0);
            ssd1306_string_pos(0, 0, "Select Motor State:");

            switch (motor_menu_state)
            {
                case 0:
                    ssd1306_string_pos(0, 2, "STAY");
                    break;
                case 1:
                    ssd1306_string_pos(0, 2, "OPEN");
                    break;
                case 2:
                    ssd1306_string_pos(0, 2, "CLOSE");
                    break;
            }

            IO_RC2_Toggle(); // Visual indicator
        }
       
        // Selecting Current Motor Option
        if (IO_RA5_CurrentValue == 0 && IO_RA5_LastValue == 1 && motor_selection_mode)
        {
            uint8_t messageType1 = 1;
            uint8_t command = motor_menu_state; // 0 = stay, 1 = open, 2 = close

            // Build then send message
            printf("AZad");
            __delay_ms(5);
            EUSART1_Write(messageType1);
            __delay_ms(5);
            EUSART1_Write(command);
            __delay_ms(5);
            printf("YB");
            __delay_ms(5);

            ssd1306_clear();
            ssd1306_setscale(0);
            char msg[32];
            switch (command)
            {
                case 0: sprintf(msg, "Motor set to: STAY");
                break;
                case 1: sprintf(msg, "Motor set to: OPEN");
                break;
                case 2: sprintf(msg, "Motor set to: CLOSE");
                break;
            }
            ssd1306_string_pos(0, 0, msg);

            IO_RC2_SetHigh(); // Visual indicator
            motor_selection_mode = false;  // Exit menu
        }
       
        // Selecting RPM Value
        if (IO_RA5_CurrentValue == 0 && IO_RA5_LastValue == 1 && RPM_selection_mode)
        {
            uint8_t messageType3 = 3;
            uint8_t selected_rpm = RPM_menu_value; // Range: 0 - 100 RPM

            // Build then send message
            printf("AZac");
            __delay_ms(5);
            EUSART1_Write(messageType3);
            __delay_ms(5);
            EUSART1_Write(selected_rpm);
            __delay_ms(5);
            printf("YB");
            __delay_ms(5);

            // Display Configuration
            ssd1306_clear();
            ssd1306_setscale(0);
            char msg[32];
            sprintf(msg, "RPM set to: %d", selected_rpm);
            ssd1306_string_pos(0, 0, msg);
           

            IO_RC0_SetHigh(); // Visual indicator
            RPM_selection_mode = false;  // Exit menu
        }

        sec_last = sec;
        ms_last = ms;
        IO_RA0_LastValue = IO_RA0_CurrentValue;
        IO_RA1_LastValue = IO_RA1_CurrentValue;
        IO_RA2_LastValue = IO_RA2_CurrentValue;
        IO_RA3_LastValue = IO_RA3_CurrentValue;
        IO_RA4_LastValue = IO_RA4_CurrentValue;
        IO_RA5_LastValue = IO_RA5_CurrentValue;
    }
}

---
