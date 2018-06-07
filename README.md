# Freefall modeling in Matlab
Modeling [Felix Baumgartner's Redbull Stratos jump](https://www.youtube.com/watch?v=FHtvDA0W34I) from the stratosphere.

Final Models:
![alt text](https://github.com/JerryCoralis/Matlab-Modeling-Space-Freefall/blob/master/g4250.png "graphs")



## Method:
1. data is read and processed and rows of excusively `NaN` are erased.
2. for each part, variable `Part` is changed, and the plotting function is called.
3. plots are generated by functions on the bottom.

## Reading data:
Code takes in a excel file that has data relevant to Felix Baumgartner's jump. Columns of the data is read and filed under
the variables they represent. Constants for gravity and the altitude he jumps are started here. 

## Functions:
`plotComparision(timeCap, title, number of plots)` 
- numer of plots, 0= all 3, 1-3 sets which of positon, velocity and acceleration to plot
- ODE45 is called at the very top so as to match any changes in parameters for a given part 

`freefall(time, Position)`
- the 2 parametesr that will be used by ODE45 later, returns 2 cols, postion and velocity
- these outputs are what is used for plotting the modeled data

`acclereation(time, positon, velocity)`
- called inside freefall, returns -9.8 for part 1 and -(acceleration due to gravity) + (accleration due to drag) otherwise

`gravityFind(position)`
- returns accelereation due to gravity 9.8 for part 1-3
- returns 9.8(Re/Re_altitude) otherwise 

`mass(~,~)`
- returns Felix's mass

`drag(time, position, velocity, mass)`
- returns b = 0.2 for part 2
- returns the b = 1/2*rho*Acd, rho is calculated either with stdatmo, or Airdensity depending on the part 

`ACdFind(position, mass)`
- calculates ACd using 2 terminal velocities, part 3 uses only the first one
- for part 5,6 altitude when he opens his chute is 3400 and full opened is 2407
- for part < 6, ACd is either 1 or 2 fixed values 
- for part 6, ACd is calculated dynamically dependent on the growth of Felix's cross sectional area due to the parachute

`getACd(acceleration, altitdue, velocity, mass)`
- solve for ACd using formula =  2*(a - a_grav).*m./(rho.*v.^2)

`openPara(paraArea, BodyArea, altitudeParaOpen, altitudeParaFullyOpen, position)`
- if above the altiude he is suppose to open parachute, ACd doesnt need to account for a growing cross-sectionall area
- else, ACd is recalculated using paraArea*((h1 - p)/(h1 - h2))^2 as a rate for the increasing ACd

rho is found either by stdatmo() or airDensity()
