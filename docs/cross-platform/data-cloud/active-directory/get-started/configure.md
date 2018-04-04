---
title: 2단계. 모바일 응용 프로그램에 대 한 서비스 액세스를 구성 합니다.
ms.prod: xamarin
ms.assetid: 8A14A457-F72E-4B08-B4B6-801F7619F893
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: f530f578b297af36ae010c25c87dbffa2d31cb2b
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="step-2-configure-service-access-for-mobile-application"></a>2단계. 모바일 응용 프로그램에 대 한 서비스 액세스를 구성 합니다.

필요할 때마다 모든 예: 웹 응용 프로그램 리소스, 웹 서비스 등 Azure Active Directory에서 보안을 유지 하도록를 등록 해야 합니다. 모든 보안 응용 프로그램 또는 서비스에서 볼 수 있습니다 **응용 프로그램** 탭 합니다. 여기서 모바일 응용 프로그램에서 액세스할 수 있으며 그에 액세스할 수 있도록 하는 응용 프로그램을 선택할 수 있습니다.

1. 에 **구성** 탭, 찾기 **다른 응용 프로그램에 대 한 사용 권한을** 섹션:

  ![](configure-images/2.1-configure.png "구성 탭에서 다른 응용 프로그램 섹션에 사용 권한을 찾을합니다")

2.  클릭 **응용 프로그램을 추가** 단추입니다. 다음 화면 팝업에서 Azure Active Directory로 보호 된 모든 응용 프로그램 목록이 표시 됩니다. 모바일 응용 프로그램에서 액세스 해야 하는 응용 프로그램을 선택 합니다.

  ![](configure-images/2.2-add-application.png "모바일 응용 프로그램에서 액세스 해야 하는 응용 프로그램을 선택 합니다.")

3. 응용 프로그램을 선택한 후 다시 한 번에 새로 추가 된 응용 프로그램 선택 **다른 응용 프로그램에 대 한 사용 권한을** 섹션 및 적절 한 권한을 부여 합니다.

  ![](configure-images/2.3-permissions.png "응용 프로그램을 선택한 후 다시 한 번 권한 다른 응용 프로그램 섹션에서 새로 추가 된 응용 프로그램을 선택 하 고 적절 한 권한을 제공합니다")

4. 마지막으로, **저장** 구성 합니다. 이제 이러한 서비스 모바일 응용 프로그램에서 사용할 수 있어야!



## <a name="related-links"></a>관련 링크

- [Microsoft NativeClient 샘플](https://github.com/AzureADSamples/NativeClient-MultiTarget-DotNet)
