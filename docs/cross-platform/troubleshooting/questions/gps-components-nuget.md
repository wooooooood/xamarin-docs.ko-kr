---
title: Google Play를 통합 서비스 구성 요소 및 NuGet
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5D962EB4-2CB3-4B7D-9D77-889DEACDAE02
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: e0ba5ee9417917b834ab060a94f72d1f071b4912
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="unifying-google-play-services-components-and-nuget"></a>Google Play를 통합 서비스 구성 요소 및 NuGet

### <a name="history"></a>기록

여러 Google 재생 서비스 구성 요소와 NuGet 패키지가 수 사용.

-   Google Play 서비스 (Froyo)
-   Google Play 서비스 (인형)
-   Google Play 서비스 ICS)
-   Google Play 서비스 (JellyBean)
-   Google Play 서비스 (KitKat)

Google Play 서비스에 대 한 Google만 실제로 두 배.jar 파일:

-   `google-play-services-froyo.jar`
-   `google-play-services.jar`

불일치 우리의 도구가 제대로 하지 않았기 때문에 있던 `aapt.exe` 지정된 된 앱에 사용할 였습니까 최대 리소스 API 레벨입니다. 즉, Google Play 서비스 (KitKat) 바인딩 인형 같은 낮은 API 수준에서 사용 하 여 하려고 하는 경우 컴파일 오류를 수신 했습니다.

### <a name="unifying-google-play-services"></a>Google Play 서비스를 통합합니다.

Xamarin.Android의 최신 버전에서 이제 위치도 제공 `aapt.exe` 를 사용 하려면,이 문제를 사라진 최대 리소스 버전입니다.

그러나 즉, 없기 실제 인형/ICS/JellyBean/KitKat (우리 여전히 필요는 별도 바인딩을 Froyo에 대 한 완전히 다른.jar 파일에 있기 때문)에 대 한 별도 패키지를 가져야 합니다.

쉽게 하려면 개발자를 위한, 우리 했으므로 이제 통합이 구성 요소와 NuGet 패키지를 두 개로:

-   Google Play 서비스 (Froyo) (바인딩합니다 `google-play-services-froyo.jar`)
-   Google Play 서비스 (바인딩합니다 `google-play-services.jar`)

### <a name="which-one-should-be-used"></a>어느 것을 사용할지?

거의 모든 경우에 Google Play 서비스를 사용 해야 합니다. (Froyo) 패키지를 사용 하는 유일한 이유는 적극적으로 Froyo를 대상으로 하는 경우. 장치의 작은 비율 Froyo 되어 있으므로이 별도.jar 파일이 Google에서 유일한 이유는,을 지원 하므로이.jar 파일은 지원 되지 않는 Google Play 서비스 스냅숏이 고정된 하지 않기로 자체가 합니다.

### <a name="note-about-gingerbread"></a>인형에 대 한 참고

인형에 기본적으로 지원 되는 조각 없고이 인해 바인딩에 클래스 중 일부를 사용할 수 없습니다 인형 장치에서 런타임에 응용 프로그램입니다. 클래스가 `MapFragment` 인형에서 작동 하지 않으며 해당 지원 variant를 대신 사용 해야 `SupportMapFragment`합니다. 것은 개발자가 사용할를 알 수 있습니다. 이러한 비 호환성은 Google에서 Google Play 서비스 문서에 기록 됩니다.

### <a name="what-happens-to-the-old-componentsnugets"></a>이전 구성 요소/NuGet의는 어떻게 됩니까?

필요 없게 된 이후 사용 안 함/Delisted 다음 구성 요소/NuGets 해야 합니다.

-   Google Play 서비스 (인형)
-   Google Play 서비스 (JellyBean)
-   Google Play 서비스 (KitKat)

기존 _Google 재생 서비스 (ICS)_ 구성 요소/Nuget로 바뀌었습니다 _Google Play 서비스_ 및 최신 앞으로 유지 됩니다. 사용 안 함/Delisted 패키지 중 하나를 참조 하는 모든 프로젝트는이 사용 하도록 업데이트 되어야 합니다.

비활성화 된 구성 요소는 여전히 존재 하 고는 여전히에서 참조를 해제 하지 않으려면 프로젝트에 대 한 복원 가능 있어야 합니다. 마찬가지로 delisted NuGet 패키지 존재 하며 복원 될 수 있습니다. 업데이트 되지 않습니다 앞으로 이동 합니다.
