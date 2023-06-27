rtos-keyadc Instructions for use
====

The configuration used when writing this manual is: `hichip_hc15xx_db_b200_defconfig`, and the corresponding deconfig for 16xx chip board replacement is also applicable.

1. Configuration

Setting of keyadc voltage value

1. Open the `hc15xx-db-b200.dts` file and find the adc node.
    adc { 
      devpath = "/dev/adc"; 
      status = "okay"; 
      key-num = <20>; 
      	/key_map = <voltagemin voltagemax, code>
      		/ key-map = <0 20 0>, 
      			<40 80 1>, 
      			<90140 2>, 
      			<160 210 3>, 
      			<230 270 4>, 
      			<310 360 5>, 
      			<390 450 6>, 
      			<470 520 7>, 
      			<580 640 8>, 
      			<680 710 9>, 
      			<740 790 10>, 
      			<840 880 11>, 
      			<900 99012>, 
      			<1020 1070 13>, 
      			<1140 1180 14>, 
      			<1230 1280 15>, 
      			<1370 1430 16>, 
      			<1480 1540 17>, 
      			<1620 1660 18>, 
      			<1760 1810 19>;
    };