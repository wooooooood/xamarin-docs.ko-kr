---
title: "통합 된 API에는 기존 앱 업데이트"
ms.topic: article
ms.prod: xamarin
ms.assetid: 8A654C95-5DCA-4BB5-A582-F96C2BECC81C
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: b1b6338494b9be98e677cf9d338410eae759feb8
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="updating-existing-apps-to-the-unified-api"></a>통합 된 API에는 기존 앱 업데이트

> [!IMPORTANT]
> **클래식 프로필 사용 중단:** 클래식 프로필 (monotouch.dll)에서 기능을 사용 하지 않으려는 점진적으로 시작 되며 새 플랫폼 Xamarin.iOS에 추가 됩니다. 예를 들어 비 NRC (새-ref-개수) 옵션이 제거 되었습니다. NRC 모두 통합 된 응용 프로그램을 항상 사용할 수 있는 (즉, 비 NRC 않은 옵션) 및에 알려진된 문제가 없습니다. 이후 릴리스에서 가비지 수집기 Boehm를 사용 하는 옵션이 제거 됩니다. 절대 통합 된 응용 프로그램에 사용할 수 없는 옵션 이기도 합니다. 클래식 지원을 완전히 제거할 Xamarin.iOS 10.0 릴리스와 함께 다음 지원은 예약 됩니다.




## <a name="how-to-update-your-apps"></a>앱을 업데이트 하는 방법

Xamarin University 무료로 사용할 수 있는 비디오에 **iOS 통합 API로 업그레이드**합니다. 방문 [Xamarin 대학 번개 강의](http://university.xamarin.com/lightninglectures) 조사할!

[ ![](updating-apps-images/xamu-video-sml.png "Xamarin University")](http://university.xamarin.com/lightninglectures)

앱을 업데이트 하는 세 단계 가지가 있습니다.

1. 컴파일러 경고를 모두 수정 기존 코드에서 사용 되지 않는 Api와 관련 된 특히 합니다.

2. Mac 용 Visual Studio에 기본 제공 마이그레이션 도구를 사용 하 여 프로젝트 파일 및 네임 스페이스를 업데이트 합니다.

3. 컴파일러 오류와 관련 된 새로운 남은 수정 [64 형식](~/cross-platform/macios/nativetypes.md) 및 [다른 Api](~/cross-platform/macios/unified/index.md#deprecated-typos) 변경 된 합니다. 체크 아웃 [이러한 팁](~/cross-platform/macios/unified/updating-tips.md) 필요할 수 있는 수동 업데이트에 대 한 자세한 내용은 합니다.

통합 API 및 64 비트 지원 앱을 업데이트할 수 있도록 각 제품에 대 한 특정 지침이 있습니다.

### <a name="xamarinios-appscross-platformmaciosunifiedupdating-ios-appsmd"></a>[Xamarin.iOS apps](~/cross-platform/macios/unified/updating-ios-apps.md)

기본적으로 Visual Studio로 Mac. 자동된 마이그레이션 도구를 사용 하 여 통합 API 하도록 기존 Xamarin.iOS 앱을 업데이트할 수 있습니다. 몇 가지 추가 수정 후 필요할 수 있습니다에 설명 된 대로 [이러한 지침](~/cross-platform/macios/unified/updating-ios-apps.md) 및 [팁](~/cross-platform/macios/unified/updating-tips.md)합니다.

###  <a name="xamarinmac-appscross-platformmaciosunifiedupdating-mac-appsmd"></a>[Xamarin.Mac apps](~/cross-platform/macios/unified/updating-mac-apps.md)

기본적으로 Visual Studio로 Mac. 자동된 마이그레이션 도구를 사용 하 여 통합 API 하도록 기존 Xamarin.Mac 응용 프로그램을 업데이트할 수 있습니다. 몇 가지 추가 수정 후 필요할 수 있습니다에 설명 된 대로 [이러한 지침](~/cross-platform/macios/unified/updating-mac-apps.md) 및 [팁](~/cross-platform/macios/unified/updating-tips.md)합니다.

###  <a name="xamarinforms-appscross-platformmaciosunifiedupdating-xamarin-forms-appsmd"></a>[Xamarin.Forms 앱](~/cross-platform/macios/unified/updating-xamarin-forms-apps.md)

통합 된 API를 사용 하는 iOS 프로젝트 기존 Xamarin.Forms 솔루션을 업데이트 하려면 다음이 지침을 따릅니다. 통합 된 API 지원은 Xamarin.Forms 1.3에서 사용할 수 있으며 나중에 있으므로 [지침](~/cross-platform/macios/unified/updating-xamarin-forms-apps.md) 또한 Xamarin.Forms 앱 버전 1.3 업데이트 하는 방법을 설명 합니다. 이러한 [팁](~/cross-platform/macios/unified/updating-tips.md) 사용자 지정 렌더러 또는 종속성 서비스의 모든 네이티브 iOS 코드를 업데이트 하는 데 도움이 될 수 있습니다.

## <a name="working-with-native-types-in-cross-platform-appscross-platformmaciosnativetypesmd"></a>[플랫폼 간 앱에서 네이티브 형식 사용](~/cross-platform/macios/nativetypes.md)

이 문서에서는 Android 또는 Windows Phone Os와 같은 비 iOS 장치 코드는 공유 하는 하는 플랫폼 간 응용 프로그램에서 새 iOS API 네이티브 통합 유형 (nint, nuint, nfloat)를 사용 하 여 설명 합니다. 네이티브 형식을 사용 해야 하는 시기에 대 한 정보를 제공 하 고 새 형식의 플랫폼 간 코드를 함께 사용 해야 하는 위치 하는 경우에 몇 가지 가능한 해결 방법을 제공 합니다.

## <a name="update-bindings-to-the-unified-api"></a>통합된 API에 대 한 업데이트 바인딩

Objective C 라이브러리에 대 한 바인딩을 만든 사용자는 API (여기서 일부 형식은 이제 됩니다 64 비트)에서 변경 내용을 반영 하 여 바인딩 프로젝트를 업데이트 해야 합니다.
다음이 지침에 따라 [통합 API를 지원 하기 위해 기존 바인딩 프로젝트를 업데이트](~/cross-platform/macios/unified/update-binding.md)합니다.




## <a name="related-links"></a>관련 링크

- [IOS 앱을 업데이트합니다.](~/cross-platform/macios/unified/updating-ios-apps.md)
- [Mac 앱 업데이트](~/cross-platform/macios/unified/updating-mac-apps.md)
- [Xamarin.Forms 앱 업데이트](~/cross-platform/macios/unified/updating-xamarin-forms-apps.md)
- [바인딩 업데이트](~/cross-platform/macios/unified/update-binding.md)
- [팁 업데이트](~/cross-platform/macios/unified/updating-tips.md)
- [클래식 vs 통합 API 차이점](http://developer.xamarin.comhttps://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
