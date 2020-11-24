## STM32L476 USB driver
References:
stm32l4xx CubeMX driver code
stm32l476 MCU reference manual
stm32 USB Device Library manual
USB complete: the developer's guide
Free Device Monitoring Studio by HHD for protocol analyzer.

### Introduction
We are focusing on the USB device for communication applications.

- usb host and device mode
USB communication can only be initiated by host. 
Even if the mouse, keyboard et al HID devices, the USB device has to inform the host before sending data.

- transfer mode:
control transfer: predefined transfer in USB specification and has to be supported in all USB devices.
This is used by ep0 in/out. control transfer can happen before address is assigned which is needed.
bulk transfer: no timing requirement
interrupt transfer: critical timing requirement
isochronous transfer: streaming data, can tolerate loss of data.

- device class
class: combine the features for similar applications and define the common interface for these applications.
common class such as CDC, HID.
device class is built on top of the low level USB driver

- To understand USB working, the stack structure shall be clear:
the hardware: usb chip in the device and usb hub on the host. Transmitting and receiving are done by the hardware, 
and interrupt events are generated to drive the logic.
device: the registers and interrupt generation, the ending point and buffers.
host: usb device up and down.

The low level driver on the device: interrupt service routing ISR and connecting with user code.
including fulfill usb spec control transfer protocols via ep0 and user communications via other eps.

device class driver: perform class defined protocols and callbacks.

user code firmware: received data processing and connection with driver code.

host (windows driver): 

host side applications: 

more about usb:

USB device connected (plugged in): 
The hub is get notified by the presence of the USB device. The host begin the enumeration and set up the logic pipe from the host to device
(the logic pipe is the communication path from host to device)
Then the host will start send control transfer to get the device information and load the appropriate driver for the device.
and assign the device address.
Once driver is loaded, a session is established between the driver and the device. And now the usb is ready to use.

application will use windows API to communicate with driver and device. The configuration such as baudrate, check comm state will involve a series of control transfer on the device class level protocol.

usb communication uses data packet, the packet includes <pid, addr, ep#> and data. It needs a token packet for the start, and a ZLP (zero length packet) for the completion of transfer.)

VCP connection:
It will send 0x21/0x20 Set/Get_Line_Coding. This actually needs no changes if we are not querying the baudrate (since this is a virtual serial port)
0x22: set control line state.

CreateFile: just get the handle to the driver.
GetCommState: Get_Line_Coding 0x21 control transfer.
SetCommState: Set_Line_Coding 0x20 control transfer.
EscapeCommFunction: 0x22 set control line status
ClearCommError: only involves with host driver.
PurgeComm: only involves with host driver
FlushFileBuffers: only involves with host driver
WriteFile: trigger a bulk transfer and data is sent back to host driver buffer.
ReadFile: only involves with the driver.
CloseHandle: only involve with the host driver.


### OTG-FS (on the go full speed)
OTG: the peripheral can switch device/host on the fly
so it includes device stack and host stack.
Here we only discuss the device stack.

the usb device library is generic for all stm32 MCUs, only the HAL layer is adapted to each stm32 device.

We only focus on the CDC (Communication Device) using VCP (Virtual comm port)
VCP is from PSTN (public switched telephone network).

some abbreviations:
- FS/LS/HS: full speed, low speed, high speed
- MAC: media access controller
- OTG: On the go
- PFC: packet FIFO controller
- PHY: physical layer
- ADP: Attach detection protocol
- LPM: link power management
- BCD: Battery charging detector
- HNP: host negotiation protocol
- SRP: session request protocol
- PCD: Peripheral controller driver (Device mode)


### Library architecture

A generic USB communication stack:

usb hardware<-->
HAL/LL usb device driver<-->
usb device class driver (CDC)<-->
User Firmware <-->
Windows Usb/VCP driver<-->
Windows USB Application.

layer 1: core driver + class driver.
core driver:
- usb device core
  APIs to manage internal state machine
  callbacks to process interrupts
- usb requests: 
- usb io requests: handle low level IO requests
- log and debug

Class driver:
predefined class drivers which is linked to core driver using USBD_RegisterClass
- CDC, HID, DFU...

#### global data structure:
- USBD_FS_DeviceDesc: device information;
- FS_desc: the get descriptor callback functions
- hUSBDeviceFS: usbd handle, including status, callbacks et al
- USBD_Interface_fops_FS: CDC callbacks to connect user firmware.
- USBD_CDC: cdc registered callbacks.
- hpcd_USB_OTG_FS: handle for pcd device

```
  if (USBD_Init(&hUsbDeviceFS, &FS_Desc, DEVICE_FS) != USBD_OK)
  {
    Error_Handler();
  }
  if (USBD_RegisterClass(&hUsbDeviceFS, &USBD_CDC) != USBD_OK)
  {
    Error_Handler();
  }
  if (USBD_CDC_RegisterInterface(&hUsbDeviceFS, &USBD_Interface_fops_FS) != USBD_OK)
  {
    Error_Handler();
  }
  if (USBD_Start(&hUsbDeviceFS) != USBD_OK)
  {
    Error_Handler();
  }


```
//FS_Desc defines the function pointers for the device descriptor
```
USBD_DescriptorsTypeDef FS_Desc =
{
  USBD_FS_DeviceDescriptor
, USBD_FS_LangIDStrDescriptor
, USBD_FS_ManufacturerStrDescriptor
, USBD_FS_ProductStrDescriptor
, USBD_FS_SerialStrDescriptor
, USBD_FS_ConfigStrDescriptor
, USBD_FS_InterfaceStrDescriptor
#if (USBD_LPM_ENABLED == 1)
, USBD_FS_USR_BOSDescriptor
#endif /* (USBD_LPM_ENABLED == 1) */
};
```

USBD_RegisterClass will:
pdev->pClass=&USBD_CDC; //pClass pointer to the CDC driver.

The CDC driver includes a set of function pointers (callback):

```
USBD_ClassTypeDef  USBD_CDC =
{
  USBD_CDC_Init,
  USBD_CDC_DeInit,
  USBD_CDC_Setup,
  NULL,                 /* EP0_TxSent, */
  USBD_CDC_EP0_RxReady,
  USBD_CDC_DataIn,
  USBD_CDC_DataOut,
  NULL,
  NULL,
  NULL,
  USBD_CDC_GetHSCfgDesc,
  USBD_CDC_GetFSCfgDesc,
  USBD_CDC_GetOtherSpeedCfgDesc,
  USBD_CDC_GetDeviceQualifierDescriptor,
};
```
USBD_CDC_RegisterInterface
register the CDC interface callbacks
pdev->pUserData=&USBD_Interface_fops_FS;
```
USBD_CDC_ItfTypeDef USBD_Interface_fops_FS =
{
  CDC_Init_FS,
  CDC_DeInit_FS,
  CDC_Control_FS,
  CDC_Receive_FS
};
```
These 4 callback functions shall be implemented by the user.
Note: Transmit is not a callback.

hUSBDeviceFS is the following data structure:
```
typedef struct _USBD_HandleTypeDef
{
  uint8_t                 id;
  uint32_t                dev_config;
  uint32_t                dev_default_config;
  uint32_t                dev_config_status;
  USBD_SpeedTypeDef       dev_speed;
  USBD_EndpointTypeDef    ep_in[15]; //end point in 
  USBD_EndpointTypeDef    ep_out[15]; //end point out
  uint32_t                ep0_state;
  uint32_t                ep0_data_len;
  uint8_t                 dev_state;
  uint8_t                 dev_old_state;
  uint8_t                 dev_address;
  uint8_t                 dev_connection_status;
  uint8_t                 dev_test_mode;
  uint32_t                dev_remote_wakeup;

  USBD_SetupReqTypedef    request; //setup request
  USBD_DescriptorsTypeDef *pDesc; //descriptor callbacks point to FS_desc
  USBD_ClassTypeDef       *pClass; //cdc class callbacks,point to USBD_CDC
  void                    *pClassData; //pointer to USBD_CDC_HandleTypeDef
  void                    *pUserData; //pointer to USBD_Interface_fops_FS (user interface)
  void                    *pData;//pointer to the hpcd_USB_OTG_FS
} USBD_HandleTypeDef;
```
pClassData stores the tx/rx status for CDC class:
```
typedef struct
{
  uint32_t data[CDC_DATA_HS_MAX_PACKET_SIZE / 4U];      /* Force 32bits alignment */
  uint8_t  CmdOpCode;
  uint8_t  CmdLength;
  uint8_t  *RxBuffer;
  uint8_t  *TxBuffer;
  uint32_t RxLength;
  uint32_t TxLength;

  __IO uint32_t TxState;
  __IO uint32_t RxState;
}
USBD_CDC_HandleTypeDef;
```
dynamically allocated memory buffer and updated using user provided cdc functions.
for example SetTxBuffer, SetRxBuffer, Prepair_ReceivePacket et al.

The hUSBDeviceFS is the core data structure which includes all necessary information.

The hardware register status is in hpcd_USB_OTG_FS.instance.

hUSBDeviceFS (USBD_HandleTypeDef)
|_ep_in (USBD_EndpointTypeDef)
|    |_status,is_used,total_len,rem_len,max_packet_size
|_ep_out (USBD_EndpointTypeDef)
|	|_status,is_used,total_len,rem_len,max_packet_size
|_request (USBD_SetupReqTypedef)
|	|_bmRequest, bRequest, wValue, wIndex, wLength
|_pDesc=&Desc_FS (USBD_DescriptorsTypeDef)
|_pClass=&USBD_CDC (USBD_ClassTypeDef)
|       |_USBD_CDC_Init/DeInit
|		|_USBD_CDC_Setup
|		|_USBD_CDC_EP0_RxReady
|		|_USBD_CDC_DataIn/Out
|_pClassData=allocated memory (USBD_CDC_HandleTypeDef)
|       |_buffer for a packet-
|		|_rx buffer,tx buffer
|		|_rx size and tx size
|		|_rx state and tx state
|_pUserData=&USBD_Interface_fops_FS (user cdc interface)
|       |_CDC_Init/DeInit_FS
|		|_CDC_Control_FS
|		|_CDC_Receive_FS
|_pData=&hpcd_USB_OTG_FS (__PCD_HandleTypeDef)
|        |_Instance=&USB_basex (PCD_TypeDef, USB global registers, memory mapped)
|		|_Init (PCD_InitTypeDef)
|		|_USB_Address
|		|_In_ep (PCD_EPTypeDef)
|		    |_Ep_num, dir, is_stall,type
|			|_pid_start, even_odd_frame, tx_fifo_num
|			|_max_pack_size,xfer_buff,dma_addr,xfer_len,xfer_count
|		|_Out_ep (PCD_EPTypeDef)
|		|_Lock
|		|_state
|		|_error code
|		|_Setup buffer
|		|_LPM_sate,BESL,lpm_active,battery_charging_active
|		|_pdata=&hUSBDeviceFS (pointer to upper stack handler)
|		|_callback pointers
		
The process to initialize the hUsbDeviceFS:
- usbd_init: set the descriptor and callbacks
   |_set pClass to null.
   |_set pDesc
   |_default state
   |_id
   |_USBD_LL_Init: init the hpcd_USB_OTG_FS and link to hUSBDeviceFS
		|_hpcd_USB_OTG_FS.pData and hUSBDeviceFS.pdata linked to each other
		|_instance pointer to usb global register
		|_init structure
		|_HAL_PCD_Init: 
		|   |_HAL_PCD_MSPInit: enable clock, Pin configure, interrupt enable.
		|	|_PCD_BUSY (no communication allowed)
		|	|_Disable interrupts
		|	|_USB_CoreInit (reset the usb core)
		|	|_Init the in/out ep data structure
		|	|_USB_DevInit: Init USB device mode registers
		|	|_set address 0
		|	|_PCD_Ready: now able to communicate with host
		|	|_DevDisconnect
		|_SetRxFifo, SetTxFifo 0 and SetTxFifo 1
- usbd_RegisterClass: hang USBD_CDC to pClass.
- usbd_CDC_RegisterInterface: hang USBD_Interface_fops_FS to pUser.
- USBD_Start
	|_USBD_LL_Start
		|_HAL_PCD_Start: dynamically allocate pData
			|_USB_DevConnect
			|_HAL_PCD_Enable

now the USB device is up and everything is handled by the PCD Interrupt Handler.
The handler will drive the callback from low level to high level.
	
		
## low level driver: files
stm32l4xx_ll_usb.h/c lower level usb driver for the OTG.
stm32l4xx_hal_pcd.h/c lower level driver for PCD (Device mode)

stm32l4xx_ll_usb.h/c:
```
/* Initialization and de-initialization functions  ******************************/
HAL_StatusTypeDef HAL_Init(void);
HAL_StatusTypeDef HAL_DeInit(void);
void HAL_MspInit(void);
void HAL_MspDeInit(void);
HAL_StatusTypeDef HAL_InitTick (uint32_t TickPriority);

/* Peripheral Control functions  ************************************************/
void HAL_IncTick(void);
void HAL_Delay(uint32_t Delay);
uint32_t HAL_GetTick(void);
void HAL_SuspendTick(void);
void HAL_ResumeTick(void);
uint32_t HAL_GetHalVersion(void);
uint32_t HAL_GetREVID(void);
uint32_t HAL_GetDEVID(void);
uint32_t HAL_GetUIDw0(void);
uint32_t HAL_GetUIDw1(void);
uint32_t HAL_GetUIDw2(void);

/* DBGMCU Peripheral Control functions  *****************************************/
void HAL_DBGMCU_EnableDBGSleepMode(void);
void HAL_DBGMCU_DisableDBGSleepMode(void);
void HAL_DBGMCU_EnableDBGStopMode(void);
void HAL_DBGMCU_DisableDBGStopMode(void);
void HAL_DBGMCU_EnableDBGStandbyMode(void);
void HAL_DBGMCU_DisableDBGStandbyMode(void);

/* SYSCFG Control functions  ****************************************************/
void HAL_SYSCFG_SRAM2Erase(void);
void HAL_SYSCFG_EnableMemorySwappingBank(void);
void HAL_SYSCFG_DisableMemorySwappingBank(void);

#if defined(VREFBUF)
void HAL_SYSCFG_VREFBUF_VoltageScalingConfig(uint32_t VoltageScaling);
void HAL_SYSCFG_VREFBUF_HighImpedanceConfig(uint32_t Mode);
void HAL_SYSCFG_VREFBUF_TrimmingConfig(uint32_t TrimmingValue);
HAL_StatusTypeDef HAL_SYSCFG_EnableVREFBUF(void);
void HAL_SYSCFG_DisableVREFBUF(void);
#endif /* VREFBUF */

void HAL_SYSCFG_EnableIOAnalogSwitchBooster(void);
void HAL_SYSCFG_DisableIOAnalogSwitchBooster(void);
```
define the states and error codes
```
/**
  * @brief  USB Mode definition
  */
typedef enum
{
  USB_DEVICE_MODE  = 0,
  USB_HOST_MODE    = 1,
  USB_DRD_MODE     = 2
} USB_ModeTypeDef;

#if defined (USB_OTG_FS) || defined (USB_OTG_HS)
/**
  * @brief  URB States definition
  */
typedef enum
{
  URB_IDLE = 0,
  URB_DONE,
  URB_NOTREADY,
  URB_NYET,
  URB_ERROR,
  URB_STALL
} USB_OTG_URBStateTypeDef;

/**
  * @brief  Host channel States  definition
  */
typedef enum
{
  HC_IDLE = 0,
  HC_XFRC,
  HC_HALTED,
  HC_NAK,
  HC_NYET,
  HC_STALL,
  HC_XACTERR,
  HC_BBLERR,
  HC_DATATGLERR
} USB_OTG_HCStateTypeDef;

```

data structures:

otg_cfg:
```
typedef struct
{
  uint32_t dev_endpoints;        /*!< Device Endpoints number.
                                      This parameter depends on the used USB core.
                                      This parameter must be a number between Min_Data = 1 and Max_Data = 15 */

  uint32_t Host_channels;        /*!< Host Channels number.
                                      This parameter Depends on the used USB core.
                                      This parameter must be a number between Min_Data = 1 and Max_Data = 15 */

  uint32_t speed;                /*!< USB Core speed.
                                      This parameter can be any value of @ref USB_Core_Speed_                */

  uint32_t dma_enable;           /*!< Enable or disable of the USB embedded DMA used only for OTG HS.                             */

  uint32_t ep0_mps;              /*!< Set the Endpoint 0 Max Packet size.
                                      This parameter can be any value of @ref USB_EP0_MPS_                   */

  uint32_t phy_itface;           /*!< Select the used PHY interface.
                                      This parameter can be any value of @ref USB_Core_PHY_                  */

  uint32_t Sof_enable;           /*!< Enable or disable the output of the SOF signal.                        */

  uint32_t low_power_enable;     /*!< Enable or disable the low power mode.                                  */

  uint32_t lpm_enable;           /*!< Enable or disable Link Power Management.                               */

  uint32_t battery_charging_enable; /*!< Enable or disable Battery charging.                                 */

  uint32_t vbus_sensing_enable;  /*!< Enable or disable the VBUS Sensing feature.                            */

  uint32_t use_dedicated_ep1;    /*!< Enable or disable the use of the dedicated EP1 interrupt.              */

  uint32_t use_external_vbus;    /*!< Enable or disable the use of the external VBUS.                        */
} USB_OTG_CfgTypeDef;
```
End point data structure:
```
typedef struct
{
  uint8_t   num;            /*!< Endpoint number
                                This parameter must be a number between Min_Data = 1 and Max_Data = 15    */

  uint8_t   is_in;          /*!< Endpoint direction
                                This parameter must be a number between Min_Data = 0 and Max_Data = 1     */

  uint8_t   is_stall;       /*!< Endpoint stall condition
                                This parameter must be a number between Min_Data = 0 and Max_Data = 1     */

  uint8_t   type;           /*!< Endpoint type
                                 This parameter can be any value of @ref USB_EP_Type_                     */

  uint8_t   data_pid_start; /*!< Initial data PID
                                This parameter must be a number between Min_Data = 0 and Max_Data = 1     */

  uint8_t   even_odd_frame; /*!< IFrame parity
                                 This parameter must be a number between Min_Data = 0 and Max_Data = 1    */

  uint16_t  tx_fifo_num;    /*!< Transmission FIFO number
                                 This parameter must be a number between Min_Data = 1 and Max_Data = 15   */

  uint32_t  maxpacket;      /*!< Endpoint Max packet size
                                 This parameter must be a number between Min_Data = 0 and Max_Data = 64KB */

  uint8_t   *xfer_buff;     /*!< Pointer to transfer buffer                                               */

  uint32_t  dma_addr;       /*!< 32 bits aligned transfer buffer address                                  */

  uint32_t  xfer_len;       /*!< Current transfer length                                                  */

  uint32_t  xfer_count;     /*!< Partial transfer length in case of multi packet transfer                 */
} USB_OTG_EPTypeDef;
```

stm32l4xx_hal_pcd.h/c
the device mode (peripheral controller device) driver:

state machine:
```
/**
  * @brief  PCD State structure definition
  */
typedef enum
{
  HAL_PCD_STATE_RESET   = 0x00,
  HAL_PCD_STATE_READY   = 0x01,
  HAL_PCD_STATE_ERROR   = 0x02,
  HAL_PCD_STATE_BUSY    = 0x03,
  HAL_PCD_STATE_TIMEOUT = 0x04
} PCD_StateTypeDef;

/* Device LPM suspend state */
typedef enum
{
  LPM_L0 = 0x00, /* on */
  LPM_L1 = 0x01, /* LPM L1 sleep */
  LPM_L2 = 0x02, /* suspend */
  LPM_L3 = 0x03, /* off */
} PCD_LPM_StateTypeDef;

typedef enum
{
  PCD_LPM_L0_ACTIVE = 0x00, /* on */
  PCD_LPM_L1_ACTIVE = 0x01, /* LPM L1 sleep */
} PCD_LPM_MsgTypeDef;

typedef enum
{
  PCD_BCD_ERROR                     = 0xFF,
  PCD_BCD_CONTACT_DETECTION         = 0xFE,
  PCD_BCD_STD_DOWNSTREAM_PORT       = 0xFD,
  PCD_BCD_CHARGING_DOWNSTREAM_PORT  = 0xFC,
  PCD_BCD_DEDICATED_CHARGING_PORT   = 0xFB,
  PCD_BCD_DISCOVERY_COMPLETED       = 0x00,

} PCD_BCD_MsgTypeDef;
```

__PCD_HandleTypeDef:
```
typedef struct __PCD_HandleTypeDef
{
  PCD_TypeDef             *Instance;   /*!< Register base address              */
  PCD_InitTypeDef         Init;        /*!< PCD required parameters            */
  __IO uint8_t            USB_Address; /*!< USB Address                        */
  PCD_EPTypeDef           IN_ep[16];   /*!< IN endpoint parameters             */
  PCD_EPTypeDef           OUT_ep[16];  /*!< OUT endpoint parameters            */
  HAL_LockTypeDef         Lock;        /*!< PCD peripheral status              */
  __IO PCD_StateTypeDef   State;       /*!< PCD communication state            */
  __IO  uint32_t          ErrorCode;   /*!< PCD Error code                     */
  uint32_t                Setup[12];   /*!< Setup packet buffer                */
  PCD_LPM_StateTypeDef    LPM_State;   /*!< LPM State                          */
  uint32_t                BESL;


  uint32_t lpm_active;                 /*!< Enable or disable the Link Power Management .
                                       This parameter can be set to ENABLE or DISABLE        */

  uint32_t battery_charging_active;    /*!< Enable or disable Battery charging.
                                       This parameter can be set to ENABLE or DISABLE        */
  void                    *pData;      /*!< Pointer to upper stack Handler */
    void (* SOFCallback)(struct __PCD_HandleTypeDef *hpcd);                              /*!< USB OTG PCD SOF callback                */
  void (* SetupStageCallback)(struct __PCD_HandleTypeDef *hpcd);                       /*!< USB OTG PCD Setup Stage callback        */
  void (* ResetCallback)(struct __PCD_HandleTypeDef *hpcd);                            /*!< USB OTG PCD Reset callback              */
  void (* SuspendCallback)(struct __PCD_HandleTypeDef *hpcd);                          /*!< USB OTG PCD Suspend callback            */
  void (* ResumeCallback)(struct __PCD_HandleTypeDef *hpcd);                           /*!< USB OTG PCD Resume callback             */
  void (* ConnectCallback)(struct __PCD_HandleTypeDef *hpcd);                          /*!< USB OTG PCD Connect callback            */
  void (* DisconnectCallback)(struct __PCD_HandleTypeDef *hpcd);                       /*!< USB OTG PCD Disconnect callback         */

  void (* DataOutStageCallback)(struct __PCD_HandleTypeDef *hpcd, uint8_t epnum);      /*!< USB OTG PCD Data OUT Stage callback     */
  void (* DataInStageCallback)(struct __PCD_HandleTypeDef *hpcd, uint8_t epnum);       /*!< USB OTG PCD Data IN Stage callback      */
  void (* ISOOUTIncompleteCallback)(struct __PCD_HandleTypeDef *hpcd, uint8_t epnum);  /*!< USB OTG PCD ISO OUT Incomplete callback */
  void (* ISOINIncompleteCallback)(struct __PCD_HandleTypeDef *hpcd, uint8_t epnum);   /*!< USB OTG PCD ISO IN Incomplete callback  */
  void (* BCDCallback)(struct __PCD_HandleTypeDef *hpcd, PCD_BCD_MsgTypeDef msg);      /*!< USB OTG PCD BCD callback                */
  void (* LPMCallback)(struct __PCD_HandleTypeDef *hpcd, PCD_LPM_MsgTypeDef msg);      /*!< USB OTG PCD LPM callback                */

  void (* MspInitCallback)(struct __PCD_HandleTypeDef *hpcd);                          /*!< USB OTG PCD Msp Init callback           */
  void (* MspDeInitCallback)(struct __PCD_HandleTypeDef *hpcd);        /*!< USB OTG PCD Msp DeInit callback         */
  } PCD_HandleTypeDef;

HAL_StatusTypeDef HAL_PCD_DevConnect(PCD_HandleTypeDef *hpcd);
HAL_StatusTypeDef HAL_PCD_DevDisconnect(PCD_HandleTypeDef *hpcd);
HAL_StatusTypeDef HAL_PCD_SetAddress(PCD_HandleTypeDef *hpcd, uint8_t address);
HAL_StatusTypeDef HAL_PCD_EP_Open(PCD_HandleTypeDef *hpcd, uint8_t ep_addr, uint16_t ep_mps, uint8_t ep_type);
HAL_StatusTypeDef HAL_PCD_EP_Close(PCD_HandleTypeDef *hpcd, uint8_t ep_addr);
HAL_StatusTypeDef HAL_PCD_EP_Receive(PCD_HandleTypeDef *hpcd, uint8_t ep_addr, uint8_t *pBuf, uint32_t len);
HAL_StatusTypeDef HAL_PCD_EP_Transmit(PCD_HandleTypeDef *hpcd, uint8_t ep_addr, uint8_t *pBuf, uint32_t len);
uint16_t          HAL_PCD_EP_GetRxCount(PCD_HandleTypeDef *hpcd, uint8_t ep_addr);
HAL_StatusTypeDef HAL_PCD_EP_SetStall(PCD_HandleTypeDef *hpcd, uint8_t ep_addr);
HAL_StatusTypeDef HAL_PCD_EP_ClrStall(PCD_HandleTypeDef *hpcd, uint8_t ep_addr);
HAL_StatusTypeDef HAL_PCD_EP_Flush(PCD_HandleTypeDef *hpcd, uint8_t ep_addr);
HAL_StatusTypeDef HAL_PCD_ActivateRemoteWakeup(PCD_HandleTypeDef *hpcd);
HAL_StatusTypeDef HAL_PCD_DeActivateRemoteWakeup(PCD_HandleTypeDef *hpcd);  
  ```

#### program API
### configure USB driver
- device init: HAL_PCD_Init
- End point configuration
  HAL_PCD_EP_Open
  HAL_PCD_EP_Close
- device core--USBD_HandleTypeDef
  the handle keeps all information in realtime as well as the state machine and endpoint information
  EP0 state:
	  0 idle
	  1 setup
	  2 data_in
	  3 data_out
	  4 status_in
	  5 status_out
	  6 stall
  Device state:
	  1 default
	  2 addressed
	  3 configured
	  4 suspended
	  note: the usb spec defines 6 states: attached, powered, default,addressed, configured, suspended.
   when there is problem try to exam this first.
- data transfer
  HAL_PCD_EP_Transmit
  HAL_PCD_EP_Receive
  HAL_PCD_EP_SetStall
  HAL_PCD_EP_ClrStall
  HAL_PCD_EP_Flush
  HAL_PCD_IRQHandler
- handling control endpoint (EP0)
  usb transfer: control, interrupt, bulk, isochronous
  (isochronous: equal time interval, 等时的)
  control packet: standard, class specific, vendor specific
  control packet->setup packet.
  EP0 receives the packet and process in interrupt and update the state machine.
  setup packet are 8 bytes fixed.
  ```
typedef  struct  usb_setup_req
{
    uint8_t   bmRequest;
    uint8_t   bRequest;
    uint16_t  wValue;
    uint16_t  wIndex;
    uint16_t  wLength;
}USBD_SetupReqTypedef;
  ```
  standard requests:
  Get_Status: device/interface/ep0 out/ep0 in/ep.
  Clear_Feature: clear the device remote wakeup feature/clear stall of EP. (not include EP0)
  Set_Feature: opposite to above
  Set_address: set device address
  Get_Decriptor: device/config/string/
  Get_configuration
  set_configuration
  get_interface
  set_interface
  
  non standard requests:
  all non standard requests are sent to class code to process
  setup req ->pdev->pClass->Setup(pdev,req)
  data stage: USBD_CtlSendData, USBD_CtlPrepareRx.
  status stage   pdev->pClass->Setup(pdev,req)
  
- non control EP transfer
  using in and out stage callback
  interrupt mode
  

#### OTG registers  

GOTGCTL: OTG global control register
GOTGINT: OTG global interrupt register
GAHBCFG: AHB config register
GUSBCFG: USB config register
GRSTCTL: reset control register
GINTSTS: core interrupt status register
GINTMSK: interrupt mask register
GRXSTSR: receive status debug read register
GRXSTSP: receive status debug pop register
GRXFSIZ: receive FIFO size register
GCCFG: general core config register
CID: core ID register
GLPMCFG: core LPM config register
GPWRDN: powerdown register
GADPCTL: adp tmer control/status register
DIEPTXFx: device IN endpoint x=1 to 5

Device mode CSR map (control/status register)
DCFG: device config register
DCTL: device control register
DSTS: device status register
DIEPMSK: device IN endpoint common interrupt mask register
DOEPMSK: device OUT endpoint common interrupt mask reg
DAINT: device all endpoint interrupt
DAINTMSK: device all endpoint interrupt mask
DVBUSDIS: device VBUS discharge timer reg
DVBUSPULSE: device vbus pulsing timer reg.
DIEPEMPMSK: device IN endpoint FIFO empty interrupt mask
DIEPCTL0: device IN endpoint 0 control reg.
DIEPCTLx: x=1:5
DIEPINTx: device IN endpoint x interrupt reg. x=0-5
DIEPTSIZ0: device IN endpoint 0 transfer size reg.
DIEPTSIZx: x=1-5
DOEPCTL0: device OUT endpoint 0 control reg.
DOEPCTLx: x=1-5
DOEPINTx
DOEPTSIZ0:
DOEPTSIZx:

#### USB isr: HAL_PCD_IRQHandler
The ISR is the heart of the USB device.
It maintains the state machine and process the transfer completed events.
The interrupt begins after the device is up.
First handshaking including:
enumeration
get descriptor
assign address
configure the device
bulk transfer.
connect and disconnection

device mode:
- invalid: quit
- MMIS: mode mismatch: clear
- OEPINT:
   - DAINT & DAINTMSK
   - high 16 bit
   - loop check all OUT ep
     1. check if EPx intterupt is set
	 2. DOEPINT & DOEPMSK
	 3. if XFRC (transfer completed interrupt)
	    clear XFRC
		clear DOEPINT0 bit 15
		clear DOEPINT0 bit 5
		check STUP (setup phase done), clear DOEPINTx bit 15 and STUP
		check OTEPDIS (OUT token received when endpoint disabled): clear it
		DataOutStageCallback, this is where to interface to application.
		
- IEPINT:
	- DAINT & DAINTMSK
	- loop over all IN ep.
		1. check if EPx interrupt
		2. DIEPINT & DIEPMSK
		3. check XFRC
		   clear fifo empty mask
		   clear xfrc bit
		   DataInStageCallback (empty)
		   check TOC (Timeout Condition), clear it
		   check ITXFE (IN token received when Tx FIFO is empty) clear it
		   check INEPNE (In token received with EP mistmatch)
		   check EPDISD (EP disabled), clear it
		   check TXFE (Transmit Fifo empty), clear it.
- WKUINT (resume/remote wakeup detection interrupt)
	- clear DCTL RWUSIG (Remote wakeup signalling),
	- LPM callback
	- claer WKUINT
- USBSUSP (usb suspend): 
	- suspend callback
	- clear flag
- LPMINT: LPM interrupt
- USBRST: USB reset
	- clear RWUSIG
	- flushTxFifo
	- init INEP, OUTEP (Ctrl and INT, MSK
	- set default address
	- EP0 to receive setup packets USB_EP0_OutStart
	- clear flag
- ENUMDNE: Enumeration done.
	- active setup
	- init hpcd env.
	- depending on HCLK, set the TRDT (turn around time) in USBCFG 
	- resetCallback
	- clear flag
- RXFLVL: Rx Fifo not empty
	- mask RXFLVL
	- get GRXSTSP (pop register)
	- get ep num from pop reg.
	- get PKTSTS (packet status) from pop reg. (0001, OUT NAK, 0010: out packet received, 0011: out transfer completed. 0100 setup completed. 0110 setup packet received)
	  - 0010: STS_DATA_UPDT
	    get BCNT (Bytes count 11 bits, max 2048 bytes) in the packet
	    read packet to ep->xfer_buff
	  - 0110: setup packet received
	    read packet
	- unmask.
SOF: start of frame
...
IISOIXFR: Incomplete ISO IN transfer.
...
INCOMPISOOUT: Incomplete ISO OUT transfer.
...
SRQINT: session request/new session detected interrupt
	- connection callback
	- clear flag
OTGINT: OTG interrupt, indicates an OTG protocol event.
	- check GOTGINT:
	- check SEDET bit (Session End detected)
	- disconnection callback

pay attention to:
DataInStageCallback
DataOutStageCallback
SetupStageCallback

setup stage:
mcu driver must respond host's requests
including
- get descriptors (enumeration stage to find the driver for it).
USBD_FS_DeviceDescriptor
USBD_FS_LangIDStrDescriptor
USBD_FS_ProductStrDescriptor
USBD_FS_ManufacturerStrDescriptor
USBD_FS_SerialStrDescriptor
USBD_FS_ConfigStrDescriptor
USBD_FS_InterfaceStrDescriptor
USBD_FS_USR_BOSDescriptor
Get_SerialNum

- enumeration
depending on the hclk used, it sets TRDT (turn around time)
setup packets process has time limit requirement. 5 seconds.
debugging with care.

usbd_core.h/c: handle all usb communication and state machine
usbd_req.h/c: request implementation in usb2.0 spec chapter 9 (not found, it is actually usbd_ctlreq.h/c)
usbd_ctlreq.h/c: handle the results of usb transaction
//usbd_conf_template.h/c: template file
usbd_def.h/c: common definition for usb.
these files are all for setup package process.

usbd_cd.h/c used for data processing for non-ep0 data process.

they are middlewares built on top of the lower level HAL drivers.

## The stm32l476xx USB driver (PCD+CDC): 

low level driver
|_stm32l4xx_ll_usb.h/c: low level usb register operations
|_stm32l4xx_hal_pcd.h/c: HAL level PCD (peripheral mode) drivers. built on top of LL.
|_stm32l4xx_hal_pcd_ex.h/c: extension module.

dependency:
|_ system_stm32l4xx.h.
|_ stm32l4xx.h.
|_ stm32l476xx,h.
    |_ USB_OTG_GlobalTypeDef: bind registers and memory layouts.
	|_ USB_OTG_DeviceTypeDef
	|_ USB_OTG_INEndpointTypeDef
	|_ USB_OTG_OUTEndpointTypeDef
	|_ USB_OTG_HostTypeDef
	|_ USB_OTG_HostChannelTypeDef

middleware driver
|_core
|    |_usbd_core.h/c: all functions for usb transactions
|    |_usbd_ctlreq.h/c: 
|    |_usbd_ioreq.h/c: 
|	 |_usbd_def.h
|_class
     |_usbd_cdc.h/c
The middleware mainly:
- process the usb protocol (high level setup packets)
- provide cdc class - application interface.

### stm32l4xx_ll_usb.h/c
|_ USB_ModeTypeDef: define the USB mode: device, host, DRD
|_ USB_OTG_URBStateTypeDef: URB state definition
|_ USB_OTG_HCStateTypeDef: host channel state definition
|_ USB_OTG_CfgTypeDef: USB OTG Initialization Structure definition
|_ USB_OTG_EPTypeDef: usb endpoint data structure.
|_ USB_OTG_HCTypeDef: usb otg host channel
|_ register layout (defined in stm32l476xx.h)
```
#define USB_OTG_FS_PERIPH_BASE               (0x50000000UL)

#define USB_OTG_GLOBAL_BASE                  (0x00000000UL)
#define USB_OTG_DEVICE_BASE                  (0x00000800UL)
#define USB_OTG_IN_ENDPOINT_BASE             (0x00000900UL)
#define USB_OTG_OUT_ENDPOINT_BASE            (0x00000B00UL)
#define USB_OTG_EP_REG_SIZE                  (0x00000020UL)
#define USB_OTG_HOST_BASE                    (0x00000400UL)
#define USB_OTG_HOST_PORT_BASE               (0x00000440UL)
#define USB_OTG_HOST_CHANNEL_BASE            (0x00000500UL)
#define USB_OTG_HOST_CHANNEL_SIZE            (0x00000020UL)
#define USB_OTG_PCGCCTL_BASE                 (0x00000E00UL)
#define USB_OTG_FIFO_BASE                    (0x00001000UL)
#define USB_OTG_FIFO_SIZE                    (0x00001000UL)

#define USBx_PCGCCTL    *(__IO uint32_t *)((uint32_t)USBx_BASE + USB_OTG_PCGCCTL_BASE)
//note USBx_BASE is not defined, we need define it before we use these registers.
```
|_ macros: mask/unmask, clear interrupt flag..
|_ functions: 
```
HAL_StatusTypeDef USB_CoreInit(USB_OTG_GlobalTypeDef *USBx, USB_OTG_CfgTypeDef Init);
HAL_StatusTypeDef USB_DevInit(USB_OTG_GlobalTypeDef *USBx, USB_OTG_CfgTypeDef Init);
HAL_StatusTypeDef USB_EnableGlobalInt(USB_OTG_GlobalTypeDef *USBx);
HAL_StatusTypeDef USB_DisableGlobalInt(USB_OTG_GlobalTypeDef *USBx);
HAL_StatusTypeDef USB_SetCurrentMode(USB_OTG_GlobalTypeDef *USBx, USB_ModeTypeDef mode);
HAL_StatusTypeDef USB_SetDevSpeed(USB_OTG_GlobalTypeDef *USBx, uint8_t speed);
HAL_StatusTypeDef USB_FlushRxFifo(USB_OTG_GlobalTypeDef *USBx);
HAL_StatusTypeDef USB_FlushTxFifo(USB_OTG_GlobalTypeDef *USBx, uint32_t num);
HAL_StatusTypeDef USB_ActivateEndpoint(USB_OTG_GlobalTypeDef *USBx, USB_OTG_EPTypeDef *ep);
HAL_StatusTypeDef USB_DeactivateEndpoint(USB_OTG_GlobalTypeDef *USBx, USB_OTG_EPTypeDef *ep);
HAL_StatusTypeDef USB_ActivateDedicatedEndpoint(USB_OTG_GlobalTypeDef *USBx, USB_OTG_EPTypeDef *ep);
HAL_StatusTypeDef USB_DeactivateDedicatedEndpoint(USB_OTG_GlobalTypeDef *USBx, USB_OTG_EPTypeDef *ep);
HAL_StatusTypeDef USB_EPStartXfer(USB_OTG_GlobalTypeDef *USBx, USB_OTG_EPTypeDef *ep);
HAL_StatusTypeDef USB_EP0StartXfer(USB_OTG_GlobalTypeDef *USBx, USB_OTG_EPTypeDef *ep);
HAL_StatusTypeDef USB_WritePacket(USB_OTG_GlobalTypeDef *USBx, uint8_t *src, uint8_t ch_ep_num, uint16_t len);
void             *USB_ReadPacket(USB_OTG_GlobalTypeDef *USBx, uint8_t *dest, uint16_t len);
HAL_StatusTypeDef USB_EPSetStall(USB_OTG_GlobalTypeDef *USBx, USB_OTG_EPTypeDef *ep);
HAL_StatusTypeDef USB_EPClearStall(USB_OTG_GlobalTypeDef *USBx, USB_OTG_EPTypeDef *ep);
HAL_StatusTypeDef USB_SetDevAddress(USB_OTG_GlobalTypeDef *USBx, uint8_t address);
HAL_StatusTypeDef USB_DevConnect(USB_OTG_GlobalTypeDef *USBx);
HAL_StatusTypeDef USB_DevDisconnect(USB_OTG_GlobalTypeDef *USBx);
HAL_StatusTypeDef USB_StopDevice(USB_OTG_GlobalTypeDef *USBx);
HAL_StatusTypeDef USB_ActivateSetup(USB_OTG_GlobalTypeDef *USBx);
HAL_StatusTypeDef USB_EP0_OutStart(USB_OTG_GlobalTypeDef *USBx, uint8_t *psetup);
uint8_t           USB_GetDevSpeed(USB_OTG_GlobalTypeDef *USBx);
uint32_t          USB_GetMode(USB_OTG_GlobalTypeDef *USBx);
uint32_t          USB_ReadInterrupts(USB_OTG_GlobalTypeDef *USBx);
uint32_t          USB_ReadDevAllOutEpInterrupt(USB_OTG_GlobalTypeDef *USBx);
uint32_t          USB_ReadDevOutEPInterrupt(USB_OTG_GlobalTypeDef *USBx, uint8_t epnum);
uint32_t          USB_ReadDevAllInEpInterrupt(USB_OTG_GlobalTypeDef *USBx);
uint32_t          USB_ReadDevInEPInterrupt(USB_OTG_GlobalTypeDef *USBx, uint8_t epnum);
void              USB_ClearInterrupts(USB_OTG_GlobalTypeDef *USBx, uint32_t interrupt);

HAL_StatusTypeDef USB_HostInit(USB_OTG_GlobalTypeDef *USBx, USB_OTG_CfgTypeDef cfg);
HAL_StatusTypeDef USB_InitFSLSPClkSel(USB_OTG_GlobalTypeDef *USBx, uint8_t freq);
HAL_StatusTypeDef USB_ResetPort(USB_OTG_GlobalTypeDef *USBx);
HAL_StatusTypeDef USB_DriveVbus(USB_OTG_GlobalTypeDef *USBx, uint8_t state);
uint32_t          USB_GetHostSpeed(USB_OTG_GlobalTypeDef *USBx);
uint32_t          USB_GetCurrentFrame(USB_OTG_GlobalTypeDef *USBx);
HAL_StatusTypeDef USB_HC_Init(USB_OTG_GlobalTypeDef *USBx,
                              uint8_t ch_num,
                              uint8_t epnum,
                              uint8_t dev_address,
                              uint8_t speed,
                              uint8_t ep_type,
                              uint16_t mps);
HAL_StatusTypeDef USB_HC_StartXfer(USB_OTG_GlobalTypeDef *USBx, USB_OTG_HCTypeDef *hc);
uint32_t          USB_HC_ReadInterrupt(USB_OTG_GlobalTypeDef *USBx);
HAL_StatusTypeDef USB_HC_Halt(USB_OTG_GlobalTypeDef *USBx, uint8_t hc_num);
HAL_StatusTypeDef USB_DoPing(USB_OTG_GlobalTypeDef *USBx, uint8_t ch_num);
HAL_StatusTypeDef USB_StopHost(USB_OTG_GlobalTypeDef *USBx);
HAL_StatusTypeDef USB_ActivateRemoteWakeup(USB_OTG_GlobalTypeDef *USBx);
HAL_StatusTypeDef USB_DeActivateRemoteWakeup(USB_OTG_GlobalTypeDef *USBx);
```

### stm32l4xx_hal_pcd.h/c
built on top of stm32l4xx_ll_usb.h/c
specialized on PCD mode.
enumerations:
|_ PCD_StateTypeDef: reset/ready/error/busy/timeout
|_ PCD_LPM_StateTypeDef
|_ PCD_LPM_MsgTypeDef
|_ PCD_BCD_MsgTypeDef: (BCD: binary-coded decimal), msg is in bcd format.

data structures:
|_ PCD_TypeDef from stm32l476xx.h.
|_ PCD_InitTypeDef from stm32l476xx.h.
|_ PCD_EPTypeDef from stm32l476xx.h.
|_ PCD_HandleTypeDef define status and callbacks

macros:
__HAL_PCD_ENABLE
__HAL_PCD_DISABLE
__HAL_PCD_GET_FLAG
__HAL_PCD_CLEAR_FLAG
__HAL_PCD_IS_INVALID_INTERRUPT
__HAL_PCD_UNGATE_PHYCLOCK
__HAL_PCD_GATE_PHYCLOCK
__HAL_PCD_IS_PHY_SUSPENDED
__HAL_USB_OTG_FS_WAKEUP_EXTI_ENABLE_IT
__HAL_USB_OTG_FS_WAKEUP_EXTI_DISABLE_IT
__HAL_USB_OTG_FS_WAKEUP_EXTI_GET_FLAG
__HAL_USB_OTG_FS_WAKEUP_EXTI_CLEAR_FLAG
__HAL_USB_OTG_FS_WAKEUP_EXTI_ENABLE_RISING_EDGE

functions:
```
//initialization de-init functions
HAL_StatusTypeDef HAL_PCD_Init(PCD_HandleTypeDef *hpcd);
HAL_StatusTypeDef HAL_PCD_DeInit(PCD_HandleTypeDef *hpcd);
void HAL_PCD_MspInit(PCD_HandleTypeDef *hpcd);
void HAL_PCD_MspDeInit(PCD_HandleTypeDef *hpcd);HAL_StatusTypeDef HAL_PCD_Start(PCD_HandleTypeDef *hpcd);

//IO operations: non-blocking mode, interrupt mode.
HAL_StatusTypeDef HAL_PCD_Stop(PCD_HandleTypeDef *hpcd);

void HAL_PCD_IRQHandler(PCD_HandleTypeDef *hpcd);//this is the core function.

void HAL_PCD_SOFCallback(PCD_HandleTypeDef *hpcd);
void HAL_PCD_SetupStageCallback(PCD_HandleTypeDef *hpcd);
void HAL_PCD_ResetCallback(PCD_HandleTypeDef *hpcd);
void HAL_PCD_SuspendCallback(PCD_HandleTypeDef *hpcd);
void HAL_PCD_ResumeCallback(PCD_HandleTypeDef *hpcd);
void HAL_PCD_ConnectCallback(PCD_HandleTypeDef *hpcd);
void HAL_PCD_DisconnectCallback(PCD_HandleTypeDef *hpcd);

void HAL_PCD_DataOutStageCallback(PCD_HandleTypeDef *hpcd, uint8_t epnum);
void HAL_PCD_DataInStageCallback(PCD_HandleTypeDef *hpcd, uint8_t epnum);
void HAL_PCD_ISOOUTIncompleteCallback(PCD_HandleTypeDef *hpcd, uint8_t epnum);
void HAL_PCD_ISOINIncompleteCallback(PCD_HandleTypeDef *hpcd, uint8_t epnum);

//PCD control functions.
HAL_StatusTypeDef HAL_PCD_DevConnect(PCD_HandleTypeDef *hpcd);
HAL_StatusTypeDef HAL_PCD_DevDisconnect(PCD_HandleTypeDef *hpcd);
HAL_StatusTypeDef HAL_PCD_SetAddress(PCD_HandleTypeDef *hpcd, uint8_t address);
HAL_StatusTypeDef HAL_PCD_EP_Open(PCD_HandleTypeDef *hpcd, uint8_t ep_addr, uint16_t ep_mps, uint8_t ep_type);
HAL_StatusTypeDef HAL_PCD_EP_Close(PCD_HandleTypeDef *hpcd, uint8_t ep_addr);
HAL_StatusTypeDef HAL_PCD_EP_Receive(PCD_HandleTypeDef *hpcd, uint8_t ep_addr, uint8_t *pBuf, uint32_t len);
HAL_StatusTypeDef HAL_PCD_EP_Transmit(PCD_HandleTypeDef *hpcd, uint8_t ep_addr, uint8_t *pBuf, uint32_t len);
uint16_t          HAL_PCD_EP_GetRxCount(PCD_HandleTypeDef *hpcd, uint8_t ep_addr);
HAL_StatusTypeDef HAL_PCD_EP_SetStall(PCD_HandleTypeDef *hpcd, uint8_t ep_addr);
HAL_StatusTypeDef HAL_PCD_EP_ClrStall(PCD_HandleTypeDef *hpcd, uint8_t ep_addr);
HAL_StatusTypeDef HAL_PCD_EP_Flush(PCD_HandleTypeDef *hpcd, uint8_t ep_addr);
HAL_StatusTypeDef HAL_PCD_ActivateRemoteWakeup(PCD_HandleTypeDef *hpcd);
HAL_StatusTypeDef HAL_PCD_DeActivateRemoteWakeup(PCD_HandleTypeDef *hpcd);
```

### middleware
usbd_def.h
- constants
- data structures
  |_USBD_SetupReqTypedef
  |_USBD_ClassTypeDef: callbacks
  |_USBD_DescriptorsTypeDef: callbacks
  |_USBD_EndpointTypeDef
  |_USBD_HandleTypeDef
usbd_core.h/c
```
USBD_StatusTypeDef USBD_Init(USBD_HandleTypeDef *pdev, USBD_DescriptorsTypeDef *pdesc, uint8_t id);
USBD_StatusTypeDef USBD_DeInit(USBD_HandleTypeDef *pdev);
USBD_StatusTypeDef USBD_Start  (USBD_HandleTypeDef *pdev);
USBD_StatusTypeDef USBD_Stop   (USBD_HandleTypeDef *pdev);
USBD_StatusTypeDef USBD_RegisterClass(USBD_HandleTypeDef *pdev, USBD_ClassTypeDef *pclass);

USBD_StatusTypeDef USBD_RunTestMode (USBD_HandleTypeDef  *pdev);
USBD_StatusTypeDef USBD_SetClassConfig(USBD_HandleTypeDef  *pdev, uint8_t cfgidx);
USBD_StatusTypeDef USBD_ClrClassConfig(USBD_HandleTypeDef  *pdev, uint8_t cfgidx);

USBD_StatusTypeDef USBD_LL_SetupStage(USBD_HandleTypeDef *pdev, uint8_t *psetup);
USBD_StatusTypeDef USBD_LL_DataOutStage(USBD_HandleTypeDef *pdev , uint8_t epnum, uint8_t *pdata);
USBD_StatusTypeDef USBD_LL_DataInStage(USBD_HandleTypeDef *pdev , uint8_t epnum, uint8_t *pdata);

USBD_StatusTypeDef USBD_LL_Reset(USBD_HandleTypeDef  *pdev);
USBD_StatusTypeDef USBD_LL_SetSpeed(USBD_HandleTypeDef  *pdev, USBD_SpeedTypeDef speed);
USBD_StatusTypeDef USBD_LL_Suspend(USBD_HandleTypeDef  *pdev);
USBD_StatusTypeDef USBD_LL_Resume(USBD_HandleTypeDef  *pdev);

USBD_StatusTypeDef USBD_LL_SOF(USBD_HandleTypeDef  *pdev);
USBD_StatusTypeDef USBD_LL_IsoINIncomplete(USBD_HandleTypeDef  *pdev, uint8_t epnum);
USBD_StatusTypeDef USBD_LL_IsoOUTIncomplete(USBD_HandleTypeDef  *pdev, uint8_t epnum);

USBD_StatusTypeDef USBD_LL_DevConnected(USBD_HandleTypeDef  *pdev);
USBD_StatusTypeDef USBD_LL_DevDisconnected(USBD_HandleTypeDef  *pdev);

/* USBD Low Level Driver */
USBD_StatusTypeDef  USBD_LL_Init (USBD_HandleTypeDef *pdev);
USBD_StatusTypeDef  USBD_LL_DeInit (USBD_HandleTypeDef *pdev);
USBD_StatusTypeDef  USBD_LL_Start(USBD_HandleTypeDef *pdev);
USBD_StatusTypeDef  USBD_LL_Stop (USBD_HandleTypeDef *pdev);
USBD_StatusTypeDef  USBD_LL_OpenEP  (USBD_HandleTypeDef *pdev,
                                      uint8_t  ep_addr,
                                      uint8_t  ep_type,
                                      uint16_t ep_mps);

USBD_StatusTypeDef  USBD_LL_CloseEP (USBD_HandleTypeDef *pdev, uint8_t ep_addr);
USBD_StatusTypeDef  USBD_LL_FlushEP (USBD_HandleTypeDef *pdev, uint8_t ep_addr);
USBD_StatusTypeDef  USBD_LL_StallEP (USBD_HandleTypeDef *pdev, uint8_t ep_addr);
USBD_StatusTypeDef  USBD_LL_ClearStallEP (USBD_HandleTypeDef *pdev, uint8_t ep_addr);
uint8_t             USBD_LL_IsStallEP (USBD_HandleTypeDef *pdev, uint8_t ep_addr);
USBD_StatusTypeDef  USBD_LL_SetUSBAddress (USBD_HandleTypeDef *pdev, uint8_t dev_addr);
USBD_StatusTypeDef  USBD_LL_Transmit (USBD_HandleTypeDef *pdev,
                                      uint8_t  ep_addr,
                                      uint8_t  *pbuf,
                                      uint16_t  size);

USBD_StatusTypeDef  USBD_LL_PrepareReceive(USBD_HandleTypeDef *pdev,
                                           uint8_t  ep_addr,
                                           uint8_t  *pbuf,
                                           uint16_t  size);

uint32_t USBD_LL_GetRxDataSize  (USBD_HandleTypeDef *pdev, uint8_t  ep_addr);
void  USBD_LL_Delay (uint32_t Delay);
```

usbd_ctlrq.h/c
```
USBD_StatusTypeDef  USBD_StdDevReq (USBD_HandleTypeDef  *pdev, USBD_SetupReqTypedef  *req);
USBD_StatusTypeDef  USBD_StdItfReq (USBD_HandleTypeDef  *pdev, USBD_SetupReqTypedef  *req);
USBD_StatusTypeDef  USBD_StdEPReq  (USBD_HandleTypeDef  *pdev, USBD_SetupReqTypedef  *req);


void USBD_CtlError  (USBD_HandleTypeDef  *pdev, USBD_SetupReqTypedef *req);

void USBD_ParseSetupRequest (USBD_SetupReqTypedef *req, uint8_t *pdata);

void USBD_GetString         (uint8_t *desc, uint8_t *unicode, uint16_t *len);
```

usbd_ioreq.h
```
USBD_StatusTypeDef  USBD_CtlSendData (USBD_HandleTypeDef *pdev,
                               uint8_t *pbuf,
                               uint16_t len);

USBD_StatusTypeDef  USBD_CtlContinueSendData (USBD_HandleTypeDef  *pdev,
                               uint8_t *pbuf,
                               uint16_t len);

USBD_StatusTypeDef USBD_CtlPrepareRx (USBD_HandleTypeDef  *pdev,
                               uint8_t *pbuf,
                               uint16_t len);

USBD_StatusTypeDef  USBD_CtlContinueRx (USBD_HandleTypeDef  *pdev,
                              uint8_t *pbuf,
                              uint16_t len);

USBD_StatusTypeDef  USBD_CtlSendStatus (USBD_HandleTypeDef  *pdev);

USBD_StatusTypeDef  USBD_CtlReceiveStatus (USBD_HandleTypeDef  *pdev);
uint32_t  USBD_GetRxCount (USBD_HandleTypeDef *pdev, uint8_t ep_addr);
```
 
## how does it work
driver send control information to ep0
hardware interrupt will drive the driver code.

- when MCU up or USB plug in, a session request interrupt is generated, and pc driver will send query and setup packages.
and eastablish the control pipe (with EP0)
- when usb plug off or MCU power down, session terminated and will generate an interrupt.
powered state
- host will send reset signalling and begin enumeration. It will generate reset interrupt and enumeration done interrupt.
and enter in default state.

- default state: set_address, not other command is accepted.

- addressed state: ready to answer usb transaction on the address.

- when usb bus 3ms idle, it will enter suspended state and generate an interrupt
- resume from the host to clear the suspended state.

1 in/out control ep0
5 in data ep
5 out data ep.

events:

• Transfer completed interrupt, indicating that data transfer was completed on both the
application (AHB) and USB sides
• Setup stage has been done (control-out only)
• Associated transmit FIFO is half or completely empty (in endpoints)
• NAK acknowledge has been transmitted to the host (isochronous-in only)
• IN token received when Tx FIFO was empty (bulk-in/interrupt-in only)
• Out token received when endpoint was not yet enabled
• Babble error condition has been detected
• Endpoint disable by application is effective
• Endpoint NAK by application is effective (isochronous-in only)
• More than 3 back-to-back setup packets were received (control-out only)
• Timeout condition detected (control-in only)
• Isochronous out packet has been dropped, without generating an interrup

how does the 5 end point cooperate to perform a transaction
divide into packets and assemble packets.






	
		   
		   
		
		



