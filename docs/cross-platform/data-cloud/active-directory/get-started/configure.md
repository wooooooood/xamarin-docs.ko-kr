---
title: 2단계. 모바일 응용 프로그램에 대 한 서비스 액세스 구성
description: 이 문서에서는 Azure Active Directory으로 보안이 유지 되는 Azure 응용 프로그램에 대 한 액세스 권한이 있는 Xamarin 응용 프로그램을 제공 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 8A14A457-F72E-4B08-B4B6-801F7619F893
author: davidortinau
ms.author: daortin
ms.date: 03/23/2017
ms.openlocfilehash: eeac7b0b70b2f11304a374de7522f28d4bcad6c6
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73016665"
---
# <a name="step-2-configure-service-access-for-mobile-application"></a>2단계. 모바일 응용 프로그램에 대 한 서비스 액세스 구성

모든 리소스 (예: 웹 응용 프로그램, 웹 서비스 등)를 Azure Active Directory를 통해 보호 해야 하는 경우에는 등록 해야 합니다. **응용 프로그램** 탭에서 모든 보안 응용 프로그램 또는 서비스를 볼 수 있습니다. 여기에서 모바일 응용 프로그램에서 액세스 해야 하는 응용 프로그램을 선택 하 고 액세스 권한을 부여할 수 있습니다.

1. **구성** 탭에서 **다른 응용 프로그램에 대 한 사용 권한** 섹션을 찾습니다.

   ![](configure-images/2.1-configure.png "On the Configure tab, locate permissions to other applications section")

2. **응용 프로그램 추가** 단추를 클릭 합니다. 다음 화면 팝업에서 Azure Active Directory으로 보안이 유지 되는 모든 응용 프로그램 목록이 표시 됩니다. 모바일 응용 프로그램에서 액세스 해야 하는 응용 프로그램을 선택 합니다.

   ![](configure-images/2.2-add-application.png "Select the applications that needs to be accessed from the mobile application")

3. 응용 프로그램을 선택한 후에는 **다른 응용 프로그램에 대 한 사용 권한** 섹션에서 새로 추가 된 응용 프로그램을 선택 하 고 적절 한 권한을 부여 합니다.

   ![](configure-images/2.3-permissions.png "After selecting the application, once again select the newly-added application in permissions to other   applications section and give appropriate rights")

4. 마지막으로 구성을 **저장** 합니다. 이제 모바일 응용 프로그램에서 이러한 서비스를 사용할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [Microsoft NativeClient 샘플](https://github.com/AzureADSamples/NativeClient-MultiTarget-DotNet)
