
# Description

* Try to use SPICE netlist in Xschem
* Environment is https://github.com/iic-jku/osic-multitool
* Sample location is https://github.com/StefanSchippers/xschem_sky130

# Preparations

## 1. Copy these files

```
~/pdk/sky130A/libs.tech/xschem/sky130_tests/
  |-- test_inv.sch  # Inverter testbench
  |-- not.sym       # Inverter symbol
  |-- lvtnot.sym    # Low-voltage threshold Inverter symbol
```

```
~/pdk/sky130A/libs.tech/xschem/
  |-- xschemrc      # Xschem setting file 
```

## Generate netlist

* Open inverter test schematic
```
xschem test_inv.sch
```
* Press Netlist

## Copy generated SPICE file

* SPICE file location
```
~/.xschem/simulations/test_inv.spice
```

* Copied contents
```
* expanding   symbol:  sky130_tests/not.sym # of pins=2
** sym_path: /home/bkic/pdk/sky130A/libs.tech/xschem/sky130_tests/not.sym
** sch_path: /home/bkic/pdk/sky130A/libs.tech/xschem/sky130_tests/not.sch
.subckt not y a VCCPIN VSSPIN     W_N=1 L_N=0.15 W_P=2 L_P=0.15
*.opin y
*.ipin a
XM1 y a VSSPIN VSSPIN sky130_fd_pr__nfet_01v8 L=L_N W=W_N nf=1 ad='int((nf+1)/2) * W/nf * 0.29' as='int((nf+2)/2) * W/nf * 0.29' pd='2*int((nf+1)/2) * (W/nf + 0.29)'
+ ps='2*int((nf+2)/2) * (W/nf + 0.29)' nrd='0.29 / W' nrs='0.29 / W' sa=0 sb=0 sd=0 mult=1 m=1
XM2 y a VCCPIN VCCPIN sky130_fd_pr__pfet_01v8 L=L_P W=W_P nf=1 ad='int((nf+1)/2) * W/nf * 0.29' as='int((nf+2)/2) * W/nf * 0.29' pd='2*int((nf+1)/2) * (W/nf + 0.29)'
+ ps='2*int((nf+2)/2) * (W/nf + 0.29)' nrd='0.29 / W' nrs='0.29 / W' sa=0 sb=0 sd=0 mult=1 m=1
.ends


* expanding   symbol:  sky130_tests/lvtnot.sym # of pins=2
** sym_path: /home/bkic/pdk/sky130A/libs.tech/xschem/sky130_tests/lvtnot.sym
** sch_path: /home/bkic/pdk/sky130A/libs.tech/xschem/sky130_tests/lvtnot.sch
.subckt lvtnot a y VCCPIN VSSPIN     W_N=1 L_N=0.15 W_P=2 L_P=0.35
*.opin y
*.ipin a
XM2 y a VCCPIN VCCPIN sky130_fd_pr__pfet_01v8_lvt L=L_P W=W_P nf=1 ad='int((nf+1)/2) * W/nf * 0.29' as='int((nf+2)/2) * W/nf * 0.29'
+ pd='2*int((nf+1)/2) * (W/nf + 0.29)' ps='2*int((nf+2)/2) * (W/nf + 0.29)' nrd='0.29 / W' nrs='0.29 / W' sa=0 sb=0 sd=0 mult=1 m=1
XM1 y a VSSPIN VSSPIN sky130_fd_pr__nfet_01v8_lvt L=L_N W=W_N nf=1 ad='int((nf+1)/2) * W/nf * 0.29' as='int((nf+2)/2) * W/nf * 0.29'
+ pd='2*int((nf+1)/2) * (W/nf + 0.29)' ps='2*int((nf+2)/2) * (W/nf + 0.29)' nrd='0.29 / W' nrs='0.29 / W' sa=0 sb=0 sd=0 mult=1 m=1
.ends
```

# Modifications
.
* Add XSCHEM_USER_LIBRARY_PATH pointing to this folder.
* Change not to knot in the symbol file and the SPICE file.
* Change lvtnot to klvtnot in the symbol file and the SPICE file.
* Replace 1 symbol in the test inverter schematic.
* Include `klib.spice` file in the test inverter schematic.
* Change type of symbols from subcircuit to primitive
* Add the current director to the `xschemrc` file.

# How to run

* Set PDK_ROOT environmental variable
```
export PDK_ROOT=~/pdk
```

* Source `ksource.sh` file
```
$ source ksource.sh
```

* Open test inverter schematic
```
$ xschem test_inv.sch
```

* Create a netlist and check SPICE file by clicking `Simulation > Edit Netlist`
* Simulate and check waveform


# References

* [[xschem.sourceforge.io] TUTORIAL: CREATE A SYMBOL AND USE AN EXISTING NETLIST](https://xschem.sourceforge.io/stefan/xschem_man/tutorial_use_existing_subckt.html)
* [[tcl.tk] pwd](https://www.tcl.tk/man/tcl9.0/TclCmd/pwd.html) 
* [[github.com] OSIC Multitool Sample analog inverter testbench](https://github.com/iic-jku/osic-multitool/blob/main/example/ana/tb_inv.sch)

