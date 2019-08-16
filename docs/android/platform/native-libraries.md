---
title: 네이티브 라이브러리 사용
ms.prod: xamarin
ms.assetid: 7AA6CEC8-C09E-BBDA-FDD6-E40559143548
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/09/2018
ms.openlocfilehash: fa0a3a75a4cc2cfd04b607f17206faa822af0474
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69523635"
---
# <a name="using-native-libraries"></a>네이티브 라이브러리 사용

Xamarin.ios는 표준 PInvoke 메커니즘을 통해 네이티브 라이브러리를 사용할 수 있도록 지원 합니다. 운영 체제의 일부가 아닌 추가 네이티브 라이브러리를 .apk에 묶을 수도 있습니다.

Xamarin Android 응용 프로그램을 사용 하 여 네이티브 라이브러리를 배포 하려면 프로젝트에 라이브러리 이진 파일을 추가 하 고 **빌드 작업** 을 **AndroidNativeLibrary**로 설정 합니다.

Xamarin.ios 라이브러리 프로젝트를 사용 하 여 네이티브 라이브러리를 배포 하려면 라이브러리 이진 파일을 프로젝트에 추가 하 고 **빌드 작업** 을 **EmbeddedNativeLibrary**로 설정 합니다.

Android는 여러 응용 프로그램 이진 인터페이스 (ABIs)를 지원 하기 때문에 Xamarin Android는 네이티브 라이브러리가 빌드되는 ABI를 알아야 합니다.
이는 두 가지 방법으로 수행할 수 있습니다.

1. 경로 "스니핑"
1. 프로젝트 파일 내 `AndroidNativeLibrary/Abi` 에서 요소 사용


경로 검색을 사용하면 네이티브 라이브러리의 부모 디렉터리 이름을 사용하여 라이브러리가 대상으로 하는 ABI를 지정할 수 있습니다. 따라서 프로젝트에를 추가 `lib/armeabi/libfoo.so` 하는 경우 ABI는로 `armeabi`"스니핑" 됩니다.

또는 프로젝트 파일을 편집 하 여 사용할 ABI를 명시적으로 지정할 수 있습니다.

```xml
<ItemGroup>
    <AndroidNativeLibrary Include="path/to/libfoo.so">
        <Abi>armeabi</Abi>
    </AndroidNativeLibrary>
</ItemGroup>
```

네이티브 라이브러리를 사용 하는 방법에 대 한 자세한 내용은 [네이티브 라이브러리와의 상호 운용성](https://www.mono-project.com/docs/advanced/pinvoke/)을 참조 하세요.

## <a name="debugging-native-code-with-visual-studio"></a>Visual Studio를 사용 하 여 네이티브 코드 디버깅

*Visual studio 2019* 또는 *visual studio 2017*를 사용 하는 경우 위에서 설명한 대로 프로젝트 파일을 수정할 필요가 없습니다.
C++ C++ **동적 공유 라이브러리 (Android)** 프로젝트에 프로젝트 참조를 추가 하 여 Xamarin android 솔루션 내에서 빌드 및 디버그할 수 있습니다.

프로젝트에서 네이티브 C++ 코드를 디버깅 하려면 다음 단계를 수행 합니다.

1. 프로젝트 **속성** 을 두 번 클릭 하 고 **Android 옵션** 페이지를 선택 합니다.
2. **디버깅 옵션**으로 스크롤합니다.
3. **디버거** 드롭다운 메뉴에서 기본 **C++** **.net (Xamarin)** 대신를 선택 합니다.

Visual Studio C++ 개발자가 visual studio 2019 또는 visual studio 2017 C++ 에서 Xamarin을 사용 하 여 디버깅을 시도 하는 [SanAngeles_NativeDebug](https://docs.microsoft.com/samples/xamarin/monodroid-samples/sanangeles-ndk) 샘플을 볼 수 있습니다. 자세한 내용은 [블로그 게시물](https://blog.xamarin.com/build-and-debug-c-libraries-in-xamarin-android-apps-with-visual-studio-2015/) 을 참조 하세요.



## <a name="related-links"></a>관련 링크

- [SanAngeles_NativeDebug (샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/sanangeles-ndk)
- [Xamarin Android 네이티브 응용 프로그램 개발](https://blogs.msdn.microsoft.com/vcblog/2015/02/23/developing-xamarin-android-native-applications/)
