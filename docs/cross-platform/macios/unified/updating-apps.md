---
title: Unified API에 기존 앱 업데이트
description: 이 문서는 Xamarin 응용 프로그램을 Unified API 업데이트 하는 방법을 설명 하는 다양 한 가이드에 연결 됩니다. Xamarin.ios 앱, Xamarin.ios 앱에 대해 설명 합니다. Xamarin Forms 앱, 플랫폼 간 앱 및 바인딩 프로젝트의 네이티브 형식입니다.
ms.prod: xamarin
ms.assetid: 8A654C95-5DCA-4BB5-A582-F96C2BECC81C
author: conceptdev
ms.author: crdun
ms.date: 03/29/2017
ms.openlocfilehash: c7742622bae16e874411fad1374c3ee522dba183
ms.sourcegitcommit: 6b833f44d5fd8dc7ab7f8546e8b7d383e5a989db
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71106056"
---
# <a name="updating-existing-apps-to-the-unified-api"></a>Unified API에 기존 앱 업데이트

> [!IMPORTANT]
> Unified API 앞에 있는 Xamarin Classic API은 더 이상 사용 되지 않습니다.
>
> - Classic API (monotouch.dialog)를 지원 하기 위한 최신 버전의 Xamarin.ios는 Xamarin.ios 9.10입니다.
> - Xamarin.ios는 여전히 Classic API을 지원 하지만 더 이상 업데이트 되지 않습니다. 개발자는 더 이상 사용 되지 않으므로 응용 프로그램을 Unified API로 이동 해야 합니다.

## <a name="how-to-update-your-apps"></a>앱을 업데이트 하는 방법

앱을 업데이트 하는 세 가지 단계가 있습니다.

1. 기존 코드, 특히 사용 되지 않는 Api와 관련 된 모든 컴파일러 경고를 수정 합니다.

2. 기본 제공 되는 마이그레이션 도구를 사용 하 여 프로젝트 파일 및 네임 스페이스를 업데이트 Mac용 Visual Studio 합니다.

3. 새 [64 형식](~/cross-platform/macios/nativetypes.md) 및 변경 된 [다른 api](~/cross-platform/macios/unified/overview.md#deprecated-typos) 와 관련 된 나머지 컴파일러 오류를 수정 합니다. 필요할 수 있는 수동 업데이트에 대 한 자세한 내용은 [다음 팁](~/cross-platform/macios/unified/updating-tips.md) 을 확인 하세요.

앱을 Unified API 및 64 비트 지원으로 업데이트 하는 데 도움이 되는 각 제품에 대 한 특정 가이드가 있습니다.

### <a name="xamarinios-appscross-platformmaciosunifiedupdating-ios-appsmd"></a>[Xamarin.ios 앱](~/cross-platform/macios/unified/updating-ios-apps.md)

Mac용 Visual Studio 기본 제공 되는 자동화 된 마이그레이션 도구를 사용 하 여 기존 Xamarin.ios 앱을 Unified API으로 업데이트할 수 있습니다. [이러한 지침](~/cross-platform/macios/unified/updating-ios-apps.md) 및 [팁](~/cross-platform/macios/unified/updating-tips.md)에 설명 된 대로 몇 가지 추가 수정이 필요할 수 있습니다.

### <a name="xamarinmac-appscross-platformmaciosunifiedupdating-mac-appsmd"></a>[Xamarin.Mac apps](~/cross-platform/macios/unified/updating-mac-apps.md)

Mac용 Visual Studio 기본 제공 되는 자동화 된 마이그레이션 도구를 사용 하 여 기존 Xamarin.ios 앱을 Unified API으로 업데이트할 수 있습니다. [이러한 지침](~/cross-platform/macios/unified/updating-mac-apps.md) 및 [팁](~/cross-platform/macios/unified/updating-tips.md)에 설명 된 대로 몇 가지 추가 수정이 필요할 수 있습니다.

### <a name="xamarinforms-appscross-platformmaciosunifiedupdating-xamarin-forms-appsmd"></a>[Xamarin.Forms 앱](~/cross-platform/macios/unified/updating-xamarin-forms-apps.md)

Unified API 사용 하려면 다음 지침에 따라 iOS 프로젝트를 사용 하 여 기존 Xamarin.ios 솔루션을 업데이트 합니다. Unified API 지원은 Xamarin.ios 1.3 이상 에서만 사용할 수 있으므로, Xamarin. Forms 앱을 버전 1.3로 업데이트 하는 방법에 대해서도 설명 [합니다](~/cross-platform/macios/unified/updating-xamarin-forms-apps.md) . 이러한 [팁](~/cross-platform/macios/unified/updating-tips.md) 은 사용자 지정 렌더러 또는 종속성 서비스에서 네이티브 iOS 코드를 업데이트 하는 데 도움이 될 수 있습니다.

## <a name="working-with-native-types-in-cross-platform-appscross-platformmaciosnativetypesmd"></a>[플랫폼 간 앱에서의 네이티브 형식 작업](~/cross-platform/macios/nativetypes.md)

이 문서에서는 Android 또는 Windows Phone Os와 같은 비 iOS 장치와 코드를 공유 하는 플랫폼 간 응용 프로그램에서 새로운 iOS Unified API 네이티브 형식 (nint, nuint, nint)을 사용 하는 방법을 설명 합니다. 네이티브 형식을 사용 해야 하는 경우에 대 한 통찰력을 제공 하 고 플랫폼 간 코드에서 새 형식을 사용 해야 하는 경우에 대 한 몇 가지 가능한 해결 방법을 제공 합니다.

## <a name="update-bindings-to-the-unified-api"></a>Unified API에 대 한 바인딩을 업데이트 합니다.

목적-C 라이브러리에 대 한 바인딩을 만든 고객은 기본 API (이제는 64 비트)의 변경 내용을 반영 하도록 바인딩 프로젝트를 업데이트 해야 합니다.
[Unified API를 지원 하도록 기존 바인딩 프로젝트를 업데이트](~/cross-platform/macios/unified/update-binding.md)하려면 다음 지침을 따르세요.

## <a name="related-links"></a>관련 링크

- [IOS 앱 업데이트](~/cross-platform/macios/unified/updating-ios-apps.md)
- [Mac 앱 업데이트](~/cross-platform/macios/unified/updating-mac-apps.md)
- [Xamarin Forms 앱 업데이트](~/cross-platform/macios/unified/updating-xamarin-forms-apps.md)
- [바인딩 업데이트](~/cross-platform/macios/unified/update-binding.md)
- [팁 업데이트](~/cross-platform/macios/unified/updating-tips.md)
- [클래식 vs Unified API 차이점](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/api_changes/classic-vs-unified-8.6.0/index.md)
