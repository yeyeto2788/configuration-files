# T8 CNC

## Default values of the original board that comes with the CNC.

**`$$ (view Grbl settings)`**

```
$0=8 (step pulse, usec)
$1=70 (step idle delay, msec)
$2=0 (step port invert mask:00000000)
$3=0 (dir port invert mask:00000000)
$4=0 (step enable invert, bool)
$5=0 (limit pins invert, bool)
$6=0 (probe pin invert, bool)
$10=3 (status report mask:00000011)
$11=0.010 (junction deviation, mm)
$12=0.002 (arc tolerance, mm)
$13=0 (report inches, bool)
$20=0 (soft limits, bool)
$21=0 (hard limits, bool)
$22=0 (homing cycle, bool)
$23=0 (homing dir invert mask:00000000)
$24=25.000 (homing feed, mm/min)
$25=500.000 (homing seek, mm/min)
$26=250 (homing debounce, msec)
$27=1.000 (homing pull-off, mm)
$100=1600.000 (x, step/mm)
$101=1600.000 (y, step/mm)
$102=1600.000 (z, step/mm)
$110=200.000 (x max rate, mm/min)
$111=200.000 (y max rate, mm/min)
$112=200.000 (z max rate, mm/min)
$120=10.000 (x accel, mm/sec^2)
$121=12.000 (y accel, mm/sec^2)
$122=12.000 (z accel, mm/sec^2)
$130=100.000 (x max travel, mm)
$131=150.000 (y max travel, mm)
$132=50.000 (z max travel, mm)
```

**`$# (view # parameters)`**

```
[G54:-24.000,-30.000,-16.920]
[G55:0.000,0.000,0.000]
[G56:0.000,0.000,0.000]
[G57:0.000,0.000,0.000]
[G58:0.000,0.000,0.000]
[G59:0.000,0.000,0.000]
[G28:0.000,0.000,0.000]
[G30:0.000,0.000,0.000]
[G92:0.000,0.000,0.000]
[TLO:0.000]
[PRB:0.000,0.000,0.000:0]
```

**`$G (view parser state)`**

```
[G0 G54 G17 G21 G91 G94 M0 M5 M9 T0 F0.]
```


**`$I (view build info)`**

```
[0.9j.20160325:]
```

**`$N (view startup blocks)`**

```
$N0=
$N1=
```
