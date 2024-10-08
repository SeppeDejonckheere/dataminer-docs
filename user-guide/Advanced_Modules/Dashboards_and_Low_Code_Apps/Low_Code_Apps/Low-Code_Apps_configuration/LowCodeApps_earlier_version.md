---
uid: LowCodeApps_earlier_version
---

# Retrieving an earlier version of a low-code app

When you make changes to an existing low-code app, the previous version will be kept in the system. Up to 15 versions of an app can be stored this way.

To go back to a previous version of a low-code app:

1. Open the application from the DataMiner landing page (see [Accessing the Low-Code Apps module](xref:Accessing_custom_apps)).

1. Access the version history of the app:

   - From DataMiner 10.3.0 [CU18]/10.4.0 [CU6]/10.4.9 onwards<!--RN 40077-->, click the ellipsis button ("...") in the top-right corner and select *Versions*.

   - Prior to DataMiner 10.3.0 [CU18]/10.4.0 [CU6]/10.4.9, click the user icon in the top-right corner and select *Versions*.

   This will show the version history of the app.

   ![Version history](~/user-guide/images/Version_History.png)<br>*Low-Code App version history in DataMiner 10.4.6*

   - The currently published version, if any, is indicated with a green *Published* label. <!-- RN 32200 -->

   - The current draft is indicated with an orange *Draft* label.

   - No label is displayed for old draft versions that were never published.

1. Click the version you want to go back to. You can also click the pencil icon next to that version in the list to immediately start editing it.

> [!NOTE]
> There can only be one published version of a specific application at the same time. If you publish a different version, the previous version will no longer be published.
