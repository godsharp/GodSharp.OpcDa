# GodSharp.OpcDa

Opc DataAccess libary for .NET.

## Lasted Version

|Name|Stable|Preview|
|---|---|---|
|GodSharp.OpcDa|[![NuGet](https://img.shields.io/nuget/v/GodSharp.OpcDa.svg?label=nuget&style=flat-square)](https://www.nuget.org/packages/GodSharp.OpcDa)|[![MyGet](https://img.shields.io/myget/godsharp/vpre/GodSharp.OpcDa.svg?label=myget&style=flat-square)](https://www.myget.org/Package/Details/godsharp?packageType=nuget&packageId=GodSharp.OpcDa)|

## Getting Started

### 1. Initialize

```
OpcTagItem tags = new OpcTagItem[] 
{
    new OpcTagItem { Id = 1, ItemId = "Channel.Test.Tag1" },
    new OpcTagItem { Id = 2, ItemId = "Channel.Test.Tag2" }
});
```

**Sample 1**
```
OpcDaClient client = new OpcDaClient(x => {
    x.ProgId = "KEPware.KEPServerEx.V4"; //required
    x.Tags = tags;
});
```

**Sample 2**
```
OpcDaClient client = new OpcDaClient();
client.Initialize(x => {
    x.ProgId = "KEPware.KEPServerEx.V4"; //required
    x.Tags = tags;
});
```

*sample 2 same as sample 1.*

**Sample 3**
```
OpcDaClient client = new OpcDaClient(new OpcDaClientOption()
{
    ProgId ="KEPware.KEPServerEx.V4", //required
    Tags = tags;
});
```

**Sample 4**
```
OpcDaClient client = new OpcDaClient();
client.Initialize(new OpcDaClientOption()
{
    ProgId ="KEPware.KEPServerEx.V4", //required
    Tags = tags;
});
```

*sample 4 same as sample 3.*

### 2. Binding events

Data changed event:

```
client.DataChange = (param, tags) =>
{
    foreach (OpcTagItem item in tags)
    {
        Console.WriteLine($@"{item.ItemId}:{item.Value.ToString()}");
    }
};
```

Server shutdown event:

```
client.Shutdown = (resaon, client) =>
{
    Console.WriteLine("Opc server shutdown.");
};
```

### 3. Connect/Disconnect

It will throw `InvalidOperationException` and message is `Opc client is not initialized` when you call `Connent` method if not initialized.

```
// Connect
client.Connent();

// Disconnect
client.Disconnect();
```

You can get connect status by `client?.Connected`.

### 4. Add tags

If you want to add tags, you must first connect to opc server.

**Add single tag**
```
client.AddTags(new OpcTagItem { Id = 3, ItemId = "Channel.Test.Tag3" });
```

**Add multi tag**
```
client.AddTags(new OpcTagItem[] 
{
    new OpcTagItem { Id = 3, ItemId = "Channel.Test.Tag3" },
    new OpcTagItem { Id = 4, ItemId = "Channel.Test.Tag4" }
});
```

### 5. Remove Tags

**Remove single tag**
```
client.RemoveTags(3); // OpcTagItem.Id
client.RemoveTags("Channel.Test.Tag3"); // OpcTagItem.ItemId
```

**Remove multi tag**
```
client.RemoveTags(new int[]{ 3, 4 });
client.RemoveTags(new string[]{ "Channel.Test.Tag3", "Channel.Test.Tag4" });
```

### 6. Remove Tag Groups

**Remove single tag group**

```
client.RemoveGroup("Channel.Test.Group1");
```

**Remove multi tag group**

```
client.RemoveGroups(new string[]{ "Channel.Test.Group1", "Channel.Test.Group2" });
```

### 7. Read Tags

**Read single tag value**

```
object val = client.SyncReadValue("Channel.Test.Tag1");
```

**Read single tag**

```
OpcTagItem val = client.SyncRead("Channel.Test.Tag1");
```

**Read multi tag**

```
IEnumerable<OpcTagItem>  tags = client.SyncRead(new string[]{ "Channel.Test.Tag1", "Channel.Test.Tag2" });
```
### 8. Write Tags

**Write single tag value**

```
client.AsycnWriter("Channel.Test.Tag1",val);
```

**Write multi tag**

```
OpcTagItem[] items = new OpcTagItem[]
{
    new OpcTagItem(){ ItemId="Channel.Test.Tag1" },
    new OpcTagItem(){ ItemId="Channel.Test.Tag2" }
};
client.AsycnWriter(items);
```

## License
