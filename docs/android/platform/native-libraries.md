---
title: 네이티브 라이브러리 사용
ms.prod: xamarin
ms.assetid: 7AA6CEC8-C09E-BBDA-FDD6-E40559143548
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/09/2018
ms.openlocfilehash: 1b0771a0ccc2597ebd800468b82044e4020d9d94
ms.sourcegitcommit: c4be32ef914465e808d89767c4d5ee72afe93cc6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/02/2019
ms.locfileid: "58854615"
---
# <a name="using-native-libraries"></a>네이티브 라이브러리 사용

Xamarin.Android 표준 PInvoke 메커니즘을 통해 네이티브 라이브러리의 사용을 지원 합니다. 또한 프로그램.apk에 운영 체제의 일부분이 아닌 추가 네이티브 라이브러리를 묶을 수 있습니다.

Xamarin.Android 응용 프로그램을 사용 하 여 네이티브 라이브러리를 배포 하려면 이진 라이브러리 프로젝트에 추가 하 고 설정 해당 **빌드 작업** 하 **AndroidNativeLibrary**합니다.

Xamarin.Android 라이브러리 프로젝트를 사용 하 여 네이티브 라이브러리를 배포 하려면 이진 라이브러리 프로젝트에 추가 하 고 설정 해당 **빌드 작업** 하 **EmbeddedNativeLibrary**합니다.

참고 Android는 여러 응용 프로그램 이진 인터페이스 (Abi)을 지원 하므로 Xamarin.Android 알고 있어야 한다는 ABI 네이티브 라이브러리에 대해 빌드됩니다.
이는 두 가지 방법으로 수행할 수 있습니다.

1.  경로 "검색"
1.  사용 하 여는 `AndroidNativeLibrary/Abi` 프로젝트 파일 내의 요소


경로 검색을 사용하면 네이티브 라이브러리의 부모 디렉터리 이름을 사용하여 라이브러리가 대상으로 하는 ABI를 지정할 수 있습니다. 따라서 추가 하는 경우 `lib/armeabi/libfoo.so` 프로젝트에 다음 ABI는 수로 "검색" `armeabi`합니다.

또는 명시적으로 사용 하려면 ABI를 지정 하기 위해 프로젝트 파일을 편집할 수 있습니다.

```xml
<ItemGroup>
    <AndroidNativeLibrary Include="path/to/libfoo.so">
        <Abi>armeabi</Abi>
    </AndroidNativeLibrary>
</ItemGroup>
```

네이티브 라이브러리를 사용 하는 방법에 대 한 자세한 내용은 참조 하세요. [네이티브 라이브러리를 사용 하 여 Interop](https://www.mono-project.com/docs/advanced/pinvoke/)합니다.

## <a name="debugging-native-code-with-visual-studio"></a>Visual Studio 사용 하 여 네이티브 코드 디버깅

사용 중인 경우 *Visual Studio 2019* 하거나 *Visual Studio 2017*, 위에서 설명한 대로 프로젝트 파일을 수정할 필요가 없습니다.
작성 하 고 c + +에 대 한 프로젝트 참조를 추가 하 여 Xamarin.Android 솔루션 내에서 c + +를 디버그할 수 있습니다 **동적 공유 라이브러리 (Android)** 프로젝트입니다.

프로젝트에서 네이티브 c + + 코드를 디버깅 하려면 다음이 단계를 수행 합니다.

1. 프로젝트를 두 번 클릭 **속성** 선택 합니다 **Android 옵션** 페이지입니다.
2. 아래로 스크롤하여 **디버깅 옵션**합니다.
3. 에 **디버거** 드롭다운 메뉴에서 **c + +** (기본값 대신 **.NET (Xamarin)**).

Visual Studio c + + 개발자가 볼 수는 [SanAngeles_NativeDebug](https://developer.xamarin.com/samples/monodroid/SanAngeles_NDK/) Visual Studio 2019 또는 Xamarin;를 사용 하 여 Visual Studio 2017에서 c + + 디버깅 하려는 샘플 및 참조 우리의 [블로그 게시물](https://blog.xamarin.com/build-and-debug-c-libraries-in-xamarin-android-apps-with-visual-studio-2015/) 자세한 합니다.



## <a name="related-links"></a>관련 링크

- [SanAngeles_NativeDebug (샘플)](https://developer.xamarin.com/samples/monodroid/SanAngeles_NDK/)
- [Xamarin Android 네이티브 응용 프로그램 개발](https://blogs.msdn.microsoft.com/vcblog/2015/02/23/developing-xamarin-android-native-applications/)
