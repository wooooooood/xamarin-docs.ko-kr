---
title: Xamarin Android 질문과 대답
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 0F0FDD2B-FFB1-476F-B674-81DB3A5E1CF3
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/29/2018
ms.openlocfilehash: c14c03d4f618644382aa80b5e0e7fc5b7a46fa9b
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70760871"
---
# <a name="android-frequently-asked-questions"></a>Android faq (질문과 대답)

## <a name="installation--setup"></a>설치 & 설치

### <a name="which-android-sdk-packages-should-i-installinstall-android-sdk-packagesmd"></a>[어떤 Android SDK 패키지를 설치해야 하나요?](install-android-sdk-packages.md)

Android SDK를 설치 하면 개발에 필요한 모든 최소 패키지가 자동으로 포함 되지 않습니다. 개별 개발자 요구 사항은 다르지만이 가이드에서는 일반적으로 Xamarin.ios를 사용 하 여 개발 하는 데 필요한 패키지에 대해 설명 합니다.

### <a name="where-can-i-set-my-android-sdk-locationsandroid-sdk-locationmd"></a>[Android SDK 위치를 어디에 설정할 수 있나요?](android-sdk-location.md)

이 가이드에서는 대부분의 설정에서 작동 해야 하는 Android SDK의 기본 설정에 대해 설명 합니다. 필요에 따라 Mac용 Visual Studio 또는 Visual Studio에서 이러한 기본값을 변경 하는 방법을 설명 합니다.

### <a name="how-do-i-update-the-java-development-kit-jdk-versionupdate-jdkmd"></a>[JDK(Java Development Kit) 버전을 업데이트하려면 어떻게 해야 하나요?](update-jdk.md)

이 문서에서는 Windows 및 Mac에서 JDK (Java Development Kit) 버전을 업데이트 하는 방법을 보여 줍니다.

### <a name="can-i-use-java-development-kit-jdk-version-9-or-laterjdk9-errorsmd"></a>[JDK (Java Development Kit) 버전 9 이상을 사용할 수 있나요?](jdk9-errors.md)

Xamarin Android에는 JDK 8 또는 Microsoft Mobile OpenJDK가 필요 합니다. 이 문서에는 jdk 9 이상이 설치 된 경우에 표시 될 수 있는 몇 가지 일반적인 오류 메시지와 JDK 버전을 확인 하는 지침이 나와 있습니다.

### <a name="how-can-i-manually-install-the-android-support-libraries-required-by-the-xamarinandroidsupport-packagesinstall-android-support-librarymd"></a>[Xamarin.Android.Support 패키지에 필요한 Android 지원 라이브러리를 수동으로 설치하려면 어떻게 해야 하나요?](install-android-support-library.md)

이 가이드에서는 Windows & Mac에 지원 `Xamarin.Android.Support.v4` 라이브러리를 설치 하는 방법에 대 한 예제 단계를 제공 합니다.

### <a name="what-usb-drivers-do-i-need-to-debug-android-on-windowsandroid-drivers-debug-windowsmd"></a>[Windows에서 Android를 디버그해야 하는 USB 드라이버는 무엇인가요?](android-drivers-debug-windows.md)

Windows에서 개발할 때 Android 장치에서 디버깅 하려면 호환 되는 USB 드라이버를 설치 해야 합니다. Android SDK 관리자는 기본적으로 Nexus 장치에 대 한 지원을 추가 하는 "Google USB 드라이버"를 포함 합니다.
다른 장치에는 장치 제조업체에서 게시 한 USB 드라이버가 필요 합니다. 이 가이드에서는 이러한 드라이버 및 대체 테스트 방법에 대 한 정보를 제공 합니다.

### <a name="is-it-possible-to-connect-to-android-emulators-running-on-a-mac-from-a-windows-vmconnect-android-emulator-mac-windowsmd"></a>[Windows VM에서 Mac에서 실행 중인 Android 에뮬레이터에 연결할 수 있나요?](connect-android-emulator-mac-windows.md)

이 가이드에서는 Android 에뮬레이터를 사용 하는 방법에 대해 설명 합니다.

## <a name="general-questions"></a>일반적인 질문

### <a name="how-do-i-automate-an-android-nunit-test-projectautomate-android-nunit-testmd"></a>[Android NUnit 테스트 프로젝트를 자동화하려면 어떻게 해야 하나요?](automate-android-nunit-test.md)

이 가이드에서는 UITest 프로젝트가 _아닌_ Android NUnit 테스트 프로젝트를 설정 하는 단계에 대해 설명 합니다. UITest 가이드는 [여기](https://docs.microsoft.com/appcenter/test-cloud/preparing-for-upload/uitest)에서 찾을 수 있습니다.

### <a name="why-cant-my-android-release-build-connect-to-the-internetandroid-internetmd"></a>[Android 릴리스 빌드를 인터넷에 연결할 수 없는 이유는 무엇인가요?](android-internet.md)

이 문제의 가장 일반적인 원인은 **인터넷** 권한이 디버그 빌드에 자동으로 포함 되지만 릴리스 빌드에 대해 수동으로 설정 해야 한다는 것입니다. 이 가이드에서는 릴리스 빌드에 대 한 사용 권한을 설정 하는 방법을 설명 합니다.

### <a name="smarter-xamarin-android-support-v4--v13-nuget-packagesandroid-support-v4v13-librariesmd"></a>[더 스마트한 Xamarin Android 지원 v4 / v13 NuGet 패키지](android-support-v4v13-libraries.md)

`Support-v4`및 `Support-v13` 는 동일한 앱에서 함께 사용할 수 없습니다. 즉, 함께 사용할 수 없습니다. 이는 실제로 `Support-v13` 의 `Support-v4`모든 형식 및 구현을 포함 하기 때문입니다. 동일한 프로젝트에서 두 항목을 모두 참조 하는 경우 중복 형식 오류가 발생 합니다.

### <a name="how-do-i-resolve-a-pathtoolongexception-errorpath-too-long-exceptionmd"></a>[PathTooLongException 오류를 해결 어떻게 할까요??](path-too-long-exception.md)

이 문서에서는 Xamarin Android 프로젝트를 빌드하는 동안 발생할 수 있는 **PathTooLongException** 오류를 해결 하는 방법을 설명 합니다.

## <a name="deprecated"></a>사용되지 않음

> [!NOTE]
> 다음 문서는 최신 버전의 Xamarin에서 해결 된 문제에 적용 됩니다. 그러나 최신 버전의 소프트웨어에서 문제가 발생 하는 경우 전체 버전 정보 및 전체 빌드 로그 출력을 사용 하 여 [새 버그](~/cross-platform/troubleshooting/questions/howto-file-bug.md) 를 작성 하세요.

### <a name="what-version-of-xamarinandroid-added-lollipop-supportxa-lollipopmd"></a>[어떤 Xamarin.Android 버전에서 Lollipop 지원이 추가되었나요?](xa-lollipop.md)

이 가이드는 원래 Android L preview 용으로 작성 되었습니다. Xamarin Android 4.17에 Android L Preview 지원이 추가 & Xamarin. android 4.20에 Android 롤리팝 지원이 추가 되었습니다.

### <a name="androidsupportv7appcompat---no-resource-found-that-matches-the-given-name-attr-androidactionmodesharedrawablemissing-action-mode-share-drawablemd"></a>[Android.Support.v7.AppCompat - 제공된 이름과 일치하는 리소스를 찾을 수 없습니다. attr ‘android:actionModeShareDrawable’](missing-action-mode-share-drawable.md)

이 오류는 필요한 Android SDK 패키지 중 일부가 누락 된 경우 이전 버전의 Xamarin에서 발생할 수 있습니다.

### <a name="adjusting-java-memory-parameters-for-the-android-designerandroid-designer-java-memorymd"></a>[Android Designer용 Java 메모리 매개 변수 조정](android-designer-java-memory.md)

Android designer에 대 한 프로세스를 `java` 시작할 때 사용 되는 기본 메모리 매개 변수는 일부 시스템 구성과 호환 되지 않을 수 있습니다. Xamarin Studio 5.7.2.7 및 Xamarin for Visual Studio 3.9.344에서 시작 하 여 이러한 설정은 프로젝트별로 사용자 지정할 수 있습니다.

### <a name="my-android-resourcedesignercs-file-will-not-updateresource-designer-wont-updatemd"></a>[Android Resource.designer.cs 파일이 업데이트되지 않습니다.](resource-designer-wont-update.md)

.Csproj 파일에서 xml 코드를 부분적으로 또는 완전히 삭제 하 여 이전에 손상 된 .csproj 파일의 버그를 5.1 합니다. 이로 인해 android 빌드 시스템의 중요 한 부분 (예: Android Resource.designer.cs 업데이트)이 실패할 수 있습니다. 7 월 15 일에 5.1.4 안정적인 릴리스를 기준으로이 버그가 수정 되었습니다. 그러나이 가이드에서 설명 하는 것 처럼 대부분의 경우 프로젝트 파일을 수동으로 복구 해야 합니다.
