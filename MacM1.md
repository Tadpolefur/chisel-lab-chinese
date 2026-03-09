# Mac M1 （苹果芯片）

使用苹果芯片的 Mac 电脑

 * 使用 Parallel 建立 Windows 11 for ARM 的虚拟机
 * 安装 Vivado
 * Windows 缺少 FTDI 驱动程序 - 安装 Digilent 的 Adapt 软件以添加缺失的驱动程序
 * 使用 Windows 11 Arm 版本生成比特流
   * 使用 Vivado 配置驱动，并使用 Putty 监听串行端口
 * 配置 Mac 上的 OpenOCD
   * 参见 https://github.com/byu-cpe/BYU-Computing-Tutorials/wiki/Program-7-Series-FPGA-from-a-Mac-or-Linux-Without-Xilinx
     * 运行它需要 sudo 权限
   * 配置文件 `7series.txt` 中包含多个 A7 板卡的配置，如 Basys3、NexysA7：
      ```
      # File: 7series.txt
      interface ftdi
      ftdi_device_desc "Digilent USB Device"
      ftdi_vid_pid 0x0403 0x6010
      # channel 1 does not have any functionality
      ftdi_channel 0
      # just TCK TDI TDO TMS, no reset
      ftdi_layout_init 0x0088 0x008b
      reset_config none
      adapter_khz 10000

      source [find cpld/xilinx-xc7.cfg]
      source [find cpld/jtagspi.cfg]
      init

      puts [irscan xc7.tap 0x09]
      puts [drscan xc7.tap 32 0]  

      puts "Programming FPGA..."
      pld load 0 Hello.bit
      exit
      ```
   * 从 Windows 机器获取 `.bit` 文件并在 `7series.txt` 中设置其名称
   * 使用 `openocd -f 7series.txt` 配置
   * 在 Mac 上使用 `ls -l /dev/cu.*`
   * 串行端口 `screen /dev/cu.usbserial-.... 115200`


