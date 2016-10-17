CreateOrUpdateObjectRequest request = new CreateOrUpdateObjectRequest()
{
    ClientId = _connect.AuthorizeHciResponse.ClientId,
    Object = new DomainObjectWrapper().Put(route, "Route"),
    WithComposites = true,
    ObjectType = "Route",
    AcquireLock = false
};