---
title: 2단계. 모바일 응용 프로그램에 대 한 서비스 액세스를 구성 합니다.
description: 이 문서에서는 Azure Active Directory에서 보호 되는 Azure 응용 프로그램에 대 한 액세스를 사용 하 여 Xamarin 응용 프로그램을 제공 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 8A14A457-F72E-4B08-B4B6-801F7619F893
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: e0015316b7be3462982ee0959862250c0c27dc74
ms.sourcegitcommit: 7ccc7a9223cd1d3c42cd03ddfc28050a8ea776c2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/13/2019
ms.locfileid: "67864652"
---
# <a name="step-2-configure-service-access-for-mobile-application"></a>2단계. 모바일 응용 프로그램에 대 한 서비스 액세스를 구성 합니다.

필요할 때마다 모든 예: 웹 응용 프로그램 리소스, 웹 서비스 등 Azure Active Directory에서 보호를 등록 해야 합니다. 모든 보안 응용 프로그램 또는 서비스에서 볼 수 있습니다 **응용 프로그램** 탭 합니다. 여기서 모바일 응용 프로그램에서 액세스할 수 있으며이에 액세스할 수 있도록 하는 응용 프로그램을 선택할 수 있습니다.

1. 에 **구성** 탭을 찾아 **다른 응용 프로그램에 대 한 사용 권한을** 섹션:

   ![](configure-images/2.1-configure.png "구성 탭에서 권한을 다른 응용 프로그램 섹션을 찾습니다")

2. 클릭할 **응용 프로그램을 추가** 단추입니다. 다음 화면 팝업에서 Azure Active Directory로 보호 되는 모든 응용 프로그램 목록이 표시 됩니다. 모바일 응용 프로그램에서 액세스 해야 하는 응용 프로그램을 선택 합니다.

   ![](configure-images/2.2-add-application.png "모바일 응용 프로그램에서 액세스 해야 하는 응용 프로그램 선택")

3. 응용 프로그램을 선택한 후 다시 한 번에 새로 추가 된 응용 프로그램 선택 **다른 응용 프로그램 권한을** 섹션 및 적절 한 권한을 부여 합니다.

   ![](configure-images/2.3-permissions.png "응용 프로그램을 선택한 후 다시 한 번 권한을 다른 응용 프로그램 섹션에서 새로 추가 된 응용 프로그램을 선택 하 고 적절 한 권한을 부여합니다")

4. 마지막으로, **저장할** 구성 합니다. 이러한 서비스는 모바일 응용 프로그램에서 사용할 수 있어야!



## <a name="related-links"></a>관련 링크

- [Microsoft NativeClient 샘플](https://github.com/AzureADSamples/NativeClient-MultiTarget-DotNet)
