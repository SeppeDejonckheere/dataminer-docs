---
uid: General_Main_Release_10.5.0_new_features
---

# General Main Release 10.5.0 – New features - Preview

> [!IMPORTANT]
> We are still working on this release. Some release notes may still be modified or moved to a later release. Check back soon for updates!

## Highlights

*No highlights have been selected yet.*

## New features

### Core functionality

#### BrokerGateway DxM will now be installed automatically during a DataMiner upgrade [ID_37714]

<!-- MR 10.5.0 - FR 10.4.1 -->

When a DataMiner Agent is upgraded to version 10.5.0/10.4.1 or above, the *BrokerGateway* DxM will automatically be installed in the `C:\Program Files\Skyline Communications\DataMiner BrokerGateway` folder.

This new DxM, which is currently still under development, is intended to manage all NATS configurations.

#### SLNetTypes and SLGlobal now support a new AlarmTreeID/SLAlarmTreeKey object [ID_37950]

<!-- MR 10.5.0 - FR 10.4.2 -->

The *SLNetTypes* and *SLGlobal* implementations have been updated to support a new *AlarmTreeID/SLAlarmTreeKey* object, which can used to refer to an alarm tree. This new object was created in order to support alarm ID ranges per element rather than per DataMiner Agent, a feature that is still in progress.

Also, a number of client messages have been adapted to support passing this new *AlarmTreeID/SLAlarmTreeKey* object, and a number of existing properties have been marked as obsolete.

#### SLNetTypes: New requests GetLogTextFileStringContentRequestMessage and GetLogTextFileBinaryContentRequestMessage [ID_39021]

<!-- MR 10.5.0 - FR 10.4.5 -->

SLNetTypes now exposes two new request-response operations that will allow you to retrieve a file from the *C:\\Skyline DataMiner\\Logging* folder or one of its subfolders:

| Type of file to be retrieved | Request | Response |
|---|---|---|
| ASCII text files (e.g. log files) | `GetLogTextFileStringContentRequestMessage` | `GetLogTextFileStringContentResponseMessage` |
| Binary files (e.g. zip files)     | `GetLogTextFileBinaryContentRequestMessage` | `GetLogTextFileBinaryContentResponseMessage` |

Both requests have the following arguments:

- The name of the file to be retrieved (with or without extension, with or without full path)
- The ID of the DataMiner Agent

Restrictions:

- The user must have administrative privileges or must be granted the *SDLogging* permission.
- The requests must sent from a managed DataMiner module, i.e. not directly from a client application.
- The requests must be sent via a scripted, wrapped connection (e.g. a QAction of a protocol)
- The file name passed in the requests must be the name of an existing file.
- The file path passed in the requests must be a valid, existing path.

#### GQI: Ad hoc data sources and custom operators can now log messages and exceptions within GQI [ID_39043]

<!-- MR 10.5.0 - FR 10.4.5 -->

When configuring an ad hoc data source or a custom operator, you can now use the new `Logger` property of the `OnInitInputArgs` class to log messages and exceptions within GQI.

#### GQI: Implementing a custom sort order for GQI columns using a custom operator [ID_39136]

<!-- MR 10.5.0 - FR 10.4.5 -->

It is now possible to define a custom sort order for GQI columns by implementing a custom operator that "redirects" the sort operation on one column to another.

New features added to allow this include:

- Comparing `IGQIColumn` objects

- Inspecting a sort operator appended to a custom operator via the `IGQISortOperator` interface

  - List of sort fields (of type `IGQISortField`)
  - Each sort field exposes a sort column (`IGQIColumn`) and a sort direction (`GQISortDirection`)

- An `IGQIFactory` property is now exposed on the `OnInitInputArgs`, which provides factory functions to generate

  - a new `IGQISortField`
  - a new `IGQISortOperator`

#### Storage as a Service: Proxy support [ID_39221] [ID_39313]

<!-- RN 39221: MR 10.5.0 - FR 10.4.5 -->
<!-- RN 39313: MR 10.5.0 - FR 10.4.6 -->

Storage as Service (STaaS) now supports the use of a proxy server.

When you configure a proxy server in the *Db.xml* file, all traffic towards STaaS will pass through the proxy instead of going directly to the cloud.

Example of a *Db.xml* file in which a proxy server has been configured:

```xml
<?xml version="1.0"?>
<DataBases xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns="http://www.skyline.be/config/db">
  <DataBase active="true" local="true" search="true" cloud="true" type="CloudStorage">
    <Proxy>
      <Address></Address>
      <Port></Port>
      <UserName></UserName>
      <Password></Password>
    </Proxy>
  </DataBase>
</DataBases>
```

> [!NOTE]
>
> - The proxy server will be used once the `<Address>` field is filled in. If the proxy server does not require any authentication, the `<UserName>` and `<Password>` fields can be left blank or removed altogether.
> - It is also possible to migrate data towards a STaaS system that is using a proxy server.

#### Correlation log file will now include correlation rule statistics [ID_39301]

<!-- MR 10.5.0 - FR 10.4.8 -->

For every correlation rule that is executed, a number of statistics will now be stored in the *SLCorrelation.txt* log file on a daily basis.

Example of a log entry containing correlation rule statistics:

```txt
2024/05/28 00:00:00.011|SLNet.exe|Log|INF|0|32|CorrelationRuleActionStatistics => [Rule => My_Correlation_Rule; NumberOfTimesExecuted => 6; TotalExecutionDuration => 00:31:41.5222024; MinimumExecutionDuration => 00:01:40.0854200; MaximumExecutionDuration => 00:10:00.4677544; FirstExecutionDuration => 00:10:00.4677544; LastExecutionDuration => 00:05:00.0185738; FirstExecutionTime => 05/27/2024 20:15:03; LastExecutionTime => 05/27/2024 20:48:02;]
```

#### API Gateway version and status can now be checked on <https://skyline-admin.dataminer.services> [ID_39381]

<!-- MR 10.5.0 - FR 10.4.7 -->

On <https://skyline-admin.dataminer.services>, you can now check the current version and current status of the API Gateway DxM.

#### GQI: Exposing the underlying GQI SLNet connection to extensions like ad hoc data sources and custom operators [ID_39489]

<!-- MR 10.5.0 - FR 10.4.6 -->

The `GetConnection()` method can now be used to expose the underlying GQI SLNet connection to GQI extensions like ad hoc data sources and custom operators via the `IConnection` interface. The method is compatible with existing Nuget packages for Automation scripts.

```csharp
IConnection GetConnection()
```

This method will return an [IConnection](xref:Skyline.DataMiner.Net.IConnection) object representing the connection between GQI and SLNet.

> [!NOTE]
> The real underlying connection may be shared by other extensions and queries but can be used as if it were a dedicated connection.

#### Failover: New SLNettypes message to check whether Pcap is installed on a DataMiner Agent [ID_40257]

<!-- MR 10.5.0 - FR 10.4.10 -->

From now on, the new SLNettypes message *PcapInfoRequestMessage* can be used to check whether Pcap is installed on a DataMiner Agent.

The response message *PcapInfoResponseMessage* contains a property called *Info* of type *PcapInfo*.

The *Info* object has the following properties:

| Property | Type | Possible values |
|----------|------|-------------|
| WinPcapDetection | PcapDetectionType | - PcapDetected: WinPcap is installed.<br>- NoPcapDetected: WinPcap is not installed.<br>- Undefined: A problem might have occurred. For more information, open the *SLNet.txt* log file, and look for entries containing the keyword "PcapDetector".  |
| NpcapDetection   | PcapDetectionType | - PcapDetected: NPcap is installed.<br>- NoPcapDetected: NPcap is not installed.<br>- Undefined: A problem might have occurred. For more information, open the *SLNet.txt* log file, and look for entries containing the keyword "PcapDetector". |

> [!NOTE]
>
> - The *PcapInfoRequestMessage* will normally be sent from DataMiner Cube to the DataMiner Agent to which it is connected when the user opens the *Failover Config* window. Only users who have been granted the *Modules > System configuration > Agents > Configure Failover* permission are allowed to send this message. When you do not have this permission, an error message will appear.
> - See also [RN 40267](xref:Cube_Feature_Release_10.4.10#failover-config-window-will-now-show-information-regarding-the-pcap-libraries-that-are-installed-on-the-dmas-id_40267)

### Protocols

#### FillArray now supports protocol.Leave and protocol.Clear [ID_38153]

<!-- MR 10.5.0 - FR 10.4.2 -->

Up to now, the SLProtocol `FillArray` methods did not any support `protocol.Clear` and `protocol.Leave`. An optional `useClearAndLeave` boolean argument has now been added to indicate that `protocol.Clear` and `protocol.Leave` should be treated as cell actions instead of cell values. When this argument is not provided, `useAndClear` will be considered false and `protocol.Clear` and `protocol.Leave` will be treated as cell values.

The following methods have been added to the `SLProtocol` and `SLProtocolExt` interfaces:

```csharp
protocol.FillArray(int tableId, object[] columns, DateTime? timeInfo, bool useClearAndLeave)
protocol.FillArray(int tableId, object[] columns, bool useClearAndLeave)
protocol.FillArray(int tableId, List<object[]> columns, DateTime? timeInfo, bool useClearAndLeave)
protocol.FillArray(int tableId, List<object[]> columns, bool useClearAndLeave)
protocol.FillArray(int tableId, List<object[]> rows, SaveOption option, DateTime? timeInfo, bool useClearAndLeave)
protocol.FillArray(int tableId, List<object[]> rows, SaveOption option, bool useClearAndLeave)
protocol.FillArrayNoDelete(int tableId, object[] columns, DateTime? timeInfo, bool useClearAndLeave)
protocol.FillArrayNoDelete(int tableId, object[] columns, bool useClearAndLeave)
protocol.FillArrayNoDelete(int tableId, List<object[]> columns, DateTime? timeInfo, bool useClearAndLeave)
protocol.FillArrayNoDelete(int tableId, List<object[]> columns, bool useClearAndLeave)
protocol.FillArrayWithColumn(int tableId, int columnPid, object[] keys, object[] values, DateTime? timeInfo, bool useClearAndLeave)
protocol.FillArrayWithColumn(int tableId, int columnPid, object[] keys, object[] values, bool useClearAndLeave)
```

The `QActionHelper` class has also been adapted.

- `protocol.Clear` and `protocol.Leave` are now supported when calling the `FillArray` methods on the `QActionTable` class objects of `SLProtocolExt`. The following methods have been added:

  ```csharp
  protocol.QActionTable.FillArray(object[] columns, DateTime? timeInfo, bool useClearAndLeave)
  protocol.QActionTable.FillArray(object[] columns, bool useClearAndLeave)
  protocol.QActionTable.FillArray(List<object> columns, DateTime? timeInfo, bool useClearAndLeave)
  protocol.QActionTable.FillArray(List<object> columns, bool useClearAndLeave)
  protocol.QActionTable.FillArray(QActionTableRow[] rows, DateTime? timeInfo, bool useClearAndLeave)
  protocol.QActionTable.FillArray(QActionTableRow[] rows, bool useClearAndLeave)
  protocol.QActionTable.FillArray(List<QActionTableRow> rows, DateTime? timeInfo, bool useClearAndLeave)
  protocol.QActionTable.FillArray(List<QActionTableRow> rows, bool useClearAndLeave)
  protocol.QActionTable.FillArrayNoDelete(object[] columns, DateTime? timeInfo, bool useClearAndLeave)
  protocol.QActionTable.FillArrayNoDelete(object[] columns, bool useClearAndLeave)
  protocol.QActionTable.FillArrayNoDelete(List<object> columns, DateTime? timeInfo, bool useClearAndLeave)
  protocol.QActionTable.FillArrayNoDelete(List<object> columns, bool useClearAndLeave)
  protocol.QActionTable.FillArrayNoDelete(QActionTableRow[] rows, DateTime? timeInfo, bool useClearAndLeave)
  protocol.QActionTable.FillArrayNoDelete(QActionTableRow[] rows, bool useClearAndLeave)
  protocol.QActionTable.FillArrayNoDelete(List<QActionTableRow> rows, DateTime? timeInfo, bool useClearAndLeave)
  protocol.QActionTable.FillArrayNoDelete(List<QActionTableRow> rows, bool useClearAndLeave)
  protocol.QActionTable.SetColumn(int columnPid, string[] keys, object[] values, DateTime? timeInfo, bool useClearAndLeave)
  protocol.QActionTable.SetColumn(int columnPid, string[] keys, object[] values, bool useClearAndLeave)
  ```

- `protocol.Clear` and `protocol.Leave` are now supported when calling the `SetRow` method on the `QActionTable` class objects of `SLProtocolExt`. The following methods have been provided:

  ```csharp
  protocol.QActionTable.AddRow(string row, DateTime? timeInfo)
  protocol.QActionTable.AddRow(object[] row, DateTime? timeInfo)
  protocol.QActionTable.AddRow(QActionTableRow row, DateTime? timeInfo)
  protocol.QActionTable.AddRowReturnKey(DateTime? timeInfo)
  protocol.QActionTable.AddRowReturnKey(object[] row, DateTime? timeInfo)
  protocol.QActionTable.AddRowReturnKey(QActionTableRow row, DateTime timeInfo)
  protocol.QActionTable.SetRow(QActionTableRow row, bool createRow, DateTime? timeInfo, bool useClearAndLeave)
  protocol.QActionTable.SetRow(QActionTableRow row, bool createRow, DateTime? timeInfo)
  protocol.QActionTable.SetRow(QActionTableRow row, bool createRow, bool useClearAndLeave)
  protocol.QActionTable.SetRow(int row, object[] data, DateTime? timeInfo, bool useClearAndLeave)
  protocol.QActionTable.SetRow(int row, object[] data, DateTime? timeInfo)
  protocol.QActionTable.SetRow(int row, object[] data, bool useClearAndLeave)
  protocol.QActionTable.SetRow(string row, object[] data, bool createRow, DateTime? timeInfo, bool useClearAndLeave)
  protocol.QActionTable.SetRow(string row, object[] data, bool createRow, DateTime? timeInfo)
  protocol.QActionTable.SetRow(string row, object[] data, bool createRow, bool useClearAndLeave)
  ```

  > [!NOTE]
  > The `AddRow` and `SetRow` methods can now also perform history sets.

#### Configuration of multiple threads for the same connection [ID_38887]

<!-- MR 10.5.0 - FR 10.4.9 -->

Control messages can now be sent in a thread of their own, which will prevent them from being blocked by ongoing polling actions on the same connections.

Previously, it was already possible to create multiple group execution queues for different connections. For example:

```xml
<Threads>
    <Thread connection="1" />
    <Thread connection="1002" />
</Threads>
```

Now you can also do this for the same connection by giving the thread an ID. Optionally, you can also give it a name, but this is currently only used for logging purposes. As a specific thread can have multiple connections linked to it, you will also need to specify the connection (by default 0). For example:

```xml
<Threads>
    <Thread id="1" name="HTTP Polling Thread" connection="0"/>
    <Thread id="2" name="HTTP Control Thread" connection="0"/>
</Threads>
```

> [!NOTE]
> If you want to use a thread definition with an ID, all thread definitions will need to have an ID. Combining thread definitions with and without ID is not supported.

You can then execute a group on a thread of your choice by specifying the thread ID on the group:

```xml
<Group id="1002" threadId="1">
```

In the element logging, the log entry for starting a thread will include the thread ID and thread name if these are defined.

For now, creating multiple threads for the same connection is **only supported for HTTP and SNMP connections**. If you try to configure this for a different kind of connection, the thread will not be created, and an entry will be added in the element logging to explain why. If you try to execute a group on a thread that has not been created for this reason, the group will be executed on the main protocol thread.

#### Protocols: Newly installed connector will automatically be promoted to production version [ID_40291]

<!-- MR 10.5.0 - FR 10.4.10 -->

Up to now, after you had installed a connector for the first time, you typically had to promote it to "production version" afterwards. From now on, when you install a connector for the first time, DataMiner will automatically promote it to "production version" and add the following entry to the *SLNet.txt* log file:

``Initial upload of protocol '{protocolName}'. Version '{protocolVersion}' will be automatically set as production.``

For example, when I install *MyConnector 1.0.0.1* for the first time, it will automatically be promoted to "production version", but when I later deploy *MyConnector 1.0.0.2*, version 1.0.0.1 will remain the production version.

A connector will only be promoted to "production version" if its first version is installed/uploaded in one of the following ways:

- When it is published from within DataMiner Integration Studio (DIS).
- When it is deployed from the [DataMiner Catalog](https://catalog.dataminer.services/).
- When its *.dmprotocol* file is uploaded from the *Protocols & Templates* app in DataMiner Cube.

> [!NOTE]
> When you install the first version of a connector by uploading its *protocol.xml* file from the *Protocols & Templates* app in DataMiner Cube (instead of its *.dmprotocol* file), the connector will not be automatically promoted to "production version".

### DataMiner modules

#### User-defined APIs: Query string support [ID_37733]

<!-- MR 10.5.0 - FR 10.4.1 -->

User-defined APIs now support the use of query strings.

The query parameters from the API requests are available in the `QueryParameters` property of the `ApiTriggerInput` class. This property is of type `IQueryParameters`.

The `IQueryParameters` class exposes the following methods:

```csharp
bool TryGetValues(string key, out List<string> values);

bool TryGetValue(string key, out string value);

List<string> GetAllKeys();

bool ContainsKey(string key);
```

> [!NOTE]
>
> - Multiple values can be added for one key.
> - Query parameter keys are case-sensitive.

#### Service & Resource Management - ResourceManagerHelper & ServiceManagerHelper: New Count methods [ID_37885] [ID_38096]

<!-- MR 10.5.0 - FR 10.4.2 -->

The *ResourceManagerHelper* and *ServiceManagerHelper* now include the following *Count* methods that will allow you to count objects using a filter.

- ServiceManagerHelper:

  - CountServiceDefinitions(filter)

- ResourceManageHelper:

  - CountResources(filter)
  - CountResourcePools(filter)
  - CountReservationInstances(filter)

Example:

```csharp
var resourceManagerHelper = new ResourceManagerHelper(engine.SendSLNetSingleResponseMessage);
var count = resourceManagerHelper.CountResources(ResourceExposers.Name.Contains("name"));
```

> [!NOTE]
> When a *Get Bookings* GQI query performs a count aggregate on ID, it will now use the new *ResourceManageHelper.CountReservationInstances* method. This will considerably enhance overall performance.

#### DataMiner Object Models: Creating, updating and deleting multiple DOM instances in one call [ID_37891]

<!-- MR 10.5.0 - FR 10.4.2 -->

From now on, the DOM API allows you to create, update or delete up to 100 DOM instances in one single call.

##### CreateOrUpdate

A `CreateOrUpdate` operation will return the IDs of the DomInstances that were successfully created or updated, as well as the IDs of the DomInstances that were not. Trace data will be available per DomInstance ID.

A `CreateOrUpdate` operation will return a list of DomInstances that were successfully created or updated. Trace data will be available per DomInstance ID.

If an issue occurs while creating or updating an item, no exception will be thrown, even when `ThrowExceptionsOnErrorData` is true. The trace data of the last call will contain the data for all items.

If the entire `CreateOrUpdate` operation would fail, a `CrudFailedException` will be thrown. If, in that case, `ThrowExceptionsOnErrorData` was set to false, null will be returned and the trace data of the last call will contain more details about the issue.

If the list returned by a `CreateOrUpdate` operation contains DomInstances sharing the same key, only the last item with that key will be considered to be created or updated.

```csharp
/// <summary>
/// Returns the created or updated `objects`.
/// </summary>
/// <param name="objects">
/// List of `DomInstances` to be created or updated.
/// The number of `DomInstances` should not exceed `MaxAmountBulkOperation`.
/// </param>
/// The result containing a list of `DomInstances` that were successfully updated or created,
/// a list of IDs of the `DomInstances` that were not successfully updated or created
/// and a list of `TraceData` per item.
/// `null` if `ThrowExceptionsOnErrorData` is `false` and there is no `BulkCreateOrUpdateResult` available.
/// </returns>
/// <exception cref="ArgumentNullException">if `objects` is `null`.</exception>
/// <exception cref="CrudFailedException">if `ThrowExceptionsOnErrorData` is `true`,  the TraceData contains errors and there is no `BulkCreateOrUpdateResult` available.</exception>
/// <exception cref="DomInstanceCrudMaxAmountExceededArgumentException">if the number of `objects` exceeds `MaxAmountBulkOperation`.</exception>
public BulkCreateOrUpdateResult<DomInstance, DomInstanceId> CreateOrUpdate(List<DomInstance> objects)
```

##### Delete

A `Delete` operation will return the IDs of the DomInstances that were deleted, as well as the IDs of the DomInstances that were not deleted. Trace data will be available per DomInstance ID.

If an issue occurs while deleting an item, no exception will be thrown, even when `ThrowExceptionsOnErrorData` is true. The trace data of the last call will contain the data for all items.

If the entire `Delete` operation would fail, a `CrudFailedException` will be thrown. If, in that case, `ThrowExceptionsOnErrorData` was set to false, null will be returned and the trace data of the last call will contain more details about the issue.

```csharp
/// <summary>
/// Deletes the given `objects`.
/// </summary>
/// <param name="objects">
/// List of `DomInstances` to be deleted.
/// The number of `DomInstances` should not exceed `MaxAmountBulkOperation`.
/// </param>
/// <returns>
/// The result containing a list of IDs of `DomInstances` that were successfully deleted
/// a list of IDs of the `DomInstances` that were not successfully deleted
/// and a list of `TraceData` per item.
/// </returns>
/// <exception cref="ArgumentNullException">if `objects` is `null`.</exception>
/// <exception cref="CrudFailedException">if `ThrowExceptionsOnErrorData` is `true`,  the TraceData contains errors and there is no `BulkDeleteResult` available.</exception>
/// <exception cref="DomInstanceCrudMaxAmountExceededArgumentException">if the amount of `objects` exceeds `MaxAmountBulkOperation`.</exception>
public BulkDeleteResult<DomInstanceId> Delete(List<DomInstance> objects)
```

##### MaxAmountBulkOperation

By default, `MaxAmountBulkOperation` will be set to 100. This means that any `CreateOrUpdate` or `Delete` operation will be able to process up to 100 DOM instances. If more than 100 instances are passed, then an error will occur.

> [!IMPORTANT]
> Since related actions (e.g. launching script actions and history tracking) might outlive a `CreateOrUpdate` or `Delete` operation, keep in mind that repeating these operations in succession can have an impact on system stability.

##### Unique IDs

When creating, updating or deleting instances using the above-mentioned bulk operations, make sure each DOM instance in the list has a unique ID.

If multiple instances share the same ID, only the last of those instances will be taken into account.

> [!NOTE]
> This also applies when PaTokens are created, updated or deleted in bulk. If multiple instances share the same ID, only the last of those instances will be taken into account.

#### DataMiner Object Models: Defining CRUD actions for DomInstances on DomDefinition level [ID_37963]

<!-- MR 10.5.0 - FR 10.4.2 -->

Using the `ExecuteScriptOnDomInstanceActionSettings` object, it is now possible to configure DomInstance CRUD actions on DomDefinition level.

The `ExecuteScriptOnDomInstanceActionSettings` object has been made available as a `ScriptSettings` property in the `ModuleSettingsOverrides` property of a DomDefinition.

> [!NOTE]
>
> - When `ScriptSettings` are filled in in the DomDefinition, these will take precedence.
> - When, in the DomDefinition, the `ScriptSettings` object is null, the `ScriptSettings` of the `ModuleSettings` will be used instead.
> - In order for the `ModuleSettings` objects to be used, the objects in the `ModuleSettingsOverrides` of the `DomDefinition` have to be *null*. Just making them empty is not sufficient.

#### DataMiner Object Models: DomInstanceHistorySettings are now also available on DomDefinition level [ID_38294]

<!-- MR 10.5.0 - FR 10.4.4 -->

`DomInstanceHistorySettings` are now also available on DomDefinition level.

The behavior is similar to that of the options in the `ModuleSettingsOverrides` property of a DomDefinition:

- When `HistorySettings` are available in the DomDefinition, these will take precedence.
- When the `HistorySettings` object in the DomDefinition is null, the `HistorySettings` of the `ModuleSettings` will be used.

> [!IMPORTANT]
> In order for the `HistorySettings` of the `ModuleSettings` to be used, the `HistorySettings` object in the DomDefinition has to be *null*. Just making the values empty is not sufficient.

#### DataMiner Object Models: New 'GetDifferences' method to compare two DOM instances [ID_38364]

<!-- MR 10.5.0 - FR 10.4.2 -->

The `DomInstanceCrudMeta` input object of a DOM CRUD script has a new `GetDifferences` method that allows you to see the changes made to a DOM instance. It will compare the previousVersion and the currentVersion of the instance in question, and return the list of differences found.

#### User-defined APIs: An event will now be sent when an ApiToken or ApiDefinition is created, updated or deleted [ID_39117]

<!-- MR 10.5.0 - FR 10.4.6 -->

From now on, the user-defined API manager will send out an event whenever an `ApiToken` or `ApiDefinition` object is created, update or deleted.

| Event name | Description |
|---|---|
| ApiTokenChangedEventMessage      | Generated when an [ApiToken](xref:UD_APIs_Objects_ApiToken) is created, updated, or deleted. |
| ApiDefinitionChangedEventMessage | Generated when an [ApiDefinition](xref:UD_APIs_Objects_ApiDefinition) is created, updated, or deleted. |

When subscribing to event messages, you can use the `SubscriptionFilter` to only receive the messages matching a specific filter. See the following example.

```csharp
// In this example, you will take the Connection object from the script's Engine object
var connection = engine.GetUserConnection();

// Create a random set ID that identifies our subscription
var setId = $"ApiTokenSubscription_{Guid.NewGuid()}"

// Create the filter for the ApiToken events, only enabled tokens should match
var filter = ApiTokenExposers.IsDisabled.Equal(false);
var subscriptionFilter = new SubscriptionFilter<ApiTokenChangedEventMessage, ApiToken>(filter);

// Attach a callback when a new event message arrives
connection.OnNewMessage += (sender, args) =>
{
    // Handle the events
}

// Subscribe
connection.AddSubscription(setId, subscriptionFilter);
```

#### Service & Resource Management: New GetFunctionDefinitions method added to ProtocolFunctionHelper class [ID_39362]

<!-- MR 10.5.0 - FR 10.4.6 -->

Up to now, it was only possible to retrieve a single function definition by ID using the *GetFunctionDefinition* method.

From now on, you can retrieve multiple function definitions in one go using the new *GetFunctionDefinitions* method.

#### Service & Resource Management: New function resource setting 'SkipDcfLinks' [ID_39446]

<!-- MR 10.5.0 - FR 10.4.8 -->

When a booking starts, DCF connections are created between the function DVEs of the assigned resources. They get deleted again when the booking finishes. From now on, the `SkipDcfLinks` setting can be used to disable this start and end action.

Default value: false.

Example of a function manager *config.xml* file containing this new setting:

```xml
<ProtocolFunctionManagerConfigInfo>
  <ActiveFunctionResourcesThreshold>100</ActiveFunctionResourcesThreshold>
  <FunctionHysteresis>PT1H</FunctionHysteresis>
  <FunctionActivationTimeout>PT1M</FunctionActivationTimeout>
  <SkipDcfLinks>true</SkipDcfLinks>
</ProtocolFunctionManagerConfigInfo>
```

For more information, see [Function resource settings](xref:Function_resource_settings)

#### DataMiner Object Models: DomInstance names now support GenericEnum fields that allow multiple values [ID_39510]

<!-- MR 10.5.0 - FR 10.4.7 -->

DomInstance names now support GenericEnum fields that allow multiple values. In other words, `list GenericEnumFieldDescriptor` will now be supported as a `FieldValueConcatenationItem` for a DomInstance name.

When you add a GenericEnumFieldDescriptor that supports multiple values to a DomDefinition NameDefinition, it will add the display names of the enum values separated by a semicolon (';').

Also, from now on, it will no longer be possible to create multiple enum values with the same values using the SLNetTypes method `AddEntry(GenericEnumEntry<T> entry)`. When you try to do so, an `InvalidOperationException` will now be thrown.

### Tools

#### SLNetClientTest tool: New SLProtocol health statistics [ID_37617]

<!-- MR 10.5.0 - FR 10.4.3 -->

When, in the *SLNetClientTest* tool, you open the *Diagnostics > DMA* menu, you can now find the following new commands:

| Command | Function |
|---------|----------|
| Health Stats (SLProtocol) > Stats      | Show the overall SLProtocol memory used by all elements. |
| Health Stats (SLProtocol) > Details... | Show all details of a specific element. |

#### Factory reset tool: Additional actions [ID_39524] [ID_39530]

<!-- MR 10.5.0 - FR 10.4.7 -->

The factory reset tool `SLReset.exe` will now perform the following additional actions:

- If the DataMiner Agent is connected to *dataminer.services*, disconnect the DataMiner Agent from *dataminer.services*.
- Clear all custom appsettings of the DataMiner extension modules (DxMs).
