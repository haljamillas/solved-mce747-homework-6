Download Link: https://assignmentchef.com/product/solved-mce747-homework-6
<br>
<h1>: Robust Passivity-Based Tracking Control of the WAM Model</h1>

The objective is to implement and tune an RPBC tracking controller for joints 1,2 and 4 of the WAM robot by simulation, with joint 3 fixed at zero. Procedure:

<ol>

 <li>Use the minimal parameter vector and regressor identified in HW5. A complete set ofparameterized matrices, the regressor and the parameter vector are contained in m-file</li>

</ol>

WAM124Ytheta.m attached to this homework.

The inertial parameters of the diagonal elements of <em>M </em>have to be adjusted to include the reflected inertia of the motors and gears. This has been already done in the m-file. Also, since the model of the previous homework did not consider friction, the following columns and corresponding parameters must be appended to the regressor:

<ol start="2">

 <li>Use the nominal parameter values declared in the supplied m-file to obtain the numerical value of the nominal Θ (25 entries).</li>

 <li>Initially tune the controller to track the following reference trajectories:</li>

</ol>

<table width="171">

 <tbody>

  <tr>

   <td width="45"></td>

   <td width="25">=</td>

   <td width="100">sin(<em>wt</em>)</td>

  </tr>

  <tr>

   <td width="45"></td>

   <td width="25">=</td>

   <td width="100">−0<em>.</em>5 + sin(<em>wt</em>)</td>

  </tr>

  <tr>

   <td width="45"></td>

   <td width="25">=</td>

   <td width="100">0<em>.</em>75sin(<em>wt</em>)</td>

  </tr>

 </tbody>

</table>

Use initial conditions that match the desired trajectories at <em>t </em>= 0. Once the controller is running, test for convergence for <em>q</em>(0) =6 <em>q<sup>d</sup></em>(0).

<ol start="4">

 <li>Apply a random perturbation to the nominal Θ, leaving the value used by the controllerthe same. Re-tune and observe the increment in chattering levels. Document the results.</li>

</ol>

<h1>2: Optimal Trajectories for Parameter Estimation</h1>

The goal is to find reference trajectories that optimize the numerical conditioning of the regressor for identification purposes, as discussed in class. Use the following objective function:

where <em>N </em>is the number of data points. As constraints, use the maximum and minimum values for the joint positions, velocities and Cartesian velocities shown in Table 1 where <em>i </em>= 1<em>,</em>2<em>,</em>4, <em>e </em>denotes the end plate position (910 mm along the <em>z</em><sub>3 </sub>axis) and <em>p </em>denotes the Variable     Min Value Max Value        Units

<em>q</em><sub>1 </sub>-1 1 rad <em>q</em><sub>2 </sub>-1 1 rad <em>q</em><sub>4 </sub>-0.5 2 rad <em>q</em>˙<em><sub>i </sub></em>-1 1 rad/s <em>v<sub>e </sub></em>-0.5 0.5 m/s <em>v<sub>p </sub></em>-0.5 0.5 m/s

Table 1: Position and Velocity Constraints

elbow (origin of frame 3).

Optimize over the coefficients of Fourier expansions for <em>q<sub>i</sub></em>, <em>i </em>= 1<em>,</em>2<em>,</em>4.

Use <em>N<sub>f </sub></em>= 4. With this, you have a total of 12 <em>a </em>coefficients and 12 <em>b </em>coefficients. Procedure:

<ul>

 <li>Code a function that returns the extended regressor (including the 4 columns associated with friction) given <em>N</em>-by-1 vectors with <em>q<sub>i</sub></em>(<em>t</em>), ˙<em>q<sub>i</sub></em>(<em>t</em>) and ¨<em>q<sub>i</sub></em>(<em>t</em>). The returned regressor will have 3<em>N </em>rows and as many columns as the parameter vector (it’s a stack of regressors evaluated at all time instants).</li>

 <li>Code for the objective function. It receives a candidate vector [<em>a b</em>], parameters <em>w</em>, <em>N</em>, <em>λ</em><sub>1</sub>, <em>λ</em><sub>2 </sub>and a time vector. The time vector to be used is [0:0.1:10]. The function evaluates <em>q<sub>i</sub></em>(<em>t</em>), ˙<em>q<sub>i</sub></em>(<em>t</em>) and ¨<em>q<sub>i</sub></em>(<em>t</em>) and the 3-row regressor at each time point.</li>

</ul>

The condition number to be used is the sum of the condition numbers of every time instance of the 3-row regressor. The same for the minimum singuar value.

<ul>

 <li>Code for the constraints function. It receives the same arguments as the objective function. It evaluates <em>q<sub>i</sub></em>(<em>t</em>), ˙<em>q<sub>i</sub></em>(<em>t</em>) and ¨<em>q<sub>i</sub></em>(<em>t</em>). From there, it calculates <em>v<sub>e </sub></em>and <em>v<sub>p </sub></em>at each time point. With this information, it finds the minimum and maximum values from the data a forms an inequality constraint output (negative when constraints are satisfied).</li>

 <li>Call fmincon with the following options:</li>

</ul>

options=optimptions(’fmincon’,’Display’,’iter’,’MaxFunctionEvaluations’,10000, ’OptimalityTolerance’,1e-11,’StepTolerance’,1e-11,’MaxIterations’,10000); Use a random guess for [<em>a b</em>] and take <em>λ</em><sub>1 </sub>= 1 and <em>λ</em><sub>2 </sub>= 10.

<ul>

 <li>Show the resulting trajectories and verify that they comply with the constraints.</li>

 <li>Tune the controller to follow these trajectories.</li>

</ul>