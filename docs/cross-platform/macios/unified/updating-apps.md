---
title: Unified API로 기존 앱을 업데이트 하는 중입니다.
description: 이 문서는 Xamarin 응용 프로그램 통합 API로 업데이트 하는 방법을 설명 하는 다양 한 설명서를 링크 합니다. Xamarin.iOS 앱, Xamarin.Mac 앱에 설명 합니다. Xamarin.Forms 앱을 플랫폼 간 앱 및 바인딩 프로젝트의 네이티브 형식입니다.
ms.prod: xamarin
ms.assetid: 8A654C95-5DCA-4BB5-A582-F96C2BECC81C
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 2d09be7b85980e5c5a8eb209dc1b4ff3136c34b3
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61195602"
---
# <a name="updating-existing-apps-to-the-unified-api"></a>Unified API로 기존 앱을 업데이트 하는 중입니다.

> [!IMPORTANT]
> 앞에 통합 API를 사용 하 여 Xamarin 클래식 API는 더 이상 사용 되지 않습니다. 
> - 클래식 API (monotouch.dll)를 지원 하기 위해 Xamarin.iOS의 마지막 버전 9.10 Xamarin.iOS 했습니다.
> - Xamarin.Mac을 클래식 API를 계속 지원 되지만 더 이상으로 업데이트 됩니다. 되지 하므로 개발자가 응용 프로그램 통합 API로 이동 해야 합니다.

## <a name="how-to-update-your-apps"></a>앱을 업데이트 하는 방법

Xamarin University에 자유롭게 사용할 수 있는 비디오를 주지 **iOS 통합 API로**합니다. 방문 [Xamarin University 번개 강의](http://university.xamarin.com/lightninglectures) 를 시청 하세요!

[ ![](updating-apps-images/xamu-video-sml.png "Xamarin University")](http://university.xamarin.com/lightninglectures)

앱을 업데이트 하는 다음과 같은 세 가지 단계가 있습니다.

1. 컴파일러 경고를 모두 수정 기존 코드에서 사용 되지 않는 Api와 관련 된 특히 합니다.

2. Mac 용 Visual Studio에 기본 제공 마이그레이션 도구를 사용 하 여 프로젝트 파일 및 네임 스페이스를 업데이트 합니다.

3. 나머지 컴파일러 오류와 관련 된 새 수정 [64 형식](~/cross-platform/macios/nativetypes.md) 하 고 [다른 Api](~/cross-platform/macios/unified/overview.md#deprecated-typos) 변경 된 합니다. 체크 아웃 [이러한 팁](~/cross-platform/macios/unified/updating-tips.md) 필요할 수 있는 수동 업데이트에 대 한 자세한 내용은 합니다.

64 비트 지원 통합 API를 응용 프로그램을 업데이트할 수 있도록 각 제품에 사용할 수 있는 특정 가이드는:

### <a name="xamarinios-appscross-platformmaciosunifiedupdating-ios-appsmd"></a>[Xamarin.iOS 앱](~/cross-platform/macios/unified/updating-ios-apps.md)

Mac 용 Visual studio 빌드된 자동화 된 마이그레이션 도구를 사용 하 여 통합 API로 기존 Xamarin.iOS 앱을 업데이트할 수 있습니다. 몇 가지 추가 수정 후 필요할 수 있습니다에 설명 된 대로 [이러한 지침](~/cross-platform/macios/unified/updating-ios-apps.md) 하 고 [팁](~/cross-platform/macios/unified/updating-tips.md)합니다.

###  <a name="xamarinmac-appscross-platformmaciosunifiedupdating-mac-appsmd"></a>[Xamarin.Mac apps](~/cross-platform/macios/unified/updating-mac-apps.md)

Mac 용 Visual studio 빌드된 자동화 된 마이그레이션 도구를 사용 하 여 통합 API로 기존 Xamarin.Mac 앱을 업데이트할 수 있습니다. 몇 가지 추가 수정 후 필요할 수 있습니다에 설명 된 대로 [이러한 지침](~/cross-platform/macios/unified/updating-mac-apps.md) 하 고 [팁](~/cross-platform/macios/unified/updating-tips.md)합니다.

###  <a name="xamarinforms-appscross-platformmaciosunifiedupdating-xamarin-forms-appsmd"></a>[Xamarin.Forms 앱](~/cross-platform/macios/unified/updating-xamarin-forms-apps.md)

Unified API를 사용 하는 iOS 프로젝트를 사용 하 여 기존 Xamarin.Forms 솔루션을 업데이트 하려면 다음이 지침을 따릅니다. 통합 된 API 지원은 에서만 Xamarin.Forms 1.3에서 사용할 수 있으며 나중에 있으므로 [지침](~/cross-platform/macios/unified/updating-xamarin-forms-apps.md) 도 버전 1.3 Xamarin.Forms 앱을 업데이트 하는 방법에 설명 합니다. 이러한 [팁](~/cross-platform/macios/unified/updating-tips.md) 종속성 서비스나 사용자 지정 렌더러 모든 네이티브 iOS 코드를 업데이트 하는 데 도움이 될 수 있습니다.

## <a name="working-with-native-types-in-cross-platform-appscross-platformmaciosnativetypesmd"></a>[플랫폼 간 앱에서의 네이티브 형식 작업](~/cross-platform/macios/nativetypes.md)

이 문서에서는 Android 또는 Windows Phone Os와 같은 비 iOS 장치를 사용 하 여 코드 공유 되는 위치 하는 플랫폼 간 응용 프로그램에서 새 iOS API 네이티브 통합 형식 (nint, nuint nfloat)를 사용 하 여 설명 합니다. 네이티브 형식을 사용할 경우에 대 한 정보를 제공 하 고 있는 플랫폼 간 코드를 사용 하 여 새 형식을 사용 해야 하는 경우에는 몇 가지 가능한 솔루션을 제공 합니다.

## <a name="update-bindings-to-the-unified-api"></a>바인딩을 Unified API로 업데이트

Objective-c 라이브러리 바인딩 만든 고객 (일부 형식은 이제 될 위치 64-bit) 기본 API의 변경 내용을 반영 하도록 바인딩 프로젝트를 업데이트 해야 합니다.
다음이 지침에 따라 [Unified API를 지원 하기 위해 기존 바인딩 프로젝트 업데이트](~/cross-platform/macios/unified/update-binding.md)합니다.

## <a name="related-links"></a>관련 링크

- [IOS 앱 업데이트](~/cross-platform/macios/unified/updating-ios-apps.md)
- [Mac 앱 업데이트](~/cross-platform/macios/unified/updating-mac-apps.md)
- [Xamarin.Forms 앱 업데이트](~/cross-platform/macios/unified/updating-xamarin-forms-apps.md)
- [바인딩 업데이트](~/cross-platform/macios/unified/update-binding.md)
- [팁 업데이트](~/cross-platform/macios/unified/updating-tips.md)
- [클래식 vs 통합 API 차이점](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
