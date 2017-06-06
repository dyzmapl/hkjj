# Description of algorithms and their parameters

Each algorithm has its own set of parameters (part common, part specific). 
A detailed description of the parameters and a description of the calculation features are given in the following topics:

- Area scan
- Circle
- Landing
- Perimeter
- Photogrammetry tool
- Takeoff
- Waypoint

<table>
<tr>
<td colspan="3"><h2>Area scan</h2></td>
</tr>
<tr>
<th>
<div><sub>Name in UCS</sub></div>
</th>
<th>
<div><sub>Type</sub></div>
</th>
<th>
<div><sub>Purpose</sub></div>
</th>
</tr>
<tr>
<td><sub>speed</sub></td>
<td><sub>Double</sub></td>
<td><sub>Flight speed</sub><br />
<p><sub>The value corresponds to the value specified for the previous segment of the route.</sub></p>
<p><sub>If there are no segments, the value is taken from the profile = default ground speed.</sub></p>
</td>
</tr>
<tr>
<td><sub>wpTurnType</sub></td>
<td><sub>Selection</sub><br>
<p><sub>Turn type. Can be one of the following values:</sub></p>
<ul>
<li><sub>STOP_AND_TURN;</sub></li>
<li><sub>STRAIGHT;</sub></li>
<li><sub>SPLINE;</sub></li>
<li><sub>BANK_TURN.</sub></li>
</ul>
<p><sub>When changing the machine, if selected value in the new list of allowed turn types, then it remains</sub></p>
</td>
<td>
<p><sub>Turn type is selected from the list of available turn types for a profile.</sub></p>
<p><sub>The default value is the default value for the profile</sub></p>
</td>
</tr>
<tr>
<td><sub>height</sub></td>
<td><sub>Double<br>Flight height</sub></td>
<td>
<ul>
<li><sub>In case the area scan is the first scheduling algorithm in the route, the altitude value is formed according to the rule:</sub><br />
<ul>
<li><sub>altitude type of the route is AGL, altitude type of the area scan is AMSL = value from the route settings from the emergency return altitude field is added to the elevation at the first point of the area scan set on the map. Accordingly, this value is emergency return altitude + elevation and will be Flight height</sub></li>
<li><sub>altitude type of the route is AGL, altitude type of the area scan is AGL = value is taken from the route settings from the emergency return altitude field</sub></li>
<li><sub>altitude type of the route is AMSL, altitude type of the area scan is AMSL = value is taken from the route settings from the emergency return altitude field</sub></li>
<li><sub>altitude type of the route is AMSL, altitude type of the area scan is AGL = value from the route settings from the emergency return altitude field is subtracted from the elevation at the first point of the area scan set on the map. Accordingly, this value is emergency return altitude - elevation and will be Flight height</sub></li>
</ul>
</li>
<li><sub>In case the area scan is followed by another route algorithm:</sub>
<ul>
<li><sub>altitude type of the route is AGL, altitude type of the area scan is AMSL =&nbsp;altitude of the last scheduling algorithm is added to the elevation at the first point of the area scan set on the map. Accordingly, this value of altitude prevoius alghorithm + elevation and is the value of Flight height</sub></li>
<li><sub>altitude type of the route is AGL, altitude type of the area scan is AGL = value is taken equal to the altitude of the last scheduling algorithm</sub></li>
<li><sub>altitude type of the route is AMSL, altitude type of the area scan is AMSL = the value is taken equal to the altitude of the last scheduling algorithm</sub></li>
<li><sub>altitude type of the route is AMSL, altitude type of the area scan is AGL =&nbsp;altitude of the last scheduling algorithm is subtracted&nbsp;from the elevation at the first point of the area scan set on the map. Accordingly, this value of altitude prevoius alghorithm - elevation and is the value of Flight height</sub></li>
</ul>
</li>
</ul>
<ul>
<li><sub>When altitude type is changed in area scan, flight height is reset</sub></p></li>
</ul>
</td>
</tr>
<tr>
<td><sub>altitudeType</sub></td>
<td><sub>Selection</sub><br>
<p><sub>Usage altitude type:</sub></p>
<ul>
<li><sub>WGS84 - route traversing one given AMSL-altitude;</sub></li>
<li><sub>AGL&nbsp;- route traversing one given AGL-altitude.</sub></li>
</ul>
</td>
<td><sub>AMSL</sub></td>
</tr>
<tr>
<td><sub>sideDistance</sub></td>
<td><sub>Double</sub></td>
<td><sub>Side distance</sub></td>
</tr>
<tr>
<td><sub>directionAngle</sub></td>
<td><sub>Double</sub><br>
<p><sub>Direction angle (0-360)</sub></p>
<p><sub>Angle of rotation of the grid.</sub></p>
</td>
<td><sub>0</sub></td>
</tr>
<tr>
<td><sub>avoidObstacles</sub></td>
<td><sub>Boolean<br>Avoid obstacles</sub></td>
<td><sub>yes</sub></td>
</tr>
<tr>
<td><sub>actionExecution</sub></td>
<td><sub>Selection</sub><br>
<p>Action execution. Shows when actions are to be performed:</sub></p>
<ul>
<li><sub>ONLY_AT_START - only at the starting point;</sub></li>
<li><sub>ACTIONS_EVERY_POINT - at each point;</sub></li>
<li><sub>ACTIONS_ON_FORWARD_PASSES - at each point of passes. But when turning, the camera should turn off.</sub></li>
</ul>
</td>
<td><sub>every point</sub></td>
</tr>
<tr>
<td><sub>overshoot</sub></td>
<td><sub>Double</sub><br>
<p><sub>Overshoot</sub></p>
<p><sub>Additional distance beyond the snake, which must be passed to properly and completely pass through the snake taking into account the features of the rotation of the plane.</sub></p>
</td>
<td></td>
</tr>
<tr>
<td><sub>overshootSpeed</sub></td>
<td><sub>Double</td>
<td><sub>Speed for overshoot part</sub></td>
</tr>
<tr>
<td><sub>areaScanAllowPartialCalculation</sub></td>
<td><sub>Boolean</td>
<td><sub>Whether to allow partial path computation or to throw an error when some of the points are inaccessible for some reason</sub></td>

</tr>
<tr>
<td><sub>tolerance</td>
<td><sub>Double</td>
<td><sub>Allowable height difference, when you can not put additional points. An additional point is placed if it goes beyond this boundary.</sub></td>
</tr>
<tr>
<td><sub>noActionsAtLastPoint</sub></td>
<td><sub>Boolean</td>
<td><sub>Do not perform actions at the last point</sub></td>
</tr>
<tr>
<td colspan="3"><h2>Circle</h2></td>
</tr>
<tr>
<th>
<div><sub>Name in UCS</sub></div>
</th>
<th>
<div><sub>Type</sub></div>
</th>
<th>
<div><sub>Purpose</sub></div>
</th>
</tr>
<tr>
<td><sub>speed</sub></td>
<td><sub>Double</sub></td>
<td><sub>Flight speed</sub><br />
<p><sub>The value corresponds to the value specified for the previous segment of the route.</sub></p>
<p><sub>If there are no segments, the value is taken from the profile = default ground speed.</sub></p>
</td>
</tr>
<tr>
<td><sub>wpTurnType</sub></td>
<td><sub>Selection</sub><br />
<p><sub>Turn type. Can be one of the following values:</sub></p>
<ul>
<li><sub>STOP_AND_TURN;</sub></li>
<li><sub>STRAIGHT;</sub></li>
<li><sub>SPLINE;</sub></li>
<li><sub>BANK_TURN.</sub></li>
</ul>
<p><sub>When changing the machine, if selected value in the new list of allowed turn types, then it remains</sub></p>
</td>
<td>
<p><sub>Turn type is selected from the list of available turn types for a profile.</sub></p>
<p><sub>The default value is the default value for the profile</sub></p>
</td>
</tr>
<tr>
<td><sub>loops</sub></td>
<td><sub>Integer</sub></td>
<td><sub>Number of repeated flights</sub></td>
</tr>
<tr>
<td><sub>flightClockwise</sub></td>
<td><sub>Boolean</sub></td>
<td><sub>Fly cloclwise</sub></td>
</tr>
<tr>
<td><sub>basePointsQnt</sub></td>
<td><sub>Integer</sub></td>
<td><sub>The number of approximating points. Allows you to specify a fixed value of the points that will be used to form a circle path.</sub></td>
</tr>
<tr>
<td><sub>followTerrain</sub></td>
<td><sub>Boolean</sub></td>
<td><sub>Follow terrain</sub></td>
</tr>
<tr>
<td><sub>actionExecution</sub></td>
<td><sub>Selection</sub></td>
<td>
<p><sub>Action execution. Shows when actions are to be performed:</sub></p>
<ul>
<li><sub>ONLY_AT_START - only at the starting point;</sub></li>
<li><sub>ACTIONS_EVERY_POINT - at each point.</sub></li>
</ul>
</td>
</tr>
<tr>
<td><sub>avoidObstacles</sub></td>
<td><sub>Boolean</sub></td>
<td><sub>Avoid obstacles</sub></td>
</tr>
<tr>
<td><sub>avoidTerrain</sub></td>
<td><sub>Boolean</sub></td>
<td><sub>Avoid terrain</sub></td>
</tr>
<tr>
<td><sub>noActionsAtLastPoint</sub></td>
<td><sub>Boolean</sub></td>
<td><sub>Do not perform actions at the last point</sub></td>
</tr>
</table>
