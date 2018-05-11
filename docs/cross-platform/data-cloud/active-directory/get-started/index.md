---
title: Azure Active Directory
description: Azure Active Directory를 사용 하도록 앱을 등록 합니다.
ms.prod: xamarin
ms.assetid: 70B3C2AB-CB4D-420C-9CFA-20CCFA0E3C78
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: decc069bd2fe31d54c886793ae4c94935b23ad02
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/10/2018
---
# <a name="azure-active-directory"></a>Azure Active Directory

_Azure Active Directory를 사용 하도록 앱을 등록 합니다._

Azure Active Directory에는 파일, 링크 및 직원이 사용 하 여 시스템에 로그인 하거나 자신의 전자 메일을 확인 하는 동일한 조직 계정을 사용 하 여 웹 Api와 같은 보안 리소스에 대 한 개발자가 있습니다.

Azure Active Directory로 인증할 수 있는 모바일 응용 프로그램 개발 세 단계가 포함 됩니다.
처음 두 단계는 일반적으로 사용 하려는 서비스에 관계 없이 동일 합니다. 세 번째 단계는 각 서비스 유형에 대 한 다릅니다.

  1. [Azure Active Directory에 등록](~/cross-platform/data-cloud/active-directory/get-started/register.md) 에 *windowsazure.com* 다음 포털
  2. [서비스 구성](~/cross-platform/data-cloud/active-directory/get-started/configure.md)합니다.
  3. 서비스를 사용 하 여 모바일 앱을 개발 합니다.

다음과 같은 다양 한 서비스에 액세스할 수 있습니다.

- [Graph API](~/cross-platform/data-cloud/active-directory/graph.md)
- Web API
- Office365


## <a name="conclusion"></a>결론

위의 단계를 사용 하 여 Azure Active Directory에 대해 모바일 앱을 인증할 수 있습니다. Active Directory 인증 라이브러리 (ADAL) 훨씬 쉽게 적은 줄만 작성 코드의 대부분의 코드를 유지 하면서 동일 하 고 있기 때문 공유 가능한 플랫폼입니다.



## <a name="related-links"></a>관련 링크

- [Microsoft NativeClient 샘플](https://github.com/AzureADSamples/NativeClient-MultiTarget-DotNet)
