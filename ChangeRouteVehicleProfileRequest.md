
```C#
ChangeRouteVehicleProfileRequest request = new ChangeRouteVehicleProfileRequest
{
    ClientId = _connect.AuthorizeHciResponse.ClientId,
    Route = route,
    NewProfile = new VehicleProfile { Id = vehicleProfile.Id }
};
```