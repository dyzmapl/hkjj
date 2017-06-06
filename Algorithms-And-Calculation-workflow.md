# Description of algorithms and their parameters

Each algorithm has its own set of parameters (part common, part specific). 
A detailed description of the parameters and a description of the calculation features are given in the following topics:

- Area scan
- Circle
- Landing
- LIDAR survey
- Perimeter
- Photogrammetry tool
- Takeoff
- Waypoint

# Area scan

<table>
<thead>
<tr>
<th>
<div>Name</div>
</th>
<th>
<div>Name in UCS</div>
</th>
<th>
<div>Type</div>
</th>
<th>
<div>Purpose</div>
</th>
<th width="300">
<div>Default value</div>
</th>
<th>
<div>Is the last value storing?</div>
</th>
</tr>
</thead>
<tbody>
<tr>
<td>Flight speed</td>
<td colspan="1">speed</td>
<td colspan="1">Double</td>
<td>Flight speed</td>
<td colspan="1">
<p>The value corresponds to the value specified for the previous segment of the route.</p>
<p>If there are no segments, the value is taken from the profile = default ground speed.</p>
</td>
<td colspan="1">no</td>
</tr>
<tr>
<td>Turn type</td>
<td colspan="1">wpTurnType</td>
<td colspan="1">Selection</td>
<td>
<p>Turn type. Can be one of the following values:</p>
<ul>
<li>STOP_AND_TURN;</li>
<li>STRAIGHT;</li>
<li>SPLINE;</li>
<li>BANK_TURN.</li>
</ul>
<p>When changing the machine, if selected value in the new list of allowed turn types, then it remains</p>
</td>
<td colspan="1">
<p>Turn type is selected from the list of available turn types for a profile.</p>
<p>The default value is the default value for the profile</p>
</td>
<td colspan="1">yes (from 2.12 per profile)</td>
</tr>
<tr>
<td colspan="1">Flight height</td>
<td colspan="1">height</td>
<td colspan="1">Double</td>
<td colspan="1">Flight height</td>
<td colspan="1">
<ol>
<li>In case the area scan is the first scheduling algorithm in the route, the altitude value is formed according to the rule:<br />
<ol>
<li>altitude type of the route is AGL, altitude type of the area scan is AMSL = value from the route settings from the emergency return altitude field is added to the elevation at the first point of the area scan set on the map. Accordingly, this value is emergency return altitude + elevation and will be Flight height</li>
<li>altitude type of the route is AGL, altitude type of the area scan is AGL = value is taken from the route settings from the emergency return altitude field</li>
<li>altitude type of the route is AMSL, altitude type of the area scan is AMSL = value is taken from the route settings from the emergency return altitude field</li>
<li>altitude type of the route is AMSL, altitude type of the area scan is AGL = value from the route settings from the emergency return altitude field is subtracted from the elevation at the first point of the area scan set on the map. Accordingly, this value is emergency return altitude - elevation and will be Flight height</li>
</ol>
</li>
<li>In case the area scan is followed by another route algorithm:
<ol>
<li>altitude type of the route is AGL, altitude type of the area scan is AMSL =&nbsp;altitude of the last scheduling algorithm is added to the elevation at the first point of the area scan set on the map. Accordingly, this value of altitude prevoius alghorithm + elevation and is the value of Flight height</li>
<li>altitude type of the route is AGL, altitude type of the area scan is AGL = value is taken equal to the altitude of the last scheduling algorithm</li>
<li>altitude type of the route is AMSL, altitude type of the area scan is AMSL = the value is taken equal to the altitude of the last scheduling algorithm<br /></li>
<li>altitude type of the route is AMSL, altitude type of the area scan is AGL =&nbsp;altitude of the last scheduling algorithm is subtracted&nbsp;from the elevation at the first point of the area scan set on the map. Accordingly, this value of altitude prevoius alghorithm - elevation and is the value of Flight height</li>
</ol>
</li>
</ol>
<p>3. When altitude type is changed in area scan, flight height is reset</p>
</td>
<td colspan="1">no</td>
</tr>
<tr>
<td colspan="1">Altitude type</td>
<td colspan="1">altitudeType</td>
<td colspan="1">Selection</td>
<td colspan="1">
<p>Usage altitude type:</p>
<ul>
<li>WGS84 - route traversing one given AMSL-altitude;</li>
<li>AGL&nbsp;- route traversing one given AGL-altitude.</li>
</ul>
</td>
<td colspan="1">AMSL</td>
<td colspan="1">yes</td>
</tr>
<tr>
<td colspan="1">Side distance</td>
<td colspan="1">sideDistance</td>
<td colspan="1">Double</td>
<td colspan="1">Side distance.</td>
<td colspan="1">null</td>
<td colspan="1">yes</td>
</tr>
<tr>
<td colspan="1">Direction angle (0-360)</td>
<td colspan="1">directionAngle</td>
<td colspan="1">Double</td>
<td colspan="1">
<p>Direction angle (0-360)</p>
<p>Angle of rotation of the grid.</p>
</td>
<td colspan="1">0</td>
<td colspan="1">yes</td>
</tr>
<tr>
<td colspan="1">Avoid obstacles</td>
<td colspan="1">avoidObstacles</td>
<td colspan="1">Boolean</td>
<td colspan="1">Avoid obstacles</td>
<td colspan="1">yes</td>
<td colspan="1">yes</td>
</tr>
<tr>
<td colspan="1">Action execution</td>
<td colspan="1">actionExecution</td>
<td colspan="1">Selection</td>
<td colspan="1">
<p>Action execution. Shows when actions are to be performed:</p>
<ul>
<li>ONLY_AT_START - only at the starting point;</li>
<li>ACTIONS_EVERY_POINT - at each point;</li>
<li>ACTIONS_ON_FORWARD_PASSES - at each point of passes. But when turning, the camera should turn off.</li>
</ul>
</td>
<td colspan="1">every point</td>
<td colspan="1">yes</td>
</tr>
<tr>
<td colspan="1">Overshoot</td>
<td colspan="1">overshoot</td>
<td colspan="1">Double</td>
<td colspan="1">
<p>Overshoot</p>
<p>Additional distance beyond the snake, which must be passed to properly and completely pass through the snake taking into account the features of the rotation of the plane.</p>
</td>
<td colspan="1">null</td>
<td colspan="1">yes</td>
</tr>
<tr>
<td colspan="1">Overshoot speed</td>
<td colspan="1">overshootSpeed</td>
<td colspan="1">Double</td>
<td colspan="1">Speed for overshoot part</td>
<td colspan="1">null</td>
<td colspan="1">yes</td>
</tr>
<tr>
<td colspan="1">Allow partial calculation</td>
<td colspan="1">areaScanAllowPartialCalculation</td>
<td colspan="1">Boolean</td>
<td colspan="1">Whether to allow partial path computation or to throw an error when some of the points are inaccessible for some reason</td>
<td colspan="1">no</td>
<td colspan="1">yes</td>
</tr>
<tr>
<td colspan="1">AGL Tolerance</td>
<td colspan="1">tolerance</td>
<td colspan="1">Double</td>
<td colspan="1">Allowable height difference, when you can not put additional points. An additional point is placed if it goes beyond this boundary.</td>
<td colspan="1">3</td>
<td colspan="1">yes</td>
</tr>
<tr>
<td colspan="1">No actions at last point</td>
<td colspan="1">noActionsAtLastPoint</td>
<td colspan="1">Boolean</td>
<td colspan="1">Do not perform actions at the last point</td>
<td colspan="1">yes</td>
</tr>
</tbody>
</table>
