---
title: 네이티브 라이브러리 사용
ms.prod: xamarin
ms.assetid: 7AA6CEC8-C09E-BBDA-FDD6-E40559143548
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/09/2018
ms.openlocfilehash: b7d69e99327aa3d3e3e1f5e5dbc61697d1fb9b71
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/10/2020
ms.locfileid: "75489169"
---
# <a name="using-native-libraries"></a>네이티브 라이브러리 사용

Xamarin.Android는 표준 PInvoke 메커니즘을 통해 네이티브 라이브러리의 사용을 지원합니다. OS의 일부가 아닌 추가 네이티브 라이브러리를 .apk에 번들로 묶을 수도 있습니다.

Xamarin.Android 애플리케이션을 사용하여 네이티브 라이브러리를 배포하려면 프로젝트에 라이브러리 이진 파일을 추가하고 **빌드 작업**을 **AndroidNativeLibrary**로 설정합니다.

Xamarin.Android 라이브러리 프로젝트를 사용하여 네이티브 라이브러리를 배포하려면 프로젝트에 라이브러리 이진 파일을 추가하고 **빌드 작업**을 **EmbeddedNativeLibrary**로 설정합니다.

Android는 여러 ABI(애플리케이션 이진 인터페이스)를 지원하므로 Xamarin.Android는 네이티브 라이브러리가 빌드되는 ABI를 알아야 합니다.
이는 두 가지 방법으로 수행할 수 있습니다.

1. 경로 "검색"
1. 프로젝트 파일 내에서 `AndroidNativeLibrary/Abi` 요소 사용

경로 검색을 사용하면 네이티브 라이브러리의 부모 디렉터리 이름을 사용하여 라이브러리가 대상으로 하는 ABI를 지정할 수 있습니다. 따라서 프로젝트에 `lib/armeabi/libfoo.so`를 추가하면 ABI가 `armeabi`로 "검색"됩니다.

또는 프로젝트 파일을 편집하여 사용할 ABI를 명시적으로 지정할 수 있습니다.

```xml
<ItemGroup>
    <AndroidNativeLibrary Include="path/to/libfoo.so">
        <Abi>armeabi</Abi>
    </AndroidNativeLibrary>
</ItemGroup>
```

네이티브 라이브러리 사용에 대한 자세한 내용은 [네이티브 라이브러리와 상호 작용](https://www.mono-project.com/docs/advanced/pinvoke/)을 참조하세요.

## <a name="debugging-native-code-with-visual-studio"></a>Visual Studio를 사용하여 네이티브 코드 디버깅

*Visual Studio 2019* 또는 *Visual Studio 2017*을 사용하는 경우에는 위에서 설명한 대로 프로젝트 파일을 수정할 필요가 없습니다.
**동적 공유 라이브러리(Android)** 프로젝트에 프로젝트 참조를 추가하여 Xamarin.Android 솔루션 내에서 C++를 빌드하고 디버그할 수 있습니다.

프로젝트에서 네이티브 C++ 코드를 디버깅하려면 다음 단계를 수행합니다.

1. 프로젝트 **속성**을 두 번 클릭하고 **Android 옵션** 페이지를 선택합니다.
2. 아래로 스크롤하여 **디버깅 옵션**을 선택합니다.
3. **디버거** 드롭다운 메뉴에서 기본 **.NET(Xamarin)** 대신 **C++** 를 선택합니다.

Visual Studio C++ 개발자는 [SanAngeles_NativeDebug](https://docs.microsoft.com/samples/xamarin/monodroid-samples/sanangeles-ndk) 샘플을 확인하여 Xamarin을 통해 Visual Studio 2019 또는 Visual Studio 2017에서 C++ 디버깅을 시도할 수 있습니다. 자세한 내용은 [블로그 게시물](https://blog.xamarin.com/build-and-debug-c-libraries-in-xamarin-android-apps-with-visual-studio-2015/)을 참조하세요.

## <a name="related-links"></a>관련 링크

- [SanAngeles_NativeDebug(샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/sanangeles-ndk)
- [Xamarin Android 네이티브 애플리케이션 개발](https://blogs.msdn.microsoft.com/vcblog/2015/02/23/developing-xamarin-android-native-applications/)
