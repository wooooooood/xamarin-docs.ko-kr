---
title: Xamarin.Android에 대 한 질문과 대답
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 0F0FDD2B-FFB1-476F-B674-81DB3A5E1CF3
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/29/2018
---

# <a name="android-frequently-asked-questions"></a>Android 질문과 대답

## <a name="installation--setup"></a>설치 및 설정

### <a name="which-android-sdk-packages-should-i-installinstall-android-sdk-packagesmd"></a>[어떤 Android SDK 패키지를 설치해야 하나요?](install-android-sdk-packages.md)

Android SDK 설치 개발 하기 위한 최소 필수 패키지를 모두 자동으로 포함 되지 않습니다. 개별 개발자 다르지만 해야이 가이드는 Xamarin.Android를 사용 하 여 개발을 해야 일반적으로 패키지를 설명 합니다.

### <a name="where-can-i-set-my-android-sdk-locationsandroid-sdk-locationmd"></a>[Android SDK 위치를 어디에 설정할 수 있나요?](android-sdk-location.md)

이 가이드에서는 대부분의 설치 프로그램;에 대 한 작동 해야 하는 Android SDK의 기본 설정을 설명 합니다. 및 필요한 경우 Mac 또는 Visual Studio에 대 한 Visual Studio에서이 기본값을 변경 하는 방법입니다.

### <a name="how-do-i-update-the-java-development-kit-jdk-versionupdate-jdkmd"></a>[JDK(Java Development Kit) 버전을 업데이트하려면 어떻게 해야 하나요?](update-jdk.md)

이 문서에서는 Windows 및 Mac. Java 개발 키트 (JDK) 버전을 업데이트 하는 방법을 보여줍니다.

### <a name="can-i-use-java-development-kit-jdk-version-9-or-laterjdk9-errorsmd"></a>[Java 개발 키트 (JDK) 버전 9 이상 사용할 수 있나요?](jdk9-errors.md)

Xamarin.Android는 JDK 8 또는 Microsoft 모바일 OpenJDK 필요합니다. 이 문서에서는 JDK 버전을 확인 하기 위한 지침과 함께 JDK 9 이상이 설치 된 경우 발생할 수 있는 몇 가지 일반적인 오류 메시지를 나열 합니다.


### <a name="how-can-i-manually-install-the-android-support-libraries-required-by-the-xamarinandroidsupport-packagesinstall-android-support-librarymd"></a>[Xamarin.Android.Support 패키지에 필요한 Android 지원 라이브러리를 수동으로 설치하려면 어떻게 해야 하나요?](install-android-support-library.md)

이 가이드에서는 설치에 대 한 예제 단계를 제공 합니다 `Xamarin.Android.Support.v4` Windows & Mac. 지원 라이브러리

### <a name="what-usb-drivers-do-i-need-to-debug-android-on-windowsandroid-drivers-debug-windowsmd"></a>[Windows에서 Android를 디버그해야 하는 USB 드라이버는 무엇인가요?](android-drivers-debug-windows.md)

Windows;에서 개발할 때 Android 장치에서 디버그 하려면 호환 USB 드라이버를 설치 해야 합니다. Android SDK Manager Nexus 장치에 대 한 지원을 추가 하는 기본적으로 "Google USB 드라이버"를 포함 합니다.
다른 장치는 장치 제조업체에서 게시 하는 USB 드라이버 필요 합니다. 이 가이드는 이러한 드라이버와 대체도 테스트 메서드 찾기에 정보를 제공 합니다.

### <a name="is-it-possible-to-connect-to-android-emulators-running-on-a-mac-from-a-windows-vmconnect-android-emulator-mac-windowsmd"></a>[Windows VM에서 Mac에서 실행 중인 Android 에뮬레이터에 연결할 수 있나요?](connect-android-emulator-mac-windows.md)

이 가이드에서는 Android 에뮬레이터를 사용 하는 경우 메서드를 설명 합니다.

## <a name="general-questions"></a>일반적인 질문

### <a name="how-do-i-automate-an-android-nunit-test-projectautomate-android-nunit-testmd"></a>[Android NUnit 테스트 프로젝트를 자동화하려면 어떻게 해야 하나요?](automate-android-nunit-test.md)

이 가이드에서는 Android NUnit 테스트 프로젝트를 설정 하기 위한 단계를 다룹니다 _되지_ Xamarin.UITest 프로젝트입니다. Xamarin.UITest 안내서 [여기](https://docs.microsoft.com/appcenter/test-cloud/preparing-for-upload/uitest)합니다.

### <a name="why-cant-my-android-release-build-connect-to-the-internetandroid-internetmd"></a>[Android 릴리스 빌드를 인터넷에 연결할 수 없는 이유는 무엇인가요?](android-internet.md)

이 문제의 가장 일반적인 원인은 합니다 **인터넷** 권한은 디버그 빌드에서 자동으로 포함 되어 있지만 릴리스 빌드를 수동으로 설정 해야 합니다. 이 가이드에는 릴리스 빌드에 대 한 권한을 사용 하도록 설정 하는 방법을 설명 합니다.

### <a name="smarter-xamarin-android-support-v4--v13-nuget-packagesandroid-support-v4v13-librariesmd"></a>[더 스마트한 Xamarin Android 지원 v4 / v13 NuGet 패키지](android-support-v4v13-libraries.md)

`Support-v4` 및 `Support-v13` 사용할 수 없습니다 함께 동일한 앱에서,는 함께 사용할 수 없습니다. 왜냐하면 `Support-v13` 실제로 형식 및 구현의 모든 포함 `Support-v4`합니다. 해 둘 다 동일한 프로젝트에서 참조 하는 경우 중복 된 형식 오류가 발생 합니다.

### <a name="how-do-i-resolve-a-pathtoolongexception-errorpath-too-long-exceptionmd"></a>[PathTooLongException 오류를 해결 하는 방법](path-too-long-exception.md)

이 문서를 해결 하는 방법에 설명 된 **PathTooLongException** Xamarin.Android 프로젝트를 빌드하는 동안 발생할 수 있는 오류입니다.



## <a name="deprecated"></a>사용되지 않음

> [!NOTE]
> 아래 문서는 Xamarin의 최신 버전에서 해결 된 문제에 적용 됩니다. 그러나 소프트웨어의 최신 버전에 문제가 발생 하면 제출 하세요를 [새 버그](~/cross-platform/troubleshooting/questions/howto-file-bug.md) 정보 및 전체 빌드 로그 출력 전체 버전을 사용 하 여 합니다.

### <a name="what-version-of-xamarinandroid-added-lollipop-supportxa-lollipopmd"></a>[어떤 Xamarin.Android 버전에서 Lollipop 지원이 추가되었나요?](xa-lollipop.md)

이 가이드는 Android L 미리 보기에 대 한 원래 작성 되었습니다. Xamarin.Android 4.17 추가 Android L 미리 보기 지원 및 Xamarin.Android 4.20 Android Lollipop 지원이 추가 되었습니다.

### <a name="androidsupportv7appcompat---no-resource-found-that-matches-the-given-name-attr-androidactionmodesharedrawablemissing-action-mode-share-drawablemd"></a>[Android.Support.v7.AppCompat - 제공된 이름과 일치하는 리소스를 찾을 수 없습니다. attr ‘android:actionModeShareDrawable’](missing-action-mode-share-drawable.md)

이 오류는 필수 Android SDK 패키지의 일부가 누락 된 경우 이전 버전의 Xamarin에서 발생할 수 있습니다.

### <a name="adjusting-java-memory-parameters-for-the-android-designerandroid-designer-java-memorymd"></a>[Android Designer용 Java 메모리 매개 변수 조정](android-designer-java-memory.md)

시작할 때 사용 되는 기본 메모리 매개 변수는 `java` Android designer를 일부 시스템 구성과 호환 되지 않을 대 한 처리 합니다. 이러한 설정은 3.9.344 Visual Studio 용 Xamarin Studio 5.7.2.7 및 Xamarin을 사용 하 여 시작 프로젝트 단위로 지정할 수 있습니다.

### <a name="my-android-resourcedesignercs-file-will-not-updateresource-designer-wont-updatemd"></a>[Android Resource.designer.cs 파일이 업데이트되지 않습니다.](resource-designer-wont-update.md)

Xamarin.Studio 5.1의 버그는 이전에 부분적으로 또는 완전히.csproj 파일의 xml 코드를 삭제 하 여.csproj 파일을 손상입니다. 이 인해 중요 한 부분 Android 빌드 시스템 (예: Android Resource.designer.cs 업데이트)에 실패 합니다. 안정적인 5.1.4 일부 터 7 월 15 일에 릴리스,이 버그가 수정 되었습니다. 하지만 대부분의 프로젝트 파일에이 가이드에 설명 된 대로 수동으로 복구 해야 합니다.



