# 차량용 SW개발


MISRA-C 룰

## Control System
![vehicle control system](./picture/vehicle_control_system.jpg)
## Automotive Control System
![automotive_control_system](./picture/car_system.gif)

- System
- Powertrain
- Chassis
- Body
- Infotainment
- Carburetor
- ECU(Engine Control Unit)
- ECU(Electronic Control Unit)
- ABS Controller

### Software engineering model
- CMMI(Capability Maturity Model Intergration)
- Waterfall model
- V model
- V model(toll-base developement process)


### MDB(Model Based Design)
- 모델 복잡도를 낮추고 생산성을 높이는 모델
- Auto Code Generator 제공
- V Cycle
  - MIL, SIL, PIL, HIL
- 자동차 SW에서의 최소unit : 함수


## AUTOSAR
- `AUT`omataion `O`pen `S`ystem `AR`chitecture
- Software, configuration tool, Auto code generation
### AUTOSAR ECU Software Architexture
- divided in three parts
- AUTOSAR Application Software
- AUTOSAR Basic Sofrware(BSW)
- AUTOSAR Runtime Environment(RTE)

>AUTOSAR Architecture
![AUTOSAR1](./picture/Autosar1.jpg)

>AUTOSAR Architecture Detail
![AUTOSAR2](./picture/Autosar2.jpg)

### ISO26262
- HARA(Hazard Analysis and Risk Assessment)

### SAE J3061
- TARA(Threat Assessment and Remediation Analysis)
- SHE -> EVITA
  

### MCU Manual - AURIX
- Process
  - Core
  - MPU
  - FPU
- Peripheral Controller
  - I/O
  - ADC
  - Timer
  - Interrupt
  - CAN
