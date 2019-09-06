---
title: Xamarin.ios 응용 프로그램 기본 사항
description: 이 문서는 앱 전송 보안, backgrounding, 이벤트 및 스레딩과 같은 Xamarin.ios 개발의 기본 개념을 설명 하는 다양 한 가이드에 연결 됩니다.
ms.prod: xamarin
ms.assetid: 608403AE-B09F-4D9C-8F59-F9DE9F0B1CF1
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 06/21/2017
ms.openlocfilehash: 59257dafc1d92756feb85046df43de7b9da0cc42
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70290149"
---
# <a name="xamarinios-application-fundamentals"></a>Xamarin.ios 응용 프로그램 기본 사항

이 섹션에서는 Xamarin.ios (이전의 Monotouch.dialog) 응용 프로그램을 개발할 때 개발자가 알고 있어야 하는 몇 가지 일반적인 작업 또는 개념에 대 한 지침을 제공 합니다.

## <a name="accessibilityiosapp-fundamentalsaccessibilitymd"></a>[액세스 가능성](~/ios/app-fundamentals/accessibility.md)

이 문서에서는 가능한 한 많은 사용자가 액세스할 수 있는 응용 프로그램을 빌드하는 데 도움이 되는 다양 한 Api 및 도구에 대해 설명 합니다.

## <a name="app-transport-securityiosapp-fundamentalsatsmd"></a>[앱 전송 보안](~/ios/app-fundamentals/ats.md)

이 문서에서는 앱 전송 보안이 iOS 9 앱에 적용 하는 보안 변경 내용 및이에 해당 하는 ATS 구성 옵션을 설명 하 고, 필요한 경우 ATS를 옵트아웃 하는 방법을 설명 합니다. ATS은 기본적으로 사용 하도록 설정 되어 있으므로 안전 하지 않은 모든 인터넷 연결이 iOS 9 앱에서 예외를 발생 시킵니다 (명시적으로 허용 하지 않는 한).

## <a name="backgroundingiosapp-fundamentalsbackgroundingindexmd"></a>[Backgrounding](~/ios/app-fundamentals/backgrounding/index.md)

백그라운드 처리 또는 backgrounding 다른 응용 프로그램이 포그라운드에서 실행 되는 동안 응용 프로그램이 백그라운드에서 작업을 수행할 수 있도록 하는 프로세스입니다. 이 가이드는 iOS의 백그라운드 처리를 소개 하는 역할을 합니다.

## <a name="creating-ios-applications-in-codeiosapp-fundamentalsios-code-onlymd"></a>[코드에서 iOS 응용 프로그램 만들기](~/ios/app-fundamentals/ios-code-only.md)

이 문서에서는 Visual Studio 및 Mac용 Visual Studio를 사용 하 여 완전히 코드에서 iOS 응용 프로그램을 만드는 방법을 살펴봅니다. 이 예제에서는 UIKit에서 보기의 계층 구조를 만들어 컨트롤러에 응용 프로그램 화면을 빌드하는 빈 프로젝트 템플릿에서 시작 하는 방법을 보여 줍니다. 그런 다음 컨트롤러에 로드할 수 있는 사용자 지정 뷰를 만드는 방법을 설명 합니다.

## <a name="exception-marshalingiosplatformexception-marshalingmd"></a>[예외 마샬링](~/ios/platform/exception-marshaling.md)

기본 및 관리 되는 프레임 간에 목표-C 및 관리 되는 예외가 마샬링되는 방법에 대해 설명 합니다.

## <a name="events-protocols-and-delegatesiosapp-fundamentalsdelegates-protocols-and-eventsmd"></a>[이벤트, 프로토콜 및 대리자](~/ios/app-fundamentals/delegates-protocols-and-events.md)

이 문서에서는 콜백을 수신 하 고 사용자 인터페이스 컨트롤을 데이터로 채우는 데 사용 되는 주요 iOS 기술을 제공 합니다. 이러한 기술은 이벤트, 프로토콜 및 대리자입니다. 이 문서에서는 각 항목의 정의와 각 항목을에서 C#사용 하는 방법을 설명 합니다. Xamarin.ios에서 iOS 컨트롤을 사용 하 여 친숙 한 .NET 이벤트를 표시 하는 방법과 Xamarin.ios에서 프로토콜 및 대리자와 같은 목적-C 개념을 지 원하는 방법을 보여 줍니다. 목표-C 대리자는 대리자와 C# 혼동 해서는 안 됩니다. 또한이 문서에서는 프로토콜이 목표-C 대리자 및 비 대리자 시나리오에 대 한 기본으로 사용 되는 방법을 보여 주는 예제를 제공 합니다.

## <a name="working-with-the-file-systemiosapp-fundamentalsfile-systemmd"></a>[파일 시스템 작업](~/ios/app-fundamentals/file-system.md)

Xamarin.ios는 동일한 System.IO 클래스를 사용 하 여 .NET 응용 프로그램에서 사용 하는 iOS의 파일 및 디렉터리 작업을 수행할 수 있습니다. 그러나 친숙 한 클래스 및 메서드에도 불구 하 고, iOS는 만들거나 액세스할 수 있는 파일에 대 한 몇 가지 제한 사항을 구현 하 고 특정 디렉터리에 대 한 특수 기능을 제공 합니다. 이 문서에서는 이러한 제한 사항 및 기능에 대해 간략하게 설명 하 고 Xamarin.ios 응용 프로그램에서 파일 액세스의 작동 방식을 보여 줍니다.

## <a name="working-with-imagesiosapp-fundamentalsimages-iconsindexmd"></a>[이미지 작업](~/ios/app-fundamentals/images-icons/index.md)

이 문서에서는 Xamarin.ios에서 이미지를 사용 하는 방법, 즉 응용 프로그램 지원 이미지 (예: 아이콘, 이미지 로드 등) 및 응용 프로그램 내의 이미지 (예: 컨트롤에 적용 된 이미지)를 사용 하는 방법을 살펴봅니다. 또한 Mac용 Visual Studio를 사용 하 여 이미지를 통합 하는 방법 뿐만 아니라 코드에서 이미지와 상호 작용 하는 방법에 대해서도 설명 합니다.

## <a name="localizationiosapp-fundamentalslocalizationindexmd"></a>[지역화](~/ios/app-fundamentals/localization/index.md)

이 가이드에서는 국제화를 지원 하기 위해 Xamarin.ios 응용 프로그램에 인코딩 추가에 대해 설명 합니다.

## <a name="working-with-property-listsiosapp-fundamentalsindexmd"></a>[속성 목록 작업](~/ios/app-fundamentals/index.md)

이 문서에서는 info.plist 및 info.plist를 사용 하기 위한 Mac용 Visual Studio의 그래픽 및 고급 속성 목록 (. info.plist) 편집기를 소개 합니다. IOS 응용 프로그램에 대 한 아이콘 및 시작 이미지를 설정 하는 방법 및 내부 Mac용 Visual Studio에서 앱 기능 (자격)을 지정 하는 방법을 보여 줍니다.

## <a name="working-with-security-and-privacyiosapp-fundamentalssecurity-privacymd"></a>[보안 및 개인 정보 작업](~/ios/app-fundamentals/security-privacy.md)

Apple에서는 개발자가 앱의 보안을 개선 하 고 최종 사용자의 개인 정보를 확인 하는 데 도움이 되는 iOS 10의 보안 및 개인 정보 모두에 대해 몇 가지 기능이 향상 되었습니다. 이 문서에서는 Xamarin.ios 앱에서 이러한 기능을 구현 하는 방법을 다룹니다.

## <a name="threadingiosapp-fundamentalsthreadingmd"></a>[스레딩](~/ios/app-fundamentals/threading.md)

이 문서에서는 Xamarin.ios 응용 프로그램의 스레딩을 설명 하 고 .NET 스레드 풀, 응답성이 뛰어난 응용 프로그램 및 가비지 수집에 대해 설명 합니다.

## <a name="touchiosapp-fundamentalstouchindexmd"></a>[터치](~/ios/app-fundamentals/touch/index.md)

오늘날의 많은 장치에서 터치 스크린을 사용 하면 사용자가 쉽고 직관적인 방법으로 장치를 빠르고 효율적으로 조작할 수 있습니다. 이 상호 작용은 간단한 터치 검색 으로만 제한 되지 않으며 제스처도 사용할 수 있습니다. 예를 들어 확대/축소 제스처는이에 대 한 매우 일반적인 예입니다. 즉, 사용자가 확대 하거나 축소할 수 있는 두 손가락으로 화면의 일부를 집기 합니다. 이 가이드에서는 iOS의 터치 및 제스처를 검사 합니다.

## <a name="working-with-user-defaultsiosapp-fundamentalsuser-defaultsmd"></a>[사용자 기본값 작업](~/ios/app-fundamentals/user-defaults.md)

클래스 `NSUserDefaults` 는 iOS 앱 및 확장이 시스템 차원의 기본 시스템과 프로그래밍 방식으로 상호 작용 하는 방법을 제공 합니다. 사용자는 기본값 시스템을 사용 하 여 앱의 동작 또는 스타일을 구성 하 여 앱의 디자인에 따라 기본 설정을 충족 시킬 수 있습니다. 예를 들어 메트릭 및 왕정 측정의 데이터를 표시 하거나 지정 된 UI 테마를 선택 합니다.
