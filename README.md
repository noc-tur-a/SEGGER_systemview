1. copy the ```SEGGER``` folder into the ```Middlewares``` folder
2. Apply the patches
3. Settings:

- In SEGGER\SEGGER\SEGGER_SYSVIEW_ConfDefaults.h<br>
```#define SEGGER_SYSVIEW_RTT_BUFFER_SIZE          1024 * 32```

- in main.c
```
/* USER CODE BEGIN PM */
#define DWT_CTRL (*(volatile uint32_t*) 0xE0001000)

/* USER CODE BEGIN 2 */
DWT_CTRL |= (1<<0);
SEGGER_SYSVIEW_Conf();
SEGGER_SYSVIEW_Start();
```
- If you want to use the build in printf functionality
```
#include "SEGGER_SYSVIEW.h"

//in the Task:
char msg[100];
//in the infinite loop
snprintf(msg, 100, "%s\n", (char*) "Hello Blue");
SEGGER_SYSVIEW_PrintfTarget(msg);
```

Use continous recording 


- https://www.segger.com/downloads/systemview/
- https://www.segger.com/products/debug-probes/j-link/models/other-j-links/st-link-on-board/
- https://www.codeinsideout.com/blog/stm32/segger-systemview/#record-functions