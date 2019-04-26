---
title: Azure Active Directory
description: 이 문서에서는 Azure Active Directory를 사용 하 여 인증 하는 모바일 앱을 허용 하려면 준수 해야 하는 단계를 설명 합니다.
ms.prod: xamarin
ms.assetid: 70B3C2AB-CB4D-420C-9CFA-20CCFA0E3C78
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: ca33422817f19dbb0a04e8870800d3f5efa8af2a
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61318300"
---
# <a name="azure-active-directory"></a>Azure Active Directory

_Azure Active Directory를 사용 하도록 앱 등록_

Azure Active Directory에는 파일, 링크 및 직원 사용 하 여 해당 시스템에 로그인 하거나 해당 전자 메일을 확인 하는 동일한 조직 계정을 사용 하 여 Web Api와 같은 보안 리소스에 개발자가 있습니다.

Azure Active Directory를 사용 하 여 인증할 수 있는 모바일 응용 프로그램을 개발 세 가지 단계가 포함 됩니다.
처음 두 단계는 일반적으로 사용 하려는 서비스에 관계 없이 동일 합니다. 세 번째 단계는 각 서비스 유형에 대해 다릅니다.

  1. [Azure Active Directory를 사용 하 여 등록](~/cross-platform/data-cloud/active-directory/get-started/register.md) 에 *windowsazure.com* 다음 포털
  2. [서비스 구성](~/cross-platform/data-cloud/active-directory/get-started/configure.md)합니다.
  3. 서비스를 사용 하 여 모바일 앱을 개발 합니다.

다른 서비스에 액세스할 수 있습니다의 예입니다.

- [Graph API](~/cross-platform/data-cloud/active-directory/graph.md)
- Web API
- Office365


## <a name="conclusion"></a>결론

위의 단계를 사용 하 여 Azure Active Directory에 대해 모바일 앱을 인증할 수 있습니다. Active Directory 인증 라이브러리 (ADAL)를 사용 하면 손쉽게 더 적은 줄의 코드를 사용 하 여 대부분의 코드를 유지 하면서 하므로 공유할 수 있는 플랫폼 및 동일 합니다.



## <a name="related-links"></a>관련 링크

- [Microsoft NativeClient 샘플](https://github.com/AzureADSamples/NativeClient-MultiTarget-DotNet)
