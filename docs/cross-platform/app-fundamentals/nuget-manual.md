---
title: Xamarin에 대 한 NuGet 패키지 수동 만들기
description: 이 문서에는 Xamarin 플랫폼을 대상으로 하는 NuGet 패키지를 빌드하는 데 유용한 팁이 포함 되어 있습니다. NuGet 패키지 Xamarin 프로필, 플랫폼 종속성이 있는 PCL Nuget 및 다양 한 오픈 소스 샘플에 대 한 링크를 설명 합니다.
ms.prod: xamarin
ms.assetid: a5964686-5fc6-4280-b087-7ba27cc1c8bf
author: asb3993
ms.author: amburns
ms.date: 03/22/2017
ms.openlocfilehash: cf694b54c8d2cdb33fd480d89d32b439f036ddc5
ms.sourcegitcommit: 1dd7d09b60fcb1bf15ba54831ed3dd46aa5240cb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70119442"
---
# <a name="manually-creating-nuget-packages-for-xamarin"></a>Xamarin에 대 한 NuGet 패키지 수동 만들기

_이 페이지에는 Xamarin 플랫폼을 대상으로 하는 NuGet 패키지를 빌드하기 위한 몇 가지 팁이 포함 되어 있습니다._

> [!NOTE]
> Xamarin Studio 6.2 및 Mac용 Visual Studio에는 PCL, .NET Standard 또는 공유 프로젝트에서 NuGet 패키지를 _자동으로_ 생성 하는 기능이 포함 되어 있습니다. 자세한 내용은 [코드 공유에 대 한 다중 플랫폼 라이브러리](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/index.md) 가이드를 참조 하세요.

## <a name="nuget-package-xamarin-profiles"></a>NuGet 패키지 Xamarin 프로필

NuGet 웹 사이트에서 [여러 .NET Framework 버전 및 프로필을 지 원하는](https://docs.nuget.org/create/enforced-package-conventions) 경우 여러 Microsoft 프레임 워크 및 프로필을 지 원하는 방법에 대해 설명 하지만 Xamarin에서 사용 되는 대상 프레임 워크 이름은 포함 되지 않습니다.

현재 사용 중인 기본 Xamarin 대상 프레임 워크는 다음과 같습니다.

- **MonoAndroid** - Xamarin.Android
- **Xamarin.ios** -xamarin.ios [Unified API](~/cross-platform/macios/unified/index.md) (64 비트 지원)
- Xamarin.ios 및 xamarin.ios의 모바일 프로필. Xamarin.ios 및 xamarin.ios API 화면에 해당 합니다.

이전 iOS [Classic API](~/cross-platform/macios/unified/index.md)에 대 한 대상도 있습니다.

- **Monotouch.dialog** Classic API

이러한 모든 항목을 대상으로 하는 **nuspec** 파일은 다음과 같습니다.

```xml
<files>
    <file src="Mac\bin\Release\*.dll" target="lib\Xamarin.Mac20" />
    <file src="iOS\bin\Release\*.dll" target="lib\Xamarin.iOS10" />
    <file src="Android\bin\Release\*.dll" target="lib\MonoAndroid10" />
    <file src="iOSClassic\bin\Release\*.dll" target="lib\MonoTouch10" />
</files>
```

위의은 이식 가능한 클래스 라이브러리를 무시 합니다.

대부분의 **nuspec** 파일은 대상 프레임 워크의 버전 번호를 지정 하지만 어셈블리가 해당 대상 프레임 워크의 모든 버전에서 작동 하는 경우 선택 사항입니다. 따라서 대상이 **lib\MonoAndroid** 인 경우 모든 버전의 xamarin.ios에서 작동 하는 것을 의미 합니다.

소수점이 하를 사용 하지 않고 숫자 집합을 사용 하 여 버전을 지정 하거나 소수점을 사용 하 여 지정할 수 있습니다. 소수점이 없는 경우 NuGet은 각 숫자 사이에 '. '를 삽입 하 여 각 숫자를 가져와 버전으로 변환 합니다.

위의 "MonoAndroid10"은 "Android 1.0"을 의미 합니다. 즉, 프로젝트의 [대상 프레임 워크](~/android/app-fundamentals/android-api-levels.md) 를 MonoAndroid 버전 1.0 이상으로 만들어야 합니다. 버전은 프로젝트 파일의 `<TargetFrameworkVersion>` 요소에 지정 됩니다.

명확 하 게 설명:

- **MonoAndroid403** 는 Android 4.0.3 이상 (ie API 수준 15)과 일치 합니다.
- **IOS10** 는 xamarin.ios 1.0 이상 일치 합니다.
- **Xamarin.ios 1.0** 도 Xamarin. ios 1.0 이상에 일치 합니다.

## <a name="pcl-nugets-with-platform-dependencies"></a>플랫폼 종속성이 있는 PCL Nuget

PCL 프로필은 액세스할 수 있는 .NET framework Api에서 제한적 이며 플랫폼별 코드에 액세스할 수 없습니다. 이러한 타사 링크는 PCL 및 네이티브 Api를 사용 하 여 Xamarin 및 기타 플랫폼에 대 한 호환성을 제공 하는 NuGet 패키지를 만들기 위한 다양 한 접근 방식을 설명 합니다.

- [이식 가능한 클래스 라이브러리를 사용 하도록 설정 하는 방법](http://blogs.msdn.com/b/dsplaisted/archive/2012/08/27/how-to-make-portable-class-libraries-work-for-you.aspx)
- [Bait 및 스위치 PCL 트릭](http://log.paulbetts.org/the-bait-and-switch-pcl-trick/)
- [Xamarin.ios와 작동 하는 NuGet PCL 만들기](http://www.jimbobbennett.io/creating-a-nuget-pcl-that-works-with-xamarin-ios/)

[NuGet 대상 이름이 있는 PCL 프로필의](http://embed.plnkr.co/03ck2dCtnJogBKHJ9EjY) 이 외부 목록도 유용한 참조입니다.

## <a name="examples"></a>예

참조할 수 있는 몇 가지 오픈 소스 예제는 다음과 같습니다.

- [**ModernHttpClient**](https://www.nuget.org/packages/modernhttpclient/) – 시스템 .Net. Http를 사용 하 여 앱을 작성 하 되,이 라이브러리를에 저장 하 고 매우 빠르게 작동 합니다 ( [원본](https://github.com/paulcbetts/ModernHttpClient)보기).
- [**스 플랫**](https://www.nuget.org/packages/Splat/) – 플랫폼 간 작업 ( [소스](https://github.com/paulcbetts/Splat)보기)을 수행 하는 라이브러리입니다.
- [**Ngraphics**](https://www.nuget.org/packages/NGraphics/) -.net에서 벡터 그래픽을 렌더링 하기 위한 플랫폼 간 라이브러리입니다 ( [원본](https://github.com/praeclarum/NGraphics/blob/master/NGraphics.nuspec)보기).

## <a name="related-links"></a>관련 링크

- [Nugetizer-3000 자동화 된 Nuget 만들기](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/index.md)       
- [프로젝트에 NuGet 포함](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)
