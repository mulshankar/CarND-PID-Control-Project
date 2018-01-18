# CarND-Controls-PID
Self-Driving Car Engineer Nanodegree Program

---

## Project Objectives

Tune a PID controller to enable a self driving car to navigate around a track provided by the Udacity simulator.


## Reflections

The PID controller for this project operates on the Cross Track Error (CTE). This refers to the deviation of the car from the center of the track. The error can be mitigated via 3 actuators - steer, throttle and angle. 

The first actuator that was controlled was steer. The process of finding the gains of the PID controller is explained below. The tuning was done in a manual fashion based on recorded values of the CTE and corresponding steer value used to compensate for the CTE. The three gains corresponding to the proportional, integral and derivative components are hereby referred to as Kp, Ki and Kd. 

Kp - the first mode of correction that was applied. It provides a simple mapping between the CTE to the steer value. Higher the Kp, faster the response but the system can beome unstable and oscillate if the value given is too high. Based on the CTE to steer value ratio, a value of -0.1 was chosen to begin with. Note that the steer value is in a direction opposite to that of the CTE. A car that is drifting right needs a steer correction to the left to bring it back on track. A value of -0.06 gave good performance but had some oscillations.  

Kd - Kd terms balances any oscillations that are caused by the Kp correction. The value of Kp and Kd balance out the oscillations while responding faster to the CTE. A value of -0.001 worked well for this project.

Ki - this term tries to mitigate any steady state error in the system. This gain did not have a major impact on this project since the tracks are consistenly changing. A small value of -0.0002 worked well for this project. 

All the above gains work only if speed of the vehicle is controlled. In normal world driving, the natural response to an upcoming sharp turn is to slow down the vehicle. A simple P controller was implemented on the throttle command based on the CTE. The throttle control is shown below:

```
/*----------throttle control----------------*/
		  
double thr_ff=0.2;

double Kp_thr=0.1;
double thr_fdbk=Kp_thr*fabs(cte);

double thr_cmd_final=thr_ff-thr_fdbk;

thr_cmd_final=std::max(0.03,thr_cmd_final);

/*------------------------------------------*/

```
A feed-forward value of 0.2 was used to accelerate the vehicle. The final command is based on CTE but is always above 0.03 to ensure forward propogation. This worked well for this project.

