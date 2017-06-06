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

# Area scan

<table style="font-size:11">
<thead>
<tr>
<th>
<div>Name in UCS</div>
</th>
<th>
<div>Type</div>
</th>
<th>
<div>Purpose</div>
</th>
</tr>
</thead>
<tbody>
<tr>
<td>speed</td>
<td>Double</td>
<td>Flight speed<br />
<p>The value corresponds to the value specified for the previous segment of the route.</p>
<p>If there are no segments, the value is taken from the profile = default ground speed.</p>
</td>
</tr>
<tr>
<td>wpTurnType</td>
<td>Selection<br>
<p>Turn type. Can be one of the following values:</p>
<ul>
<li>STOP_AND_TURN;</li>
<li>STRAIGHT;</li>
<li>SPLINE;</li>
<li>BANK_TURN.</li>
</ul>
<p>When changing the machine, if selected value in the new list of allowed turn types, then it remains</p>
</td>
<td>
<p>Turn type is selected from the list of available turn types for a profile.</p>
<p>The default value is the default value for the profile</p>
</td>
</tr>
<tr>
<td>height</td>
<td>Double<br>Flight height</td>
<td>
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
</tr>
<tr>
<td>altitudeType</td>
<td>Selection<br>
<p>Usage altitude type:</p>
<ul>
<li>WGS84 - route traversing one given AMSL-altitude;</li>
<li>AGL&nbsp;- route traversing one given AGL-altitude.</li>
</ul>
</td>
<td>AMSL</td>
</tr>
<tr>
<td>sideDistance</td>
<td>Double</td>
<td>Side distance</td>
</tr>
<tr>
<td>directionAngle</td>
<td>Double<br>
<p>Direction angle (0-360)</p>
<p>Angle of rotation of the grid.</p>
</td>
<td>0</td>
</tr>
<tr>
<td>avoidObstacles</td>
<td>Boolean<br>Avoid obstacles</td>
<td>yes</td>
</tr>
<tr>
<td>actionExecution</td>
<td>Selection<br>
<p>Action execution. Shows when actions are to be performed:</p>
<ul>
<li>ONLY_AT_START - only at the starting point;</li>
<li>ACTIONS_EVERY_POINT - at each point;</li>
<li>ACTIONS_ON_FORWARD_PASSES - at each point of passes. But when turning, the camera should turn off.</li>
</ul>
</td>
<td>every point</td>
</tr>
<tr>
<td>overshoot</td>
<td>Double<br>
<p>Overshoot</p>
<p>Additional distance beyond the snake, which must be passed to properly and completely pass through the snake taking into account the features of the rotation of the plane.</p>
</td>
<td></td>
</tr>
<tr>
<td>overshootSpeed</td>
<td>Double</td>
<td>Speed for overshoot part</td>
</tr>
<tr>
<td>areaScanAllowPartialCalculation</td>
<td>Boolean</td>
<td>Whether to allow partial path computation or to throw an error when some of the points are inaccessible for some reason</td>

</tr>
<tr>
<td>tolerance</td>
<td>Double</td>
<td>Allowable height difference, when you can not put additional points. An additional point is placed if it goes beyond this boundary.</td>
</tr>
<tr>
<td>noActionsAtLastPoint</td>
<td>Boolean</td>
<td>Do not perform actions at the last point</td>
</tr>
</tbody>
</table>
