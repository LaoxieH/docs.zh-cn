---
title: "服务和事务"
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net-framework
ms.reviewer: 
ms.suite: 
ms.technology:
- dotnet-clr
ms.tgt_pltfrm: 
ms.topic: article
helpviewer_keywords:
- service contracts [WCF], designing services and transactions
ms.assetid: 864813ff-2709-4376-912d-f5c8d318c460
caps.latest.revision: 
author: dotnet-bot
ms.author: dotnetcontent
manager: wpickett
ms.workload:
- dotnet
ms.openlocfilehash: f6256db06825a79b5235b92e2ed205608f04aac7
ms.sourcegitcommit: 16186c34a957fdd52e5db7294f291f7530ac9d24
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/22/2017
---
# <a name="services-and-transactions"></a>服务和事务
[!INCLUDE[indigo1](../../../includes/indigo1-md.md)] 应用程序可以从客户端中启动事务，然后在服务操作中协调该事务。 客户端可以启动事务和调用多个服务操作，并可确保服务操作作为一个单元提交或回滚。  
  
 通过为需要客户端事务的服务操作指定 <xref:System.ServiceModel.ServiceBehaviorAttribute> 并设置其 <xref:System.ServiceModel.ServiceBehaviorAttribute.TransactionIsolationLevel%2A> 和 <xref:System.ServiceModel.OperationBehaviorAttribute.TransactionScopeRequired%2A> 属性，可以启用服务协定中的事务行为。 <xref:System.ServiceModel.OperationBehaviorAttribute.TransactionAutoComplete%2A> 参数指定在没有引发未处理异常的情况下，在其中执行方法的事务是否自动完成。 [!INCLUDE[crabout](../../../includes/crabout-md.md)]这些属性，请参阅[ServiceModel 事务属性](../../../docs/framework/wcf/feature-details/servicemodel-transaction-attributes.md)。  
  
 在服务操作中执行并由资源管理器管理的工作（如记录数据库更新）是客户端事务的一部分。  
  
 下面的示例演示如何使用 <xref:System.ServiceModel.ServiceBehaviorAttribute> 和 <xref:System.ServiceModel.OperationBehaviorAttribute> 属性来控制服务端的事务行为。  
  
```  
[ServiceBehavior(TransactionIsolationLevel = System.Transactions.IsolationLevel.Serializable)]  
public class CalculatorService: ICalculatorLog  
{  
    [OperationBehavior(TransactionScopeRequired = true,  
                           TransactionAutoComplete = true)]  
    public double Add(double n1, double n2)  
    {  
        recordToLog(String.Format("Added {0} to {1}", n1, n2));  
        return n1 + n2;  
    }  
  
    [OperationBehavior(TransactionScopeRequired = true,   
                               TransactionAutoComplete = true)]  
    public double Subtract(double n1, double n2)  
    {  
        recordToLog(String.Format("Subtracted {0} from {1}", n1, n2));  
        return n1 - n2;  
    }  
  
    [OperationBehavior(TransactionScopeRequired = true,   
                                       TransactionAutoComplete = true)]  
    public double Multiply(double n1, double n2)  
    {  
        recordToLog(String.Format("Multiplied {0} by {1}", n1, n2));  
        return n1 * n2;  
    }  
  
    [OperationBehavior(TransactionScopeRequired = true,   
                                       TransactionAutoComplete = true)]  
    public double Divide(double n1, double n2)  
    {  
        recordToLog(String.Format("Divided {0} by {1}", n1, n2));  
        return n1 / n2;  
    }  
  
}  
```  
  
 你可以启用事务和事务流通过配置客户端和服务绑定使用 Ws-atomictransaction 协议，以及设置[ \<transactionFlow >](../../../docs/framework/configure-apps/file-schema/wcf/transactionflow.md)元素`true`，如所示在下面的示例配置。  
  
```xml  
<client>  
    <endpoint address="net.tcp://localhost/ServiceModelSamples/service"   
          binding="netTcpBinding"   
          bindingConfiguration="netTcpBindingWSAT"   
          contract="Microsoft.ServiceModel.Samples.ICalculatorLog" />  
</client>  
  
<bindings>  
    <netTcpBinding>  
        <binding name="netTcpBindingWSAT"  
                transactionFlow="true"  
                transactionProtocol="WSAtomicTransactionOctober2004" />  
     </netTcpBinding>  
</bindings>  
```  
  
 客户端可以通过创建 <xref:System.Transactions.TransactionScope>，然后在该事务范围内调用服务操作来开始该事务。  
  
```  
using (TransactionScope ts = new TransactionScope(TransactionScopeOption.RequiresNew))  
{  
    //Do work here  
    ts.Complete();  
}  
```  
  
## <a name="see-also"></a>请参阅  
 [System.ServiceModel 中的事务性支持](../../../docs/framework/wcf/feature-details/transactional-support-in-system-servicemodel.md)  
 [事务模型](../../../docs/framework/wcf/feature-details/transaction-models.md)  
 [WS 事务流](../../../docs/framework/wcf/samples/ws-transaction-flow.md)
