---
title: "了解生成的客户端代码"
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net-framework
ms.reviewer: 
ms.suite: 
ms.technology: dotnet-clr
ms.tgt_pltfrm: 
ms.topic: article
dev_langs:
- csharp
- vb
ms.assetid: c3f6e4b0-1131-4c94-aa39-a197c5c2f2ca
caps.latest.revision: "9"
author: dotnet-bot
ms.author: dotnetcontent
manager: wpickett
ms.workload: dotnet
ms.openlocfilehash: 4c57469b61a12ff5043632cf2b6f4fe3a8a53d56
ms.sourcegitcommit: 16186c34a957fdd52e5db7294f291f7530ac9d24
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/22/2017
---
# <a name="understanding-generated-client-code"></a>了解生成的客户端代码
[ServiceModel Metadata Utility Tool (Svcutil.exe)](../../../../docs/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe.md) 可生成用于生成客户端应用程序的客户端代码和客户端应用程序配置文件。 本主题提供了有关标准服务协定方案的生成代码示例的教程。 [!INCLUDE[crabout](../../../../includes/crabout-md.md)] 使用生成的代码生成客户端应用程序的详细信息，请参阅 [WCF Client Overview](../../../../docs/framework/wcf/wcf-client-overview.md)。  
  
## <a name="overview"></a>概述  
 如果使用 [!INCLUDE[vsprvs](../../../../includes/vsprvs-md.md)] 来生成项目的 [!INCLUDE[indigo1](../../../../includes/indigo1-md.md)] 客户端类型，通常并不需要检查生成的客户端代码。 如果您并未使用为您执行相同服务的开发环境，您可以使用 Svcutil.exe 之类的工具来生成客户端代码，然后使用该代码开发客户端应用程序。  
  
 由于 Svcutil.exe 具有许多可以修改生成的类型信息的选项，因此本主题并不对所有方案进行讨论。 但是，以下标准任务涉及查找生成的代码：  
  
-   标识服务协定接口。  
  
-   标识 [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] 客户端类。  
  
-   标识数据类型。  
  
-   标识双工服务的回调协定。  
  
-   标识帮助器服务协定通道接口。  
  
### <a name="finding-service-contract-interfaces"></a>查找服务协定接口  
 若要查找对服务协定建模的接口，请搜索使用 <xref:System.ServiceModel.ServiceContractAttribute?displayProperty=nameWithType> 属性进行标记的接口。 由于存在其他属性 (Attribute) 和设置在该属性 (Attribute) 自身上的显式属性 (Property)，因此要通过快速读取查找该属性 (Attribute) 通常并不容易。 请记住，服务协定接口和客户端协定接口属于两种不同的类型。 下面的代码示例演示原始服务协定。  
  
 [!code-csharp[C_GeneratedCodeFiles#22](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_generatedcodefiles/cs/proxycode.cs#22)]  
  
 下面的代码示例演示 Svcutil.exe 生成的相同服务协定。  
  
 [!code-csharp[C_GeneratedCodeFiles#12](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_generatedcodefiles/cs/proxycode.cs#12)]  
  
 您可以使用生成的服务协定接口和 <xref:System.ServiceModel.ChannelFactory?displayProperty=nameWithType> 类一起创建 [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] 通道对象，使用该通道对象可调用服务操作。 [!INCLUDE[crdefault](../../../../includes/crdefault-md.md)][如何： 使用 ChannelFactory](../../../../docs/framework/wcf/feature-details/how-to-use-the-channelfactory.md)。  
  
### <a name="finding-wcf-client-classes"></a>查找 WCF 客户端类  
 若要查找实现您要使用的服务协定的 [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] 客户端类，请搜索 <xref:System.ServiceModel.ClientBase%601?displayProperty=nameWithType> 的扩展名，其中类型参数为您先前查找到的且扩展该接口的服务协定接口。 下面的代码示例演示类型为 <xref:System.ServiceModel.ClientBase%601> 的 `ISampleService`类。  
  
 [!code-csharp[C_GeneratedCodeFiles#14](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_generatedcodefiles/cs/proxycode.cs#14)]  
  
 您可以使用此 [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] 客户端类，方法是创建它的一个新实例并调用它实现的方法。 这些方法调用服务操作，可设计和配置该服务操作以进行交互。 [!INCLUDE[crdefault](../../../../includes/crdefault-md.md)][WCF 客户端概述](../../../../docs/framework/wcf/wcf-client-overview.md)。  
  
> [!NOTE]
>  SvcUtil.exe 在生成 WCF 客户端类时会将一个 <xref:System.Diagnostics.DebuggerStepThroughAttribute> 添加到该客户端类，以防止调试器逐步调试该 WCF 客户端类。  
  
### <a name="finding-data-types"></a>查找数据类型  
 若要在生成的代码中查找数据类型，最简单的方法就是标识协定中指定的类型名称并搜索该类型声明的代码。 例如，下面的协定指定 `SampleMethod` 可以返回 `microsoft.wcf.documentation.SampleFault`类型的 SOAP 错误。  
  
 [!code-csharp[C_GeneratedCodeFiles#11](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_generatedcodefiles/cs/proxycode.cs#11)]  
  
 搜索 `SampleFault` 查找下面的类型声明。  
  
 [!code-csharp[C_GeneratedCodeFiles#30](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_generatedcodefiles/cs/proxycode.cs#30)]  
  
 在本示例中，数据类型为客户端上的特定异常（一个 <xref:System.ServiceModel.FaultException%601> ，其中详细信息类型为 `microsoft.wcf.documentation.SampleFault`）引发的详细信息类型。 [!INCLUDE[crabout](../../../../includes/crabout-md.md)] 数据类型，请参阅 [Specifying Data Transfer in Service Contracts](../../../../docs/framework/wcf/feature-details/specifying-data-transfer-in-service-contracts.md)。 [!INCLUDE[crabout](../../../../includes/crabout-md.md)] 在客户端处理异常，请参阅 [Sending and Receiving Faults](../../../../docs/framework/wcf/sending-and-receiving-faults.md)。  
  
### <a name="finding-callback-contracts-for-duplex-services"></a>查找双工服务的回调协定  
 如果要查找协定接口为其 <xref:System.ServiceModel.ServiceContractAttribute.CallbackContract%2A?displayProperty=nameWithType> 属性指定一个值的服务协定，则该协定指定一个双工协定。 双工协定要求客户端应用程序创建一个回调类，该类实现回调协定并将此类的一个实例传递给 <xref:System.ServiceModel.DuplexClientBase%601?displayProperty=nameWithType> 或用来与服务进行通信的 <xref:System.ServiceModel.DuplexChannelFactory%601?displayProperty=nameWithType>。 [!INCLUDE[crabout](../../../../includes/crabout-md.md)]双工客户端，请参阅[如何： 使用双工协定访问服务](../../../../docs/framework/wcf/feature-details/how-to-access-services-with-a-duplex-contract.md)。  
  
 下面的协定指定一个 `SampleDuplexHelloCallback`类型的回调协定。  
  
 [!code-csharp[C_GeneratedCodeFiles#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_generatedcodefiles/cs/duplexproxycode.cs#2)]
 [!code-vb[C_GeneratedCodeFiles#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_generatedcodefiles/vb/duplexproxycode.vb#2)]  
  
 搜索该回调协定定位下面的接口，客户端应用程序必须实现该接口。  
  
 [!code-csharp[C_GeneratedCodeFiles#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_generatedcodefiles/cs/duplexproxycode.cs#4)]
 [!code-vb[C_GeneratedCodeFiles#4](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_generatedcodefiles/vb/duplexproxycode.vb#4)]  
  
### <a name="finding-service-contract-channel-interfaces"></a>查找服务协定通道接口  
 当 <xref:System.ServiceModel.ChannelFactory> 类与服务协定接口一起使用时，您必须强制转换到 <xref:System.ServiceModel.IClientChannel?displayProperty=nameWithType> 接口以显式打开、关闭或中止通道。 为便于操作，Svcutil.exe 工具还生成一个帮助器接口，该接口可同时实现服务协定接口和 <xref:System.ServiceModel.IClientChannel> ，以使您无需进行强制转换就能够与客户端通道基础结构进行交互。 下面的代码演示实现上述服务协定的帮助器客户端通道的定义。  
  
 [!code-csharp[C_GeneratedCodeFiles#13](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_generatedcodefiles/cs/proxycode.cs#13)]  
  
## <a name="see-also"></a>请参阅  
 [WCF 客户端概述](../../../../docs/framework/wcf/wcf-client-overview.md)
