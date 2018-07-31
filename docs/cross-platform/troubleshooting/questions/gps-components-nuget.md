---
title: Google Play 통합 서비스 구성 요소 및 NuGet
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5D962EB4-2CB3-4B7D-9D77-889DEACDAE02
author: asb3993
ms.author: amburns
ms.date: 05/08/2018
ms.openlocfilehash: 3f5c5f75ae1c7a44537afa59ff4a15d54b1df50b
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351485"
---
# <a name="unifying-google-play-services-components-and-nuget"></a>Google Play 통합 서비스 구성 요소 및 NuGet

## <a name="history"></a>기록

여러 Google Play 서비스 구성 요소 및 NuGet 패키지를 사용 있습니다.

-   Google Play 서비스 (Froyo)
-   Google Play 서비스 (Gingerbread)
-   Google Play 서비스 ICS)
-   Google Play 서비스 (JellyBean)
-   Google Play 서비스 (KitKat)

Google Play 서비스에 대 한 Google 실제로 두 배.jar 파일:

-   `google-play-services-froyo.jar`
-   `google-play-services.jar`

이 도구가 제대로 하지 않았기 때문에 불일치가 존재 `aapt.exe` 지정된 된 앱에 사용할을 하는 최대 리소스 API 수준입니다. 즉, Gingerbread 같은 낮은 API 수준에서 Google Play 서비스 (KitKat) 바인딩을 사용 하는 것이 노력 하는 경우 컴파일 오류가 발생 했습니다.

## <a name="unifying-google-play-services"></a>Google Play 서비스를 통합합니다.

Xamarin.Android의 최신 버전에서는 이제 알려줍니다 `aapt.exe` 우리 회사에이 문제가 계속 되므로 사용 하려면 최대 리소스 버전입니다.

그러나 즉, 이유는 없습니다 실제 Gingerbread/ICS/JellyBean/KitKat (에서는 여전히 필요는 별도 바인딩을 Froyo 용 이므로 다른.jar 파일을 모두)에 대 한 별도 패키지가 있어야 합니다.

쉽게 하려면 개발자를 위한,에서는 했으므로 이제 통합이 구성 요소 및 NuGet 패키지를 두 개:

-   Google Play 서비스 (Froyo) (바인딩합니다 `google-play-services-froyo.jar`)
-   Google Play 서비스 (바인딩합니다 `google-play-services.jar`)

### <a name="which-one-should-be-used"></a>어떤 것을 사용 해야

거의 모든 경우 Google Play 서비스를 사용 해야 합니다. (Froyo) 패키지를 사용 하는 유일한 이유는 적극적으로 Froyo를 대상으로 하는 경우. Froyo 장치의 몇 퍼센트 이므로 별도.jar 파일이 Google에서이 유일한 이유는,이.jar 파일 Google Play Services의 고정 된, 지원 되지 않는 스냅숏 이므로, 지원 하지 않기로 자체가 합니다.

### <a name="note-about-gingerbread"></a>Gingerbread에 대 한 정보

Gingerbread 기본적으로 지원 되는 조각 없고이 인해 바인딩에 클래스 중 일부를 사용할 수 없습니다 Gingerbread 장치에서 런타임 시 앱에서. 같은 클래스 `MapFragment` Gingerbread에서 작동 하지 않으며 해당 지원 variant를 대신 사용 해야 `SupportMapFragment`합니다. 알고 사용 하는 개발자에 게 됩니다. 이러한 비 호환성은 Google에서가 Google Play Services 설명서에 기록 됩니다.

### <a name="what-happens-to-the-old-componentsnugets"></a>이전 구성 요소/NuGet의 되나요?

더 이상 필요 하므로 사용 안 함/Delisted 다음 구성 요소/Nuget 없습니다.

-   Google Play 서비스 (Gingerbread)
-   Google Play 서비스 (JellyBean)
-   Google Play 서비스 (KitKat)

기존 _Google Play Services ICS ()_ 구성 요소/Nuget로 바뀌었습니다 _Google Play Services_ 및 최신 상태로 계속 유지 됩니다. 사용 안 함/Delisted 패키지 중 하나를 참조 하는 모든 프로젝트는이 사용 하도록 업데이트 되어야 합니다.

비활성화 된 구성 요소 존재 하며 해당 손상을 방지 하기 위해에서는 여전히 참조 되는 프로젝트에 대 한 복원 가능 해야 합니다. 마찬가지로 delisted NuGet 패키지를 계속 존재 하 고 복원할 수 있습니다. 이러한 업데이트 되지 않습니다 앞으로 이동 합니다.
