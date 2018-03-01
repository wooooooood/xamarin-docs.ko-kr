---
title: "질문과 대답"
ms.topic: article
ms.prod: xamarin
ms.assetid: 0F0FDD2B-FFB1-476F-B674-81DB3A5E1CF3
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: d6d1a5f659317267859164181216177857a67fd2
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="frequently-asked-questions"></a>질문과 대답

## <a name="installation--setup"></a>설치 및 설정

### <a name="which-android-sdk-packages-should-i-installinstall-android-sdk-packagesmd"></a>[Android SDK 패키지를 설치 해야 합니까?](install-android-sdk-packages.md)

Android SDK 설치 개발 하기 위한 최소 필수 패키지를 모두 자동으로 포함 되지 않습니다. 개인 개발자 다 필요이 가이드에서는 Xamarin.Android를 사용 하 여 개발에 대해 받아야 일반적으로 패키지를 설명 합니다.

### <a name="where-can-i-set-my-android-sdk-locationsandroid-sdk-locationmd"></a>[내 Android SDK 위치 설정](android-sdk-location.md)

이 가이드에서는 대부분의 설치 프로그램;에 대해 작동 하는 Android SDK의 기본 설정을 설명합니다 및 필요한 경우 Mac 이나 Visual Studio에 대 한 Visual Studio에서이 기본값을 변경 하는 방법입니다.

### <a name="how-do-i-update-the-java-development-kit-jdk-versionupdate-jdkmd"></a>[키트 JDK (Java Development) 버전을 업데이트 하려면 어떻게 해야 합니까?](update-jdk.md)

이 문서에서는 Windows 및 Mac.에 키트 JDK (Java Development) 버전을 업데이트 하는 방법을 보여줍니다.

### <a name="can-i-use-java-development-kit-jdk-version-9jdk9-errorsmd"></a>[Java Development Kit (JDK) 버전 9 사용할 수 있습니까?](jdk9-errors.md)

Xamarin.Android는 JDK 9 보다는 JDK 8 필요합니다. 이 문서에서는 JDK 9가 설치 되어 있는지, JDK 버전 검사에 대 한 지침과 함께 볼 수 있는 몇 가지 일반적인 오류 메시지를 나열 합니다.


### <a name="how-can-i-manually-install-the-android-support-libraries-required-by-the-xamarinandroidsupport-packagesinstall-android-support-librarymd"></a>[수동으로 Xamarin.Android.Support 패키지에 필요한 Android 지원 라이브러리를 설치할 수는 방법](install-android-support-library.md)

설치에 대 한 예제 단계를 제공 하는이 가이드는 `Xamarin.Android.Support.v4` windows& Mac.에 지원 라이브러리

### <a name="how-do-i-install-google-play-services-in-an-emulatorinstall-gpsmd"></a>[에뮬레이터에서 Google Play 서비스를 설치 하려면 어떻게 합니까?](install-gps.md)

이 가이드는 Visual Studio의 Android 에뮬레이터에서 Google Play 서비스를 설치 하는 방법에 대 한 정보를 링크 합니다.

### <a name="what-usb-drivers-do-i-need-to-debug-android-on-windowsandroid-drivers-debug-windowsmd"></a>[Windows에서 Android 디버그에 어떤 USB 드라이버 필요 합니까?](android-drivers-debug-windows.md)

Windows;에서 개발할 때 Android 장치에서 디버깅 하려면 호환 되는 USB 드라이버를 설치 해야 합니다. Nexus 장치에 대 한 지원을 추가 하는 기본적으로 "Google USB 드라이버"를 포함 하는 Android SDK Manager.
다른 장치에는 장치 제조업체에서 구체적으로 게시 하는 USB 드라이버가 필요 합니다. 이 가이드에서는 테스트도 대체 방법 이들이 드라이버를 찾는 방법에 정보를 제공 합니다.

### <a name="is-it-possible-to-connect-to-android-emulators-running-on-a-mac-from-a-windows-vmconnect-android-emulator-mac-windowsmd"></a>[Android 에뮬레이터에 연결할 수 Mac에서 실행 하는 Windows VM에서?](connect-android-emulator-mac-windows.md)

이 가이드에서는 Google Android 에뮬레이터에서 앱을 사용 하는 경우 메서드를 설명 합니다.

## <a name="general-questions"></a>일반적인 질문

### <a name="how-do-i-automate-an-android-nunit-test-projectautomate-android-nunit-testmd"></a>[Android NUnit 테스트 프로젝트를 자동화 하는 방법](automate-android-nunit-test.md)

이 가이드는 Android NUnit 테스트 프로젝트를 설정 하기 위한 단계를 설명 _하지_ Xamarin.UITest 프로젝트. Xamarin.UITest 설명서를 참조할 수 있습니다 [여기](https://docs.microsoft.com/appcenter/test-cloud/preparing-for-upload/uitest)합니다.

### <a name="how-do-i-enable-intellisense-in-android-axml-filesenable-axml-intellisensemd"></a>[Android.axml 파일에서 Intellisense는 어떻게 사용 합니까?](enable-axml-intellisense.md)

이 가이드에서는 Android.axml 파일에 대 한 Visual Studio의 Intellisense를 활성화 하는 방법에 설명 합니다.

### <a name="why-cant-my-android-release-build-connect-to-the-internetandroid-internetmd"></a>[내 Android 릴리스 빌드는 인터넷에 연결할 수 없는 이유는?](android-internet.md)

이 문제의 가장 일반적인 이유는 **인터넷** 권한은 자동으로 디버그 빌드에 연결 되어 있지만 릴리스 빌드에 대해 수동으로 설정 해야 합니다. 이 가이드에서는 릴리스 빌드에 대 한 권한을 설정 하는 방법에 설명 합니다.

### <a name="smarter-xamarin-android-support-v4--v13-nuget-packagesandroid-support-v4v13-librariesmd"></a>[더 효율적인 Xamarin Android v4 지원 / v13 NuGet 패키지](android-support-v4v13-libraries.md)

`Support-v4` 및 `Support-v13` 사용할 수 없습니다 함께 동일한 앱에서 즉는 함께 사용할 수 없습니다. 때문에 이것이 `Support-v13` 실제로 형식 및의 구현을 모두 포함 되어 `Support-v4`합니다. 시도 하 고 모두 동일한 프로젝트에서 참조 하는 경우에 중복 되는 형식 오류가 발생 합니다.


## <a name="deprecated"></a>사용 되지 않음

> [!NOTE]
> **참고:** 아래 문서 최신 버전의 Xamarin에 해결 된 문제에 적용 합니다. 그러나 최신 버전의 소프트웨어에 문제가 발생 하면 보관는 [새 버그](~/cross-platform/troubleshooting/questions/howto-file-bug.md) 정보 및 전체 빌드 로그 출력 전체 프로그램 버전 관리를 사용 합니다.

### <a name="what-version-of-xamarinandroid-added-lollipop-supportxa-lollipopmd"></a>[Xamarin.Android의 버전 롤리팝 지원이 추가 되었습니다.](xa-lollipop.md)

이 가이드는 Android L 미리 보기에 대 한 원래 작성 되었습니다. 4.17 Xamarin.Android 추가 Android L 미리 보기 지원 및 Xamarin.Android 4.20 Android 롤리팝 지원이 추가 됩니다.

### <a name="androidsupportv7appcompat---no-resource-found-that-matches-the-given-name-attr-androidactionmodesharedrawablemissing-action-mode-share-drawablemd"></a>[Android.Support.v7.AppCompat-지정 된 이름과 일치 하는 리소스를 찾을 수: ' android: actionModeShareDrawable' attr](missing-action-mode-share-drawable.md)

이 오류는 필요한 Android SDK pacakages 일부가 누락 된 경우 이전 버전의 Xamarin에 발생할 수 있습니다.

### <a name="adjusting-java-memory-parameters-for-the-android-designerandroid-designer-java-memorymd"></a>[Android 디자이너에 대 한 Java 메모리 매개 변수를 조정합니다.](android-designer-java-memory.md)

시작할 때 사용 되는 기본 메모리 매개 변수는 `java` Android 디자이너 일부 시스템 구성에 호환 되지 않을 수 있습니다에 대 한 처리 합니다. 이러한 설정은 3.9.344 Visual Studio 용 Xamarin Studio 5.7.2.7 및 Xamarin이 시작 프로젝트 수준이에 지정할 수 있습니다.

### <a name="my-android-resourcedesignercs-file-will-not-updateresource-designer-wont-updatemd"></a>[Android Resource.designer.cs 파일이 업데이트 되지 않습니다.](resource-designer-wont-update.md)

이전에 Xamarin.Studio 5.1의 버그는 부분적으로 또는 완전히.csproj 파일의 xml 코드를 삭제 하 여.csproj 파일 손상 되었습니다. 이 인해 시스템 (예: Android Resource.designer.cs 업데이트)을 작성 하는 Android의 일부 중요 한 실패 합니다. 7 월 15 일에 발표 안정적인 5.1.4 기준으로,이 버그는 수정 되었습니다. 하지만 대부분의 경우에서이 가이드에 설명 된 대로 수동으로 수정 해야 하는 프로젝트 파일에 있습니다.



