---
title: Xamarin.iOS 응용 프로그램 기본 사항
description: 이 문서는 다양 한 가이드 Xamarin.iOS 개발에 앱 전송 보안과 같은 기본 개념을 설명 하는 backgrounding, 이벤트 및 스레딩에 연결 합니다.
ms.prod: xamarin
ms.assetid: 608403AE-B09F-4D9C-8F59-F9DE9F0B1CF1
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/21/2017
ms.openlocfilehash: 120bbfe0d5fa91e632fc56ee05431f5555653360
ms.sourcegitcommit: a153623a69b5cb125f672df8007838afa32e9edf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67268598"
---
# <a name="xamarinios-application-fundamentals"></a>Xamarin.iOS 응용 프로그램 기본 사항

이 섹션에서는 일반적인 작업 작업 또는 개발자가 Xamarin.iOS (이전 MonoTouch) 응용 프로그램을 개발할 때 알아야 할 필요는 개념 중 일부에 제공 합니다.

## <a name="accessibilityiosapp-fundamentalsaccessibilitymd"></a>[액세스 가능성](~/ios/app-fundamentals/accessibility.md)

이 문서에서는 다양 한 Api 및 최대한 많은 사용자에 게 액세스할 수 있는 응용 프로그램을 빌드하는 데 사용할 수 있는 도구를 설명 합니다.

## <a name="app-transport-securityiosapp-fundamentalsatsmd"></a>[앱 전송 보안](~/ios/app-fundamentals/ats.md)

이 문서는 Xamarin.iOS 프로젝트에 대 한 의미 및 iOS 9 앱에 앱 전송 보안을 적용 하는 보안 변경 내용, ATS 구성 옵션을 살펴봅니다 고 필요한 경우 옵트아웃 ATS, 하는 방법을 다룰 것입니다. ATS는 기본적으로 사용 하도록 설정 하기 때문에 모든 안전 하지 않은 인터넷 연결 (하지 않는 한 명시적으로 허용한 것) iOS 9 앱의 경우 예외가 발생 합니다.

## <a name="backgroundingiosapp-fundamentalsbackgroundingindexmd"></a>[Backgrounding](~/ios/app-fundamentals/backgrounding/index.md)

백그라운드 처리 또는 backgrounding은 응용 프로그램이 다른 응용 프로그램이 포그라운드에서 실행 되는 동안 백그라운드에서 작업을 수행할 수 있도록 프로세스입니다. 이 가이드는 iOS에서 처리 하는 백그라운드 소개 됩니다.

## <a name="creating-ios-applications-in-codeiosapp-fundamentalsios-code-onlymd"></a>[코드에서 iOS 응용 프로그램 만들기](~/ios/app-fundamentals/ios-code-only.md)

이 기사에서는 mac 용 Visual Studio 및 Visual Studio를 사용 하 여 코드에서 완전히 iOS 응용 프로그램을 만드는 방법 검토 화면을 빌드하는 응용 프로그램 컨트롤러의 UIKit의 계층 뷰를 만들어 빈 프로젝트 템플릿에서 시작 하는 방법을 보여 줍니다. 그런 다음 컨트롤러에서 로드할 수 있는 사용자 지정 보기를 만드는 방법에 설명 합니다.

## <a name="exception-marshalingiosplatformexception-marshalingmd"></a>[예외 마샬링](~/ios/platform/exception-marshaling.md)

Objective-c와 관리 되는 예외 네이티브 및 관리 되는 프레임 간에 마샬링되는 방법을 설명 합니다.

## <a name="events-protocols-and-delegatesiosapp-fundamentalsdelegates-protocols-and-eventsmd"></a>[이벤트, 프로토콜 및 대리자](~/ios/app-fundamentals/delegates-protocols-and-events.md)

이 문서에서는 콜백을 수신 하는 사용자 인터페이스 컨트롤을 데이터로 채우는 데 사용 되는 키 iOS 기술을 제공 합니다. 이러한 기술은 이벤트, 프로토콜 및 대리자 이 문서에서는 새로운 설명 이러한 각각은 및에서 C# 사용은 각 방법. Xamarin.iOS가 Xamarin.iOS 프로토콜 및 대리자 같은 Objective-c 개념에 대 한 지원을 제공 하는 방법과 친숙 한.NET 이벤트를 노출 하도록 iOS 컨트롤을 사용 하는 방법을 보여 줍니다 (Objective-c 대리자 혼동 해서는 안 됩니다 C# 대리자를 사용 하 여). 이 문서는 또한 사용 방법을 보여 주는 프로토콜은 모두 기준으로 비 대리자 시나리오 및 Objective-c 대리자에 대 한 예제를 제공 합니다.

## <a name="working-with-the-file-systemiosapp-fundamentalsfile-systemmd"></a>[파일 시스템 작업](~/ios/app-fundamentals/file-system.md)

Xamarin.iOS 모든.NET 응용 프로그램에서 사용할 수 있는 iOS의 파일 및 디렉터리를 사용 하려면 동일한 System.IO 클래스를 사용할 수 있습니다. 그러나 친숙 한 클래스 및 메서드를 불구 하 고 iOS 생성 하거나 액세스할 수 있는 파일에 몇 가지 제한 사항이 구현 및 특별 한 기능도 제공 특정 디렉터리입니다. 이 문서에서는 이러한 제한 사항 및 기능에 간략하게 설명 하 고 Xamarin.iOS 응용 프로그램에서 파일 액세스의 작동 방식을 보여 줍니다.

## <a name="working-with-imagesiosapp-fundamentalsimages-iconsindexmd"></a>[이미지 작업](~/ios/app-fundamentals/images-icons/index.md)

이 아티클에서 Xamarin.iOS, (예: 아이콘, 이미지 등 로드) 응용 프로그램 지원 이미지와 이미지 (예: 이미지 컨트롤에 적용) 응용 프로그램 내에서 이미지를 사용 하는 방법을 살펴봅니다. 또한 코드에서 이미지를 사용 하 여 상호 작용 하는 방법 뿐만 아니라 이미지를 통합 하기 위해 Mac 용 Visual Studio를 사용 하는 방법을 다룹니다.

## <a name="localizationiosapp-fundamentalslocalizationindexmd"></a>[지역화](~/ios/app-fundamentals/localization/index.md)

이 가이드에서는 인코딩 국제화 지원 하기 위해 Xamarin.iOS 응용 프로그램을 추가 합니다.

## <a name="working-with-property-listsiosapp-fundamentalsindexmd"></a>[속성 목록 사용](~/ios/app-fundamentals/index.md)

이 문서에서는 Entitlements.plist Info.plist와 작업에 대 한 Mac의 그래픽 및 고급 속성 목록 (.plist) 편집기에 대 한 Visual Studio를 소개 합니다. 설정 아이콘에 설명 하 고 시작 iOS 응용 프로그램에 대 한 이미지에서 지정 하 여 앱 기능 (자격)을 보여 줍니다. mac 용 Visual Studio 내에서

## <a name="working-with-security-and-privacyiosapp-fundamentalssecurity-privacymd"></a>[보안 및 개인 정보 사용](~/ios/app-fundamentals/security-privacy.md)

Apple는 개발자가 앱의 보안을 강화 하 고 최종 사용자의 개인 정보 보호에 도움이 되는 보안 및 개인 정보 보호 iOS 10 개 (이상)에 모두 몇 가지 향상을 했습니다. 이 문서에서는 Xamarin.iOS 앱에서 이러한 기능을 구현 다룰 것입니다.

## <a name="threadingiosapp-fundamentalsthreadingmd"></a>[스레딩](~/ios/app-fundamentals/threading.md)

이 문서에서는 Xamarin.iOS 응용 프로그램에서는 스레딩에 대해 설명 하 고.NET 스레드 풀, 응답성이 뛰어난 응용 프로그램 및 가비지 수집에 대 한 비트를 설명 합니다.

## <a name="touchiosapp-fundamentalstouchindexmd"></a>[터치](~/ios/app-fundamentals/touch/index.md)

여러 오늘날의 장치에서 터치 스크린에 빠르고 효율적으로 자연스럽 고 직관적인 방식으로 장치와 상호 작용할 수가 있습니다. 이 상호 작용 바로 간단한 터치 감지에 제한 되지 않습니다. – 제스처도 사용할 수 있습니다. 예를 들어 축소 확대/축소 제스처가이 – 매우 일반적인 예로 사용자 수 확대 또는 축소 하는 두 손가락을 사용 하 여 화면의 밀기입니다. 이 가이드에는 터치 및 제스처를 iOS에서 검사합니다.

## <a name="working-with-user-defaultsiosapp-fundamentalsuser-defaultsmd"></a>[사용자 기본값 사용](~/ios/app-fundamentals/user-defaults.md)

`NSUserDefaults` 클래스 iOS 앱 및 확장 시스템 차원의 기본 시스템을 사용 하 여 프로그래밍 방식으로 상호 작용 하는 방법을 제공 합니다. 기본적으로 시스템을 사용 하 여 사용자 앱의 동작 또는 (앱의 디자인에 따라) 해당 기본 설정에 맞게 스타일 지정을 구성할 수 있습니다. 예를 들어 야드/파운드법 측정 메트릭 vs의 데이터를 제공 하거나 특정된 UI 테마를 선택 합니다.
