# Scenario 1: Adjust Mass
Through simple manual binary searching, the mass should be around `0.5` for the quad to stay in simulator.

# Scenario 2: Attitude Control
Result:
<p align="center">
<img src="animations/s2.gif" width="500"/>
</p>

Firstly, implement `GenerateMotorCommands()` by solving the following equations:</br>
<a href="https://www.codecogs.com/eqnedit.php?latex=\left\{\begin{matrix}&space;c=F_1&plus;F_2&plus;F_3&plus;F_4&space;\\&space;\tau_x=(F_1-F_2-F_3&plus;F_4)*l\\&space;\tau_y=(F_1&plus;F_2-F_3-F_4)*l\\&space;\tau_z=-k_m*(F_1-F_2&plus;F_3-F_4)&space;\end{matrix}\right." target="_blank"><img src="https://latex.codecogs.com/gif.latex?\left\{\begin{matrix}&space;c=F_1&plus;F_2&plus;F_3&plus;F_4&space;\\&space;\tau_x=(F_1-F_2-F_3&plus;F_4)*l\\&space;\tau_y=(F_1&plus;F_2-F_3-F_4)*l\\&space;\tau_z=-k_m*(F_1-F_2&plus;F_3-F_4)&space;\end{matrix}\right." title="\left\{\begin{matrix} c=F_1+F_2+F_3+F_4 \\ \tau_x=(F_1-F_2-F_3+F_4)*l\\ \tau_y=(F_1+F_2-F_3-F_4)*l\\ \tau_z=-k_m*(F_1-F_2+F_3-F_4) \end{matrix}\right." /></a></br>
One thing to notice is that the order of motors are not numbered in the classic clockwise way.

P controllers `BodyRateControl()` and `RollPitchControl()` are almost the same with what we have done in the exercises of Lesson 14: Full 3D control.
Since body rate controller is a low level critical controller others depend on, we set `kpPQR` to larger value `65, 65, 20`. Based on this setting, the `kpBank` for roll-pitch controller is tuned to `10` for quick settling and small overshoot.

# Scenario 3: Position Control
Result:
<p align="center">
<img src="animations/s3.gif" width="500"/>
</p>

PD controllers `AltitudeControl()` and `LateralPostionControl()`, and P controller `YawControl()` are implemented. The I part of `AltitudeControl()` will be done in the next scenario. For tuning the parameters, the velocity gains are kept 3~4 times larger than the position gains:
```
kpPosXY = 3
kpPosZ = 3
kpVelXY = 10
kpVelZ = 10
kpYaw = 4
```

# Scenario 4: Non-idealities
Result:
<p align="center">
<img src="animations/s4.gif" width="500"/>
</p>

Directly running the controller and parameter set from Step 3 does not work well for the green and red quad -- The green one runs a shifted curve path and the red one stops too early. After relaxing the parameters, green one becomes better, but the red one still has a constant error. Then by adding the I term in `AltitudeControl()`, and adjust the parameters properly, we get nice paths for all the three quads.

