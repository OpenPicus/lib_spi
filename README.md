lib_spi
=======

Flyport library for SPI communication, released under GPL v.3.<br>
The library allows to communicate through SPI.<br>
More info on wiki.openpicus.com.<br>
1) import files inside Flyport IDE using the external libs button.<br>
2) add following code example in FlyportTask.c:<br>

<code>
/* example with MCP41100 digital potentiometer (Microchip) */

#include "taskFlyport.h"
#include "SPIHelper.h"

void FlyportTask()
{
    
  UARTWrite(1,"set PIN and SPI");

	IOInit(p8, SPICLKOUT);
	IOInit(p10, SPI_OUT);
	IOInit(p12, SPI_IN);
	//SS pin
	IOInit(p14, out);
	IOPut(p14, on);
	IOInit(p14, out);
	
	SPIContext mySPI;
	
	SPIConfig(&mySPI, SPI_OPT_MASTER | SPI_OPT_MODE_0, p14, 250000);
	SPIContextRestore(&mySPI);
	SPIOpen();
	
	BYTE i=0;

	while(1)
	{
            IOPut(o5,on);
            for(i=240;i<255;i++)
            {
                vTaskSuspendAll();
                SPIStart(&mySPI);
                SPIWriteByte(0b00010001);   //command: write + out1
                SPIWriteByte(i);            //write byte that set the percent of partitioning
                SPIStop(&mySPI);
                xTaskResumeAll();
                vTaskDelay(25);
            }
            IOPut(o5,off);
            vTaskDelay(50);
	}
}
</code>
