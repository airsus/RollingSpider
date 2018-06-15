# Rolling Spider

This coursework project is one of key part of ME231B (Experimential Advanced Control Design - Spring 2018). The main framework of this project is based on Rolling Spider software package developed by Parrot <https://github.com/Parrot-Developers/RollingSpiderEdu>, where we can adjust controller, do fancy realtime experiments without too many consideration of the framework. 

# Framework Description and Compile tools
The embedded code is generated by c/c++ auto generator in Matlab. The main drone simulator is written in the file:

# Code folder 
`RollingSpider/Allfiles/MIT_MatlabToolbox/trunk/matlab/Simulation`

# Extended Kalman Filter
`RollingSpider/Allfiles/MIT_MatlabToolbox/trunk/matlab/Simulation`
- Already written in `./sim_quadrotor.slx`
- Kalman Filter parameters setting: `./parameters_estimationcontrol.m`
- Default compensator is `./sim_quadrotor.slx.r2015a`

# LQR Controller 
`RollingSpider/Allfiles/MIT_MatlabToolbox/trunk/matlab/Simulation/controllers/controller_fullstate/LQR`
- The parameters of LQR controller is calculated by global inverse of matrix `./LQRControl.m`)
and stored at `./linsys1.mat`
- The default controller is PID

# Desired Trajectory Setting
`RollingSpider/Allfiles/MIT_MatlabToolbox/trunk/embcode/rsedu_control.c`

From line 1294
`
            // Add position reference tracking
            waitCycles = calibCycles + takeoffCycles + 350;

            if(counter>waitCycles && counter<=waitCycles+refCycles){
            	Drone_Compensator_U_pos_refin[0] = 0;
            	Drone_Compensator_U_pos_refin[1] = (counter - waitCycles)/refCycles;
            }else if((counter>waitCycles+refCycles && counter<=waitCycles+2*refCycles)){
            	Drone_Compensator_U_pos_refin[0] = (counter - waitCycles - refCycles)/refCycles;
            	Drone_Compensator_U_pos_refin[1] = 1;
            }else if((counter>waitCycles+2*refCycles && counter<=waitCycles+3*refCycles)){
            	Drone_Compensator_U_pos_refin[0] = 1;
            	Drone_Compensator_U_pos_refin[1] = 1 - (counter - waitCycles - 2*refCycles)/refCycles;
            }else if((counter>waitCycles+3*refCycles && counter<=waitCycles+4*refCycles)){
            	Drone_Compensator_U_pos_refin[0] = 1 - (counter - waitCycles - 3*refCycles)/refCycles;
            	Drone_Compensator_U_pos_refin[1] = 0;
            }else{
            	Drone_Compensator_U_pos_refin[0] = 0;
            	Drone_Compensator_U_pos_refin[1] = 0;
            }
`
which illustrates a up-down scenario (we use this to test altitude controller). This part can be replaced by any other codes to generate various trajectories (helix, square, triangle, etc)
