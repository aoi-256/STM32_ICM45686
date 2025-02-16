# STM32_ICM45686

デバック機能を追加します

## サンプルコード

**wrapper.cpp**
```cpp 
#include "ICM45686.h"
#include "wrapper.hpp"
#include "usart.h"
#include <string>

ICM45686 icm(&hi2c3, &huart2);

int16_t Gyro_Data[3];
int16_t Accel_Data[3];

void Send_Data(int16_t data[3]);

void init(){

	icm.Connection();
	icm.Accel_Config(icm.Mode::low_noize, icm.Accel_Scale::scale_02g, icm.ODR::rate_6400hz);
	icm.Gyro_Config(icm.Mode::low_noize, icm.Gyro_Scale::scale_0250dps, icm.ODR::rate_6400hz);

}

void loop(){

	icm.Get_Data(Accel_Data, Gyro_Data);

	Send_Data(Accel_Data);

	HAL_Delay(1);
}

void Send_Data(int16_t Data[3]){

	std::string str;
	str = std::to_string(Data[0]) + " " + std::to_string(Data[1]) + " " + std::to_string(Data[2]) + "\n";

	HAL_UART_Transmit(&huart2, (uint8_t *)str.c_str(),str.length(),100);
}
```
