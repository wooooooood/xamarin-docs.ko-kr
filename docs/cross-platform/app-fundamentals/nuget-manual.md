---
title: "Xamarin에 대 한 NuGet 패키지를 수동으로 만들기"
description: "이 페이지에는 몇 가지 팁을을 Xamarin 플랫폼을 대상으로 하는 NuGet 패키지를 작성할 수 있습니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: a5964686-5fc6-4280-b087-7ba27cc1c8bf
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/22/2017
ms.openlocfilehash: 76f35819b00302f4a586643798afbd27416d3997
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="manually-creating-nuget-packages-for-xamarin"></a>Xamarin에 대 한 NuGet 패키지를 수동으로 만들기

_이 페이지에는 몇 가지 팁을을 Xamarin 플랫폼을 대상으로 하는 NuGet 패키지를 작성할 수 있습니다._

> [!NOTE]
> Xamarin Studio 6.2 (및 Mac 용 Visual Studio)의 기능이 포함 되어 _자동으로_ PCL,.NET 표준 또는 공유 프로젝트에서 NuGet 패키지를 생성 합니다.
> 참조는 [코드 공유에 대 한 다중 플랫폼 라이브러리](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/index.md) 자세한 내용은 가이드입니다.

## <a name="nuget-package-xamarin-profiles"></a>NuGet 패키지 Xamarin 프로필


NuGet 웹 사이트의 [지 원하는 여러.NET Framework 버전 및 프로필](https://docs.nuget.org/create/enforced-package-conventions) 를 다른 Microsoft 프레임 워크 및 프로필을 지 원하는 방법에 설명 하지만 Xamarin에서 사용 하는 대상 프레임 워크 이름을 포함 하지 않습니다.

사용 하 여 기본 Xamarin 대상 프레임 워크 오늘 됩니다.

* **MonoAndroid** - Xamarin.Android
* **Xamarin.iOS** -Xamarin.iOS [통합 API](~/cross-platform/macios/unified/index.md) (64 비트 지원)
* **Xamarin.Mac** -Xamarin.Mac의 모바일 프로필 Xamarin.iOS 및 Xamarin.Android API 화면에 해당 합니다.

이전 버전 iOS에 대 한 대상 이기도 [클래식 API](~/cross-platform/macios/unified/index.md):

* **MonoTouch** -iOS 클래식 API

A **.nuspec** 파일 이러한 모든 요소를 대상으로 하는 같습니다.

```xml
<files>
    <file src="Mac\bin\Release\*.dll" target="lib\Xamarin.Mac20" />
    <file src="iOS\bin\Release\*.dll" target="lib\Xamarin.iOS10" />
    <file src="Android\bin\Release\*.dll" target="lib\MonoAndroid10" />
    <file src="iOSClassic\bin\Release\*.dll" target="lib\MonoTouch10" />
</files>
```

위의 모든 이식 가능한 클래스 라이브러리를 무시합니다.

대부분 **.nuspec** 파일의 대상 프레임 워크의 버전 번호를 지정 하지만는 어셈블리의 대상 프레임 워크에 있는 모든 버전 사용 하는 경우 선택 사항입니다. 따라서 대상 된 경우 **lib\MonoAndroid** Xamarin.Android의 모든 버전에서 작동 하는 것을 의미 하므로이 있습니다.

소수점이 없는 숫자 집합으로 버전을 지정할 수 있습니다 또는 소수점이 하를 사용 하 여 지정할 수 있습니다. 소수점 없이 NuGet는 방금 각 번호를 사용 하 고 설정 버전으로 삽입 하 여는 '.' 각 자릿수 사이입니다.

에 위의 "MonoAndroid10" "Android 1.0"을 의미합니다. 프로젝트의 것일 뿐 [대상 프레임 워크](~/android/app-fundamentals/android-api-levels.md) MonoAndroid 버전 1.0 이상이 이어야 합니다. 에 지정 된 버전의 `<TargetFrameworkVersion>` 프로젝트 파일의 요소입니다.

명확히:

- **MonoAndroid403** 일치 하는 Android 4.0.3 이상 (ie API 수준 15)
- **Xamarin.iOS10** Xamarin.iOS 1.0 이상 일치
- **Xamarin.iOS1.0** 도 Xamarin.iOS 1.0 이상 일치


## <a name="pcl-nugets-with-platform-dependencies"></a>플랫폼 종속성이 있는 PCL NuGets

PCL 프로필은 어떤.NET framework Api에 액세스 하려면 제한적 및 확실히 플랫폼 특정 코드에 액세스할 수 없습니다. 이러한 타사 링크에서 호환성을 제공 하기 Xamarin 및 다른 플랫폼에 대 한 PCL 및 네이티브 Api를 사용 하는 NuGet 패키지를 만들기 위한 두 가지 방법을 설명 합니다.

- [이식 가능한 클래스 라이브러리 작업을 수행할 수 있도록 하는 방법](http://blogs.msdn.com/b/dsplaisted/archive/2012/08/27/how-to-make-portable-class-libraries-work-for-you.aspx)
- [밥 and 스위치 PCL 트릭](http://log.paulbetts.org/the-bait-and-switch-pcl-trick/)
- [Xamarin.iOS 작동 하는 NuGet PCL 만들기](http://www.jimbobbennett.io/creating-a-nuget-pcl-that-works-with-xamarin-ios/)

이 외부 [NuGet 대상 이름을 PCL 프로필 목록](http://embed.plnkr.co/03ck2dCtnJogBKHJ9EjY) 유용한 참조 이기도 합니다.

## <a name="examples"></a>예제

참조할 수 있는 몇 가지 오픈 소스 예:

- [**ModernHttpClient** ](https://www.nuget.org/packages/modernhttpclient/) – System.Net.Http를 사용 하 여 앱을 작성 하지만에이 라이브러리를 삭제 하 고 더 빠르게 현저 하 게 이동 합니다 (보기 [소스](https://github.com/paulcbetts/ModernHttpClient)).
- [**스 플랫** ](https://www.nuget.org/packages/Splat/) – 되어야 하는 작업을 수행 하기 플랫폼 간 라이브러리 (보기 [소스](https://github.com/paulcbetts/Splat)).
- [**NGraphics** ](https://www.nuget.org/packages/NGraphics/) -.NET에 벡터 그래픽을 렌더링 하기 위한 플랫폼 간 라이브러리입니다 (보기 [소스](https://github.com/praeclarum/NGraphics/blob/master/NGraphics.nuspec)).


## <a name="related-links"></a>관련 링크

- [Nugetizer 3000 자동 Nuget 만들기](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/index.md)
- [NuGets iOS 64 비트에 대 한 업데이트](http://blog.xamarin.com/how-to-update-nuget-packages-for-64-bit/)
- [프로젝트에서 NuGet을 포함 하 여](/visualstudio/mac/nuget-walkthrough/index.md)
