---
title: Xamarin 용 NuGet 패키지를 수동으로 만들기
description: 이 문서는 Xamarin 플랫폼을 대상으로 하는 NuGet 패키지를 빌드하는 데 유용한 팁을 포함 합니다. 플랫폼 종속성을 통해 PCL Nuget, NuGet 패키지 Xamarin 프로필을 설명 하 고 다양 한 오픈 소스 샘플에 연결 합니다.
ms.prod: xamarin
ms.assetid: a5964686-5fc6-4280-b087-7ba27cc1c8bf
author: asb3993
ms.author: amburns
ms.date: 03/22/2017
ms.openlocfilehash: 4f4ca327479a7f4eb4a7dc7feafdd71291c1b7fe
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61186145"
---
# <a name="manually-creating-nuget-packages-for-xamarin"></a>Xamarin 용 NuGet 패키지를 수동으로 만들기

_이 페이지는 Xamarin 플랫폼을 대상으로 하는 NuGet 패키지를 작성 하는 데는 몇 가지 팁을 포함 합니다._

> [!NOTE]
> Xamarin Studio 6.2 (및 Mac 용 Visual Studio)를 포함 하는 기능 _자동으로_ PCL,.NET Standard 또는 공유 프로젝트에서 NuGet 패키지를 생성 합니다. 참조 된 [코드 공유에 대 한 다중 플랫폼 라이브러리](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/index.md) 자세한 가이드입니다.

## <a name="nuget-package-xamarin-profiles"></a>NuGet 패키지 Xamarin 프로필

NuGet 웹 사이트 [지 원하는 여러.NET Framework 버전 및 프로필](https://docs.nuget.org/create/enforced-package-conventions) Xamarin에서 사용 하는 대상 프레임 워크 이름을 포함 하지 않는 다른 Microsoft 프레임 워크 및 프로필을 지 원하는 방법에 설명 합니다.

기본 Xamarin 대상 프레임 워크에서 사용 하 여 현재 다음과 같습니다.

* **MonoAndroid** - Xamarin.Android
* **Xamarin.iOS** -Xamarin.iOS [Unified API](~/cross-platform/macios/unified/index.md) (64 비트 지원)
* **Xamarin.Mac** -Xamarin.iOS 및 Xamarin.Android API 화면에 해당는 Xamarin.Mac의 모바일 프로필입니다.

오래 된 iOS에 대 한 대상 이기도 [클래식 API](~/cross-platform/macios/unified/index.md):

* **MonoTouch** -iOS 클래식 API

A **.nuspec** 이러한 모든 요소를 대상으로 하는 파일 같이 표시 됩니다.

```xml
<files>
    <file src="Mac\bin\Release\*.dll" target="lib\Xamarin.Mac20" />
    <file src="iOS\bin\Release\*.dll" target="lib\Xamarin.iOS10" />
    <file src="Android\bin\Release\*.dll" target="lib\MonoAndroid10" />
    <file src="iOSClassic\bin\Release\*.dll" target="lib\MonoTouch10" />
</files>
```

위의 모든 이식 가능한 클래스 라이브러리를 무시합니다.

대부분 **.nuspec** 파일은 대상 프레임 워크의 버전 번호를 지정 하지만 어셈블리는 대상 프레임 워크의 모든 버전에서 작동 하는 경우 선택 사항입니다. 대상 된 만약 **lib\MonoAndroid** 모든 버전의 Xamarin.Android 사용 하 여 작동를 의미 합니다.

소수점이 없는 숫자 집합을 사용 하 여 버전을 지정할 수 있습니다 또는 소수점이 하를 사용 하 여 지정할 수 있습니다. 소수점 NuGet은 바로 각 숫자를 사용 하지 않고 삽입 하 여 버전으로 설정 된 '.' 각 숫자 사이.

에 위의 "MonoAndroid10" "Android 1.0"을 의미합니다. 이 프로젝트의 의미 [대상 프레임 워크](~/android/app-fundamentals/android-api-levels.md) MonoAndroid 버전 1.0 이상 이어야 합니다. 버전에 지정 된 된 `<TargetFrameworkVersion>` 프로젝트 파일의 요소입니다.

명확 합니다.

- **MonoAndroid403** 일치 Android 4.0.3 이상 (예: API 수준 15)
- **Xamarin.iOS10** Xamarin.iOS 1.0 이상 일치
- **Xamarin.iOS1.0** Xamarin.iOS 1.0 이상 일치

## <a name="pcl-nugets-with-platform-dependencies"></a>플랫폼 종속성을 사용 하 여 PCL Nuget

확실히 플랫폼 특정 코드에 액세스할 수 없습니다 및 PCL 프로필 Api에 액세스할 수 있는.NET framework에서 제한 됩니다. 이러한 타사 링크는 Xamarin 및 다른 플랫폼에 대 한 호환성을 위해 PCL 및 네이티브 Api를 사용 하는 NuGet 패키지를 만드는 두 가지를 설명 합니다.

- [이식 가능한 클래스 라이브러리가 작동 하는 방법](http://blogs.msdn.com/b/dsplaisted/archive/2012/08/27/how-to-make-portable-class-libraries-work-for-you.aspx)
- [Bait 및 스위치 PCL 트릭](http://log.paulbetts.org/the-bait-and-switch-pcl-trick/)
- [Xamarin.iOS와 함께 작동 하는 NuGet PCL을 만들기](http://www.jimbobbennett.io/creating-a-nuget-pcl-that-works-with-xamarin-ios/)

이 외부 [는 NuGet 대상 이름과 함께 PCL 프로필 목록을](http://embed.plnkr.co/03ck2dCtnJogBKHJ9EjY) 도 유용한 참고 자료입니다.

## <a name="examples"></a>예제

참조할 수 있는 몇 가지 오픈 소스 예제:

- [**ModernHttpClient** ](https://www.nuget.org/packages/modernhttpclient/) System.Net.Http를 사용 하 여 앱을 작성 하지만이 라이브러리 삭제 – 현저 하 게 더 빠르게 이동 (뷰 [원본](https://github.com/paulcbetts/ModernHttpClient)).
- [**스 플랫** ](https://www.nuget.org/packages/Splat/) – 해야 하는 작업을 하기 위해 플랫폼 간 라이브러리 (뷰 [원본](https://github.com/paulcbetts/Splat)).
- [**NGraphics** ](https://www.nuget.org/packages/NGraphics/) -.NET에서 벡터 그래픽을 렌더링 하기 위한 플랫폼 간 라이브러리 (뷰 [원본](https://github.com/praeclarum/NGraphics/blob/master/NGraphics.nuspec)).

## <a name="related-links"></a>관련 링크

- [Nugetizer 3000 Nuget 만들기 자동화](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/index.md)
- [64 비트 iOS에 대 한 Nuget을 업데이트 하는 중입니다.](https://blog.xamarin.com/how-to-update-nuget-packages-for-64-bit/)
- [NuGet 프로젝트 포함](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)
