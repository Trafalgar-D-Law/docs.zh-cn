---
title: 访问用户数据 (Visual Basic)
ms.date: 07/20/2015
helpviewer_keywords:
- domain names [Visual Basic], retrieving
- data [Visual Basic], accessing user data
- My.User object [Visual Basic], tasks
- user data [Visual Basic], domain
- user names [Visual Basic], retrieving
- user data [Visual Basic], accessing
- login names [Visual Basic]
- examples [Visual Basic], accessing user data
ms.assetid: 32492a15-ee59-4a63-a1f1-9b24cc13140a
ms.openlocfilehash: 097006caf56072d5a6e9f2945f5969eed249849e
ms.sourcegitcommit: 3d5d33f384eeba41b2dff79d096f47ccc8d8f03d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/04/2018
ms.locfileid: "33582839"
---
# <a name="accessing-user-data-visual-basic"></a>访问用户数据 (Visual Basic)
本部分包含有关处理 `My.User` 对象的主题以及可通过该操作完成的任务。  
  
 `My.User` 对象通过返回实现 <xref:System.Security.Principal.IPrincipal> 接口的对象，提供有关登录用户的信息的访问权限。  
  
## <a name="tasks"></a>任务  
  
|到|查看|  
|--------|---------|  
|获取用户的登录名|<xref:Microsoft.VisualBasic.ApplicationServices.User.Name%2A>|  
|如果应用程序使用 Windows 身份验证，则获取用户的域名|<xref:Microsoft.VisualBasic.ApplicationServices.User.CurrentPrincipal>|  
|确定用户的角色|<xref:Microsoft.VisualBasic.ApplicationServices.User.IsInRole%2A>|  
  
## <a name="see-also"></a>请参阅  
 <xref:Microsoft.VisualBasic.ApplicationServices.User>
