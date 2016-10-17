| Property     | Type | Description | Required |
|:------------- |:-------------|:----- |:-----|
| ClientId      | int | Current connection client ID | Yes 
| ObjectType    | string | Object type (Route, Mission etc...)      | Yes 
| WithComposites | bool | ... | No 
| WithCompositesSpecified | bool | ... | No 
| AcquireLock | bool | ... | No 
| AcquireLockSpecified | bool | ... | No 

CreateOrUpdateObjectRequest Example

```C#
CreateOrUpdateObjectRequest request = new CreateOrUpdateObjectRequest()
{
    ClientId = _connect.AuthorizeHciResponse.ClientId,
    Object = new DomainObjectWrapper().Put(route, "Route"),
    WithComposites = true,
    ObjectType = "Route",
    AcquireLock = false
};
```