---
title: Azure Active Directory
description: 이 문서에서는 모바일 앱이 Azure Active Directory 인증을 허용 하기 위해 따라야 하는 단계를 설명 합니다.
ms.prod: xamarin
ms.assetid: 70B3C2AB-CB4D-420C-9CFA-20CCFA0E3C78
author: conceptdev
ms.author: crdun
ms.date: 03/23/2017
ms.openlocfilehash: b28fcea37d991879df0231609d09eeb2eca49505
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70766346"
---
# <a name="azure-active-directory"></a>Azure Active Directory

_Azure Active Directory 사용할 앱 등록_

Azure Active Directory를 통해 개발자는 직원 들이 시스템에 로그인 하거나 전자 메일을 확인 하는 데 사용 하는 것과 동일한 조직 계정을 사용 하 여 파일, 링크 및 웹 Api와 같은 리소스를 보호할 수 있습니다.

Azure Active Directory 인증할 수 있는 모바일 응용 프로그램 개발에는 세 단계가 포함 됩니다.
처음 두 단계는 사용 하려는 서비스에 관계 없이 일반적으로 동일 합니다. 세 번째 단계는 각 서비스 유형에 따라 다릅니다.

  1. *Windowsazure.com* 포털에서 [Azure Active Directory 등록](~/cross-platform/data-cloud/active-directory/get-started/register.md)
  2. [서비스를 구성](~/cross-platform/data-cloud/active-directory/get-started/configure.md)합니다.
  3. 서비스를 사용 하 여 모바일 앱을 개발 합니다.

액세스할 수 있는 다른 서비스의 예는 다음과 같습니다.

- [Graph API](~/cross-platform/data-cloud/active-directory/graph.md)
- Web API
- Office365

## <a name="conclusion"></a>결론

위의 단계를 사용 하 여 Azure Active Directory에 대해 모바일 앱을 인증할 수 있습니다. Active Directory 인증 라이브러리 (ADAL)를 사용 하면 코드의 줄 수를 줄이고, 대부분의 코드를 동일 하 게 유지 하는 동시에 플랫폼 간에 공유할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [Microsoft NativeClient 샘플](https://github.com/AzureADSamples/NativeClient-MultiTarget-DotNet)
