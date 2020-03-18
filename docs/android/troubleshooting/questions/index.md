---
title: Xamarin.Android 질문과 대답
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 0F0FDD2B-FFB1-476F-B674-81DB3A5E1CF3
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 08/29/2018
ms.openlocfilehash: 28b13951a0d681ffdb8e643667e496e0b3606628
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/10/2020
ms.locfileid: "76724004"
---
# <a name="android-frequently-asked-questions"></a>Android 질문과 대답

## <a name="installation--setup"></a>설치 & 설정

### <a name="which-android-sdk-packages-should-i-install"></a>[어떤 Android SDK 패키지를 설치해야 하나요?](install-android-sdk-packages.md)

Android SDK를 설치하면 개발에 필요한 최소 패키지가 자동으로 모두 포함되지는 않습니다. 개발자마다 요구 사항이 다르므로 이 가이드에서는 Xamarin.Android를 사용하여 개발하는 데 일반적으로 필요한 패키지에 대해 설명합니다.

### <a name="where-can-i-set-my-android-sdk-locations"></a>[Android SDK 위치를 어디에 설정할 수 있나요?](android-sdk-location.md)

이 가이드에서는 대부분의 설정에서 작동하는 Android SDK의 기본 설정과 필요에 따라 Mac용 Visual Studio 또는 Visual Studio에서 이러한 기본값을 변경하는 방법을 모두 설명합니다.

### <a name="how-do-i-update-the-java-development-kit-jdk-version"></a>[JDK(Java Development Kit) 버전을 업데이트하려면 어떻게 해야 하나요?](update-jdk.md)

이 문서에서는 Windows 및 Mac에서 JDK(Java Development Kit) 버전을 업데이트하는 방법을 보여줍니다.

### <a name="can-i-use-java-development-kit-jdk-version-9-or-later"></a>[JDK(Java Development Kit) 버전 9 이상을 사용할 수 있나요?](jdk9-errors.md)

Xamarin.Android에는 JDK 8 또는 Microsoft Mobile OpenJDK가 필요합니다. 이 문서에는 JDK 9 이상이 설치된 경우 표시될 수 있는 몇 가지 일반적인 오류 메시지와 JDK 버전을 확인하는 지침이 나와 있습니다.

### <a name="how-can-i-manually-install-the-android-support-libraries-required-by-the-xamarinandroidsupport-packages"></a>[Xamarin.Android.Support 패키지에 필요한 Android 지원 라이브러리를 수동으로 설치하려면 어떻게 해야 하나요?](install-android-support-library.md)

이 가이드에서는 Windows 및 Mac에서 `Xamarin.Android.Support.v4` 지원 라이브러리를 설치하기 위한 예제 단계를 제공합니다.

### <a name="what-usb-drivers-do-i-need-to-debug-android-on-windows"></a>[Windows에서 Android를 디버그해야 하는 USB 드라이버는 무엇인가요?](android-drivers-debug-windows.md)

Windows에서 개발할 때 Android 디바이스에서 디버그하려면 호환되는 USB 드라이버를 설치해야 합니다. Android SDK 관리자에는 기본적으로 Nexus 디바이스에 대한 지원을 추가하는 "Google USB 드라이버"가 포함됩니다.
다른 디바이스에는 해당 디바이스 제조업체에서 게시한 USB 드라이버가 필요합니다. 이 가이드에서는 이러한 드라이버 및 대체 테스트 방법에 대한 정보를 제공합니다.

### <a name="is-it-possible-to-connect-to-android-emulators-running-on-a-mac-from-a-windows-vm"></a>[Windows VM에서 Mac에서 실행 중인 Android 에뮬레이터에 연결할 수 있나요?](connect-android-emulator-mac-windows.md)

이 가이드에서는 Android 에뮬레이터를 사용하는 방법을 설명합니다.

## <a name="general-questions"></a>일반적인 질문

### <a name="how-do-i-automate-an-android-nunit-test-project"></a>[Android NUnit 테스트 프로젝트를 자동화하려면 어떻게 해야 하나요?](automate-android-nunit-test.md)

이 가이드에서는 Xamarin.UITest 프로젝트가 _아닌_ Android NUnit 테스트 프로젝트를 설정하는 단계를 설명합니다. Xamarin.UITest 가이드는 [여기에서](/appcenter/test-cloud/preparing-for-upload) 찾을 수 있습니다.

### <a name="why-cant-my-android-release-build-connect-to-the-internet"></a>[Android 릴리스 빌드를 인터넷에 연결할 수 없는 이유는 무엇인가요?](android-internet.md)

이 문제의 가장 일반적인 원인은 **INTERNET** 권한이 디버그 빌드에는 자동으로 포함되지만 릴리스 빌드에서는 수동으로 설정되어야 한다는 것입니다. 이 가이드에서는 릴리스 빌드에 대한 권한을 설정하는 방법을 설명합니다.

### <a name="smarter-xamarin-android-support-v4--v13-nuget-packages"></a>[더 스마트한 Xamarin Android 지원 v4 / v13 NuGet 패키지](android-support-v4v13-libraries.md)

`Support-v4`와 `Support-v13`은 동일한 앱에서 함께 사용할 수 없습니다. 즉, 상호 배타적입니다. 이는 `Support-v13`이 실제로는 `Support-v4`의 모든 형식 및 구현을 포함하기 때문입니다. 동일한 프로젝트에서 둘 모두를 참조하려 하면 중복 형식 오류가 발생합니다.

### <a name="how-do-i-resolve-a-pathtoolongexception-error"></a>[PathTooLongException 오류를 해결하려면 어떻게 할까요?](path-too-long-exception.md)

이 문서에서는 Xamarin.Android 프로젝트를 빌드하는 동안 발생할 수 있는 **PathTooLongException** 오류를 해결하는 방법을 설명합니다.

## <a name="deprecated"></a>사용되지 않음

> [!NOTE]
> 다음 문서는 최근 버전의 Xamarin에서 해결된 문제에 적용됩니다. 그러나 최신 버전의 소프트웨어에서 문제가 발생하는 경우 전체 버전 정보 및 전체 빌드 로그 출력으로 [새 버그](~/cross-platform/troubleshooting/questions/howto-file-bug.md)를 제출하세요.

### <a name="what-version-of-xamarinandroid-added-lollipop-support"></a>[어떤 Xamarin.Android 버전에서 Lollipop 지원이 추가되었나요?](xa-lollipop.md)

이 가이드는 원래 Android L 미리 보기용으로 작성되었습니다. Xamarin.Android 4.17에는 Android L Preview 지원이 추가되고, Xamarin.Android 4.20에는 Android Lollipop 지원이 추가되었습니다.

### <a name="androidsupportv7appcompat---no-resource-found-that-matches-the-given-name-attr-androidactionmodesharedrawable"></a>[Android.Support.v7.AppCompat - 제공된 이름과 일치하는 리소스를 찾을 수 없습니다. attr ‘android:actionModeShareDrawable’](missing-action-mode-share-drawable.md)

이 오류는 이전 버전의 Xamarin에서 필요한 Android SDK 패키지 중 일부가 누락된 경우 발생할 수 있습니다.

### <a name="adjusting-java-memory-parameters-for-the-android-designer"></a>[Android Designer용 Java 메모리 매개 변수 조정](android-designer-java-memory.md)

Android Designer용 `java` 프로세스를 시작할 때 사용되는 기본 메모리 매개 변수는 일부 시스템 구성과 호환되지 않을 수 있습니다. Xamarin Studio 5.7.2.7 및 Visual Studio용 Xamarin 3.9.344부터 이러한 설정은 프로젝트별로 사용자 지정할 수 있습니다.

### <a name="my-android-resourcedesignercs-file-will-not-update"></a>[Android Resource.designer.cs 파일이 업데이트되지 않습니다.](resource-designer-wont-update.md)

Xamarin.Studio 5.1의 버그는 .csproj 파일에서 xml 코드를 부분적으로 또는 완전히 삭제하여 이전에 .csproj 파일을 손상시켰습니다. 이로 인해 Android 빌드 시스템의 중요한 부분(예: Android Resource.designer.cs 업데이트)이 실패할 수 있습니다. 7월 15일 5.1.4 안정적인 릴리스를 기준으로 이 버그가 수정되었지만, 대부분의 경우 이 가이드에 설명된 대로 프로젝트 파일을 수동으로 복구해야 합니다.
