---
title: 네이티브 라이브러리를 사용 하 여
ms.prod: xamarin
ms.assetid: 7AA6CEC8-C09E-BBDA-FDD6-E40559143548
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/09/2018
ms.openlocfilehash: 0fa66f3a16047c18af19cb7257c778b498bc0c9b
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30774813"
---
# <a name="using-native-libraries"></a>네이티브 라이브러리를 사용 하 여

Xamarin.Android 표준 PInvoke 메커니즘을 통해 네이티브 라이브러리의 사용을 지원 합니다. 운영 체제.apk 프로그램에 포함 되지 않은 추가 네이티브 라이브러리를 묶을 수 있습니다.

Xamarin.Android 응용 프로그램으로 네이티브 라이브러리를 배포 하려면 이진 라이브러리 프로젝트에 추가 하 고 설정의 **빌드 작업** 를 **AndroidNativeLibrary**합니다.

Xamarin.Android 라이브러리 프로젝트를 네이티브 라이브러리를 배포 하려면 이진 라이브러리 프로젝트에 추가 하 고 설정의 **빌드 작업** 를 **EmbeddedNativeLibrary**합니다.

여러 응용 프로그램 이진 인터페이스 (ABIs)를 지 원하는 Android 이후 Xamarin.Android 알고 있어야 한다는 어떤 ABI 네이티브 라이브러리 참고 작성 됩니다.
이는 두 가지 방법으로 수행할 수 있습니다.

1.  경로 "성이"
1.  사용 하 여 프로그램 `AndroidNativeLibrary/Abi` 프로젝트 파일의 요소를


경로 검색을 사용하면 네이티브 라이브러리의 부모 디렉터리 이름을 사용하여 라이브러리가 대상으로 하는 ABI를 지정할 수 있습니다. 따라서 추가 하는 경우 `lib/armeabi/libfoo.so` 을 프로젝트에 다음 ABI 됩니다 수 "스니핑"으로 `armeabi`합니다.

또는 사용 하려면 ABI를 명시적으로 지정 하도록 프로젝트 파일을 편집할 수 있습니다.

```xml
<ItemGroup>
    <AndroidNativeLibrary Include="path/to/libfoo.so">
        <Abi>armeabi</Abi>
    </AndroidNativeLibrary>
</ItemGroup>
```

네이티브 라이브러리를 사용 하는 방법에 대 한 자세한 내용은 참조 [네이티브 라이브러리의 Interop](http://www.mono-project.com/docs/advanced/pinvoke/)합니다.

## <a name="debugging-native-code-with-visual-studio-2015"></a>Visual Studio 2015를 사용 하 여 네이티브 코드 디버깅

사용 중인 경우 *Visual Studio 2015*합니다 (위에서 설명한)에서 프로젝트 파일을 수정할 필요가 없습니다.
작성 하 고 c + +에 대 한 프로젝트 참조를 추가 하 여 c + + 프로그램 Xamarin.Android 솔루션 내 디버그할 수 있습니다 **동적 공유 라이브러리 (Android)** 프로젝트.

Visual Studio c + + 개발자를 볼 수는 [SanAngeles_NativeDebug](https://developer.xamarin.com/samples/monodroid/SanAngeles_NDK/) ; Xamarin 사용한 Visual Studio 2015에서 c + + 디버깅 시도를 샘플링 하 고 참조 우리의 [블로그 게시물](https://blog.xamarin.com/build-and-debug-c-libraries-in-xamarin-android-apps-with-visual-studio-2015/) 자세한 정보에 대 한 합니다.



## <a name="related-links"></a>관련 링크

- [SanAngeles_NativeDebug (샘플)](https://developer.xamarin.com/samples/monodroid/SanAngeles_NDK/)
