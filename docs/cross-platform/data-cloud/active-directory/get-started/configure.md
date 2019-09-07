---
title: 2단계. 모바일 응용 프로그램에 대 한 서비스 액세스 구성
description: 이 문서에서는 Azure Active Directory으로 보안이 유지 되는 Azure 응용 프로그램에 대 한 액세스 권한이 있는 Xamarin 응용 프로그램을 제공 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 8A14A457-F72E-4B08-B4B6-801F7619F893
author: conceptdev
ms.author: crdun
ms.date: 03/23/2017
ms.openlocfilehash: ec5dd15ffb838d7062c8c769375289e7b07b24d2
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70766376"
---
# <a name="step-2-configure-service-access-for-mobile-application"></a>2단계. 모바일 응용 프로그램에 대 한 서비스 액세스 구성

모든 리소스 (예: 웹 응용 프로그램, 웹 서비스 등)를 Azure Active Directory를 통해 보호 해야 하는 경우에는 등록 해야 합니다. **응용 프로그램** 탭에서 모든 보안 응용 프로그램 또는 서비스를 볼 수 있습니다. 여기에서 모바일 응용 프로그램에서 액세스 해야 하는 응용 프로그램을 선택 하 고 액세스 권한을 부여할 수 있습니다.

1. **구성** 탭에서 **다른 응용 프로그램에 대 한 사용 권한** 섹션을 찾습니다.

   ![](configure-images/2.1-configure.png "구성 탭에서 다른 응용 프로그램에 대 한 사용 권한 섹션을 찾습니다.")

2. **응용 프로그램 추가** 단추를 클릭 합니다. 다음 화면 팝업에서 Azure Active Directory으로 보안이 유지 되는 모든 응용 프로그램 목록이 표시 됩니다. 모바일 응용 프로그램에서 액세스 해야 하는 응용 프로그램을 선택 합니다.

   ![](configure-images/2.2-add-application.png "모바일 응용 프로그램에서 액세스 해야 하는 응용 프로그램을 선택 합니다.")

3. 응용 프로그램을 선택한 후에는 **다른 응용 프로그램에 대 한 사용 권한** 섹션에서 새로 추가 된 응용 프로그램을 선택 하 고 적절 한 권한을 부여 합니다.

   ![](configure-images/2.3-permissions.png "응용 프로그램을 선택한 후에는 다른 응용 프로그램에 대 한 사용 권한 섹션에서 새로 추가 된 응용 프로그램을 선택 하 고 적절 한 권한을 부여 합니다.")

4. 마지막으로 구성을 **저장** 합니다. 이제 모바일 응용 프로그램에서 이러한 서비스를 사용할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [Microsoft NativeClient 샘플](https://github.com/AzureADSamples/NativeClient-MultiTarget-DotNet)
