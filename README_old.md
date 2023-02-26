1. Create SEGGER folder in middleware
2. copy "Config" folder to SEGGER->Config
3. copy systemview\Sample\FreeRTOSV10\Config\Cortex-M\SEGGER_SYSVIEW_Config_FreeRTOS.c 
to "Config"
4. copy systemview\Sample\FreeRTOSV10 .c and .h file to "OS" folder
5. copy SEGGER to SEGGER ansd delte non GCC files in Syscalls folder

1. in freeRTOSConfig.h add
at the end:
#include "SEGGER_SYSVIEW_FreeRTOS.h"

and at the other defines add
#define INCLUDE_xTaskGetIdleTaskHandle 1
#define INCLUDE_pxTaskGetStackStart 1

2. In SEGGER\SEGGER\SEGGER_SYSVIEW_ConfDefaults.h
change 
#define SEGGER_SYSVIEW_CORE SEGGER_SYSVIEW_CORE_OTHER
with 
#define SEGGER_SYSVIEW_CORE SEGGER_SYSVIEW_CORE_CM3

change
#define SEGGER_SYSVIEW_RTT_BUFFER_SIZE          1024
with 
#define SEGGER_SYSVIEW_RTT_BUFFER_SIZE          1024 * 4

3. in SEGGER\Config\SEGGER_SYSVIEW_Config_FreeRTOS.c
change SYSVIEW_APP_NAME andSYSVIEW_DEVICE_NAME

4. in main.c add
/* Private variables ---------------------------------------------------------*/
#define DWT_CTRL (*(volatile uint32_t*) 0xE0001000)

/* USER CODE BEGIN 2 */
DWT_CTRL |= (1<<0);
SEGGER_SYSVIEW_Conf();
SEGGER_SYSVIEW_Start();

5. add SEGGER Folder to includes

6. stm32l4xx_hal_msp.c add
NVIC_SetPriorityGrouping( 0 );

7 port.c change

void xPortSysTickHandler( void )
{
	/* The SysTick runs at the lowest interrupt priority, so when this interrupt
	executes all interrupts must be unmasked.  There is therefore no need to
	save and then restore the interrupt mask value as its value is already
	known. */
	portDISABLE_INTERRUPTS();
	traceISR_ENTER();
	{
		/* Increment the RTOS tick. */
		if( xTaskIncrementTick() != pdFALSE )
		{
			traceISR_EXIT_TO_SCHEDULER();
			/* A context switch is required.  Context switching is performed in
			the PendSV interrupt.  Pend the PendSV interrupt. */
			portNVIC_INT_CTRL_REG = portNVIC_PENDSVSET_BIT;
		}
		else {
			traceISR_EXIT();
		}
	}
	portENABLE_INTERRUPTS();
}


/include/FreeRTOS.h
/Source/portable/GCC/ARM_CM4F/port.c
/Source/portable/GCC/ARM_CM4F/portmacro.h
/Source/tasks.c

x /Middlewares/Third_Party/FreeRTOS/Source/include/FreeRTOS.h
x /Core/Inc/FreeRTOSConfig.h
x /Middlewares/Third_Party/FreeRTOS/Source/portable/GCC/ARM_CM4F/port.c
/Core/Src/stm32l4xx_hal_msp.c
/Middlewares/Third_Party/FreeRTOS/Source/tasks.c

/Middlewares/Third_Party/FreeRTOS/Source/include/FreeRTOS.h
/Middlewares/Third_Party/FreeRTOS/Source/portable/GCC/ARM_CM4F/port.c
/Middlewares/Third_Party/FreeRTOS/Source/portable/GCC/ARM_CM4F/portmacro.h
/Middlewares/Third_Party/FreeRTOS/Source/tasks.c
/Core/Inc/FreeRTOSConfig.h
