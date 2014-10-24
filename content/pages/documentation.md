+++
url = "documentation"
+++

## Documentation

### Basic usage

The EtcdClient object implements the System.Collections.Generic.IDictionary&lt;string, object&gt;
interface.

#### Get key

    var client = EtcdClient.CreateDefault("http://localhost:4001/")

    var value = client["key"];

#### Set key

    var client = EtcdClient.CreateDefault("http://localhost:4001/");

    client["key"] = "value";

### Advanced usage

The AdvancedEtcdClient exposes methods that allows you to use all features in
the etcd HTTP API.

#### Get key

    var client = EtcdClient.CreateDefault("http://localhost:4001/");

    var request = new GetKeyRequest
    {
      Key = "key"
    };

    var response = client.Advanced.GetKey(request);

    var value = response.Node.Value;

[Source](https://github.com/netcd/netcd/blob/master/src/Client/Advanced/Requests/GetKeyRequest.cs)

#### Set key

    var client = EtcdClient.CreateDefault("http://localhost:4001/");

    var request = new SetKeyRequest
    {
      Key = "key",
      Value = "value"
    };

    var response = client.Advanced.SetKey(request);

[Source](https://github.com/netcd/netcd/blob/master/src/Client/Advanced/Requests/SetKeyRequest.cs)

#### Delete key

    var client = EtcdClient.CreateDefault("http://localhost:4001/");

    var request = new DeleteKeyRequest
    {
      Key = "key"
    };


    var response = client.Advanced.DeleteKey(request);

[Source](https://github.com/netcd/netcd/blob/master/src/Client/Advanced/Requests/DeleteKeyRequest.cs)

#### Watch key

    var client = EtcdClient.CreateDefault("http://localhost:4001/");

    Action<Response> callback = response =>
    {
      log.Info("{0} changed value to {1}", response.Node.Key, response.Node.Value);
    };

    var request = new WatchKeyRequest
    {
      Key = "key",
      Callback = callback
    };

    client.Advanced.WatchKey(request);

[Source](https://github.com/netcd/netcd/blob/master/src/Client/Advanced/Requests/WatchKeyRequest.cs)

#### Create directory

    var client = EtcdClient.CreateDefault("http://localhost:4001/");

    var request = new CreateDirectoryRequest
    {
      Key = "directory"
    };

    var response = client.Advanced.CreateDirectory(request);

[Source](https://github.com/netcd/netcd/blob/master/src/Client/Advanced/Requests/CreateDirectoryRequest.cs)

#### Get directory

    var client = EtcdClient.CreateDefault("http://localhost:4001/");

    var request = new GetDirectoryRequest
    {
      Key = "directory"
    };

    var response = client.Advanced.GetDirectory(request);

    foreach (var value in response.Node.Nodes)
    {
      log.Info("{0} has value {1} in directory {2}", value.Key, value.Value, response.Node.Key);
    }

[Source](https://github.com/netcd/netcd/blob/master/src/Client/Advanced/Requests/GetDirectoryRequest.cs)

#### Delete directory

    var client = EtcdClient.CreateDefault("http://localhost:4001/");

    var request = new DeleteDirectoryRequest
    {
      Key = "directory"
    };

    var response = client.Advanced.DeleteDirectory(request);

[Source](https://github.com/netcd/netcd/blob/master/src/Client/Advanced/Requests/DeleteDirectoryRequest.cs)

#### Watch directory

    var client = EtcdClient.CreateDefault("http://localhost:4001/");

    Action<Response> callback = response =>
    {
      log.Info("{0} changed value to {1}", response.Node.Key, response.Node.Value);
    };

    var request = new WatchDirectoryRequest
    {
      Key = "directory",
      Callback = callback
    };

[Source](https://github.com/netcd/netcd/blob/master/src/Client/Advanced/Requests/WatchDirectoryRequest.cs)

### Service discovery

#### Announce

    var client = EtcdClient.CreateDefault("http://localhost:4001/");

    client.Service.Announce("serviceName", "instanceName", "address");

#### Retire

    var client = EtcdClient.CreateDefault("http://localhost:4001/");

    client.Service.Retire("serviceName", "instanceName");

#### Discover

    var client = EtcdClient.CreateDefault("http://localhost:4001/");

    var services = client.Service.Discover("serviceName");

    foreach (var service in services)
    {
      log.Info("serviceName is hosted at {0}", service);
    }