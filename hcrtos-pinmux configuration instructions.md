1. Configuration instructions
----
Take the uart@0 node in `board/hichip/hc16xx/common/dts/hc16xx-db-c3100-v20.dts` as an example:

    uart@0 { 
    	pinmux-active = <PINPADL17 6 PINPADL18 6>; 
    	devicetype = "hichip,hcrtos-setup-setbit"; 
    	regbit = <0xb8800094 18 1>; 
    	devpath ="/dev/uart0"; 
    	status = "disabled"; 
    };

pinmux-active: This attribute is pinmux configuration information

pinmux-active = <PINPADL17 6 PINPADL18 6>; 
Explanation: pinmux-active = <select L17 pinmux pin, function selection of L17, select L18 pinmux pin, function selection of L18>