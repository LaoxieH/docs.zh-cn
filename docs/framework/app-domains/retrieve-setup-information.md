---
title: "从应用程序域中检索安装信息"
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net-framework
ms.reviewer: 
ms.suite: 
ms.technology: dotnet-bcl
ms.tgt_pltfrm: 
ms.topic: article
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- AppDomainSetup object
- retrieving setup information
- application domains, retrieving setup information
ms.assetid: 5cdb12ae-1e37-4a62-8ec7-93d6dcc6e8d9
caps.latest.revision: "10"
author: rpetrusha
ms.author: ronpet
manager: wpickett
ms.workload: dotnet
ms.openlocfilehash: eeb0921eefc7ac157b94f3b6de43460cdfd42ba8
ms.sourcegitcommit: c0dd436f6f8f44dc80dc43b07f6841a00b74b23f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/19/2018
---
# <a name="retrieving-setup-information-from-an-application-domain"></a>从应用程序域中检索安装信息
应用程序域的每个实例由属性和 <xref:System.AppDomainSetup> 信息组成。 可使用 <xref:System.AppDomain?displayProperty=nameWithType> 类从应用程序域中检索安装信息。 此类提供多个成员，可用于检索应用程序域的相关配置信息。  
  
 还可以查询应用程序域的 AppDomainSetup 对象，获取创建域时传递到该域的安装信息。  
  
 下面的示例创建新的应用程序域，然后将几个成员值打印到控制台。  
  
 [!code-cpp[AppDomain_Setup#2](../../../samples/snippets/cpp/VS_Snippets_CLR/AppDomain_Setup/CPP/source2.cpp#2)]
 [!code-csharp[AppDomain_Setup#2](../../../samples/snippets/csharp/VS_Snippets_CLR/AppDomain_Setup/CS/source2.cs#2)]
 [!code-vb[AppDomain_Setup#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/AppDomain_Setup/VB/source2.vb#2)]  
  
 下面的示例设置并检索某一应用程序域的安装信息。 请注意，`AppDomain.SetupInformation.ApplicationBase` 获取配置信息。  
  
 [!code-cpp[AppDomain_Setup#3](../../../samples/snippets/cpp/VS_Snippets_CLR/AppDomain_Setup/CPP/source3.cpp#3)]
 [!code-csharp[AppDomain_Setup#3](../../../samples/snippets/csharp/VS_Snippets_CLR/AppDomain_Setup/CS/source3.cs#3)]
 [!code-vb[AppDomain_Setup#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/AppDomain_Setup/VB/source3.vb#3)]  
  
## <a name="see-also"></a>请参阅  
 [对应用程序域进行编程](http://msdn.microsoft.com/library/bd36055b-56bd-43eb-b4d8-820c37172131)  
 [使用应用程序域](../../../docs/framework/app-domains/use.md)
