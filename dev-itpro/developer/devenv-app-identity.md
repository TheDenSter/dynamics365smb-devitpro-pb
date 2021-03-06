---
title: "App Identity"
description: "Description of what makes up the app identity of an app for Business Central."
author: SusanneWindfeldPedersen
ms.custom: na
ms.date: 04/01/2021
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.service: "dynamics365-business-central"
ms.author: solsen
---

# App Identity

Apps built using AL extend the functionality of [!INCLUDE[prod_short](../includes/prod_short.md)]. The `app.json` file is, together with the `launch.json` file, automatically generated when you create a new AL project. The `app.json` file contains information about the app that you are building, such as publisher information and specifies the minimum version of base application objects that the extension is built on. Often the `app.json` file is referred to as the *manifest*. The `app.json` file contains numerous project settings, but a few of them constitutes the actual identity of the app that you are creating.

|Setting|Example|Description|
|-------|------|-----|
|`id`   |`"id": "ef4dabfc-1de7-4d90-b948-4a9c2933d794"`| The `id`, also known as the app ID. This is a GUID which is auto-generated when the project is created. The app ID is also bound to how tables are named in [!INCLUDE[prod_short](../includes/prod_short.md)] and how the identity of an application is computed. Changing the app ID may have severe consequences, such as the app not functioning properly, or data not being available.|
|`name`|`"name": "Partner X extension"`|The unique app name. <br>**Note:** The name can be used by other apps to express a compile-time dependency on the app. Changing the name of your app will force any apps that have taken a dependency to update their manifest and can cause upgrade/extension deployment issues. |
|`publisher`|`"publisher": "Business Central Partner X"`|The name of your publisher, for example: **NAV Partner**, **LLC**. <br>**Note:** The publisher can be used by other apps to express a compile-time dependency on the app. Changing the publisher of your app will force any apps that have taken a dependency to update their manifest and can cause upgrade and/or app deployment issues. |
|`version`|`"version": "1.0.0.0"`| The version is used to distinguish between different iterations of your app. The version number should increase as you make changes to your app.|

For more settings, see [JSON Files](devenv-json-files.md).

For apps published in the `Global` scope, see [Publish NAVApp](/powershell/module/microsoft.dynamics.nav.apps.management/publish-navapp?view=businesscentral-ps-17), such as AppSource and 1st party applications, the `id` and the `version`, and the `name`, `publisher`, and `version` identify a unique application package. The [!INCLUDE[prod_short](../includes/prod_short.md)] service uses these tuples to refer to apps in different flows. To prevent issues, it is recommended that the `id`, `name`, and `publisher` remain the same after an app is uploaded to the [!INCLUDE[prod_short](../includes/prod_short.md)] service, and that you only increment the `version`.

For apps published in the `Tenant` scope, see [Publish NAVApp](/powershell/module/microsoft.dynamics.nav.apps.management/publish-navapp?view=businesscentral-ps-17), such as per-tenant customizations, in addition to the `id`, `name`,`publisher`,`version`, the `tenant ID` is also used to uniquely identify an app.

## When is it okay to change the ID of an app?

The `id` of an app is automatically generated by the AL Language extension when you create a new app or if you use the **AL: Generate manifest** command. 

If you have copied the app or the manifest from another app, you must change the `id` before publishing it to the online service as a per-tenant extension or AppSource app.

After the app has been published, you should only change the `id` if you intend to use the code base to develop a new app. You will not be able to upgrade from the app with the old `id` to the app with the new `id` because the system does not have knowledge about the correspondence.

If you have published your app as a per-tenant extension, but you are now considering publishing it to AppSource, you must assign a new `id` to the AppSource app, as well as ensure that it follows all the technical requirements for publishing to AppSource.

It is recommended to use a different `id` for the app that you publish from Visual Studio Code or to the container. Once you are satisfied with the quality of your app and ready to publish it to AppSource, it is recommended to use a different `id`. If you do not follow this approach, the app that you have published from Visual Studio Code to a developer sandbox will be automatically unpublished if another user tries to install the AppSource app.

## When is it okay to change the name of an app?

The `name` of an app can be changed at any point before it is published to the online service as a per-tenant extension or as an AppSource app. Once the app has been published, you must not change the name because any other app that has taken a dependency on your app, will then not compile against the newest version of your app.

## When is it okay to change the publisher of an app?

The `publisher` of an app can be changed at any point before it is published to the online service as a per-tenant extension or as an AppSource app. Once the app has been published you must not change the publisher because any other app that has taken a dependency on your app, will then not compile against the newest version of your app.

## When is it okay to change the version of an app?

The `version` must be incremented any time a new version of your app is uploaded to AppSource or as a per-tenant extension. While developing it in Visual Studio Code, you can keep using the same version and iterate on your code.

> [!NOTE]  
> In a Visual Studio Code workspace an app's `name`, `publisher`, and `version` are part of identifying a project and a project dependency. Therefore, if any of these properties change, it is recommended that you reload the workspace.
 
## See Also

[JSON Files](devenv-json-files.md)  
[Publish NAVApp](/powershell/module/microsoft.dynamics.nav.apps.management/publish-navapp?view=businesscentral-ps-17)  
