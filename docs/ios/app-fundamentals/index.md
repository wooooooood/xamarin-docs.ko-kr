---
title: "응용 프로그램 기본 사항"
description: "응용 프로그램의 핵심 개념"
ms.topic: article
ms.prod: xamarin
ms.assetid: DBBAA086-BB0F-8161-DF44-632F4F5DFE5D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/21/2017
ms.openlocfilehash: 9cace37a9e762a49a90963591931793d30d05453
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="application-fundamentals"></a>응용 프로그램 기본 사항

이 섹션에서 일반적인 작업 작업 또는 개발자가 Xamarin.iOS (이전의 MonoTouch) 응용 프로그램을 개발할 때 알고 있어야 하는 개념 중 일부는 지침을 제공 합니다.

## <a name="app-transport-securityiosapp-fundamentalsatsmd"></a>[앱의 전송 보안](~/ios/app-fundamentals/ats.md)

이 문서에서는 소개 Xamarin.iOS 프로젝트에 대 한 의미 및 iOS 9 응용 프로그램에 앱 전송 보안을 적용 하는 보안 변경 내용, AT 구성 옵션에 적용 됩니다 및 필요한 경우 되지 않게 AT, 하는 방법에 적용 됩니다. AT 기본적으로 사용 하도록 설정 하기 때문에 안전 하지 않은 모든 인터넷 연결 (하지 않은 것을 명시적으로 허용 된) iOS 9 앱에서 예외가 발생 합니다.


## <a name="backgroundingiosapp-fundamentalsbackgroundingindexmd"></a>[Backgrounding](~/ios/app-fundamentals/backgrounding/index.md)

백그라운드 처리 또는 backgrounding은 응용 프로그램이 전경에 다른 응용 프로그램을 실행 하는 동안 백그라운드에서 작업을 수행할 수 있도록 하는 과정입니다. 이 가이드는 백그라운드 처리에 iOS에 대 한 기본적인 사용 됩니다.


## <a name="events-protocols-and-delegatesiosapp-fundamentalsdelegates-protocols-and-eventsmd"></a>[이벤트, 프로토콜 및 대리자](~/ios/app-fundamentals/delegates-protocols-and-events.md)

이 문서에서는 콜백을 받으려면 하 고 사용자 인터페이스 컨트롤을 데이터로 채우는 데 사용 되는 키 iOS 기술을 제공 합니다. 이러한 기술은 이벤트, 프로토콜 및 대리자; 이 문서에 대해 설명는 이러한 각와 C#에서 사용 방법 각각. Xamarin.iOS 이벤트를 노출할 친숙 한.NET도 Xamarin.iOS 프로토콜, 대리자 등 Objective-c 개념에 대 한 지원을 제공 하는 방법에 따라 iOS 컨트롤을 사용 하는 방법을 보여 줍니다 (Objective-c 대리자 혼동 해서는 안 C# 대리자와 함께). 이 문서는 또한 사용 방법을 보여 주는 프로토콜은 모두 기준으로 대리자가 아닌 시나리오에서 및 Objective-c 대리자에 대 한 예를 제공 합니다.

## <a name="threadingiosapp-fundamentalsthreadingmd"></a>[스레딩](~/ios/app-fundamentals/threading.md)

이 문서는 Xamarin.iOS 응용 프로그램의 스레딩에 대 한 약간 설명 하 고는.NET 스레드 풀, 응답성이 뛰어난 응용 프로그램 및 가비지 수집 합니다.&nbsp;

## <a name="working-with-imagesiosapp-fundamentalsimages-iconsindexmd"></a>[이미지 작업](~/ios/app-fundamentals/images-icons/index.md)

이 문서 Xamarin.iOS 합니다 (예: 아이콘, 이미지 로드) 응용 프로그램 지원 이미지와 응용 프로그램 (예: 이미지 컨트롤에 적용) 내에서 이미지를 모두에서 이미지를 사용 하는 방법을 검사 합니다. 또한 코드에서 이미지와 상호 작용 하는 방법 뿐만 아니라 Mac 용 Visual Studio를 사용 하 여 이미지를 통합 하는 방법을 다룹니다.

## <a name="working-with-property-listsiosapp-fundamentalsindexmd"></a>[속성 목록 사용](~/ios/app-fundamentals/index.md)

이 문서에서는 Info.plist Entitlements.plist와 작업에 대 한 Mac의 그래픽 및 고급 속성 목록 (.plist) 편집기에 대 한 Visual Studio를 소개 합니다. 설정 아이콘에 설명 하 고 시작 iOS 응용 프로그램에 대 한 이미지 및에서 지정 하 여 앱 기능 (권한 부여)을 보여 줍니다. Mac.에 대 한 Visual Studio 내

## <a name="working-with-the-file-systemiosapp-fundamentalsfile-systemmd"></a>[파일 시스템 작업](~/ios/app-fundamentals/file-system.md)

Xamarin.iOS 모든.NET 응용 프로그램에서 사용할 수 있는 iOS의 파일 및 디렉터리를 사용 하는 동일한 System.IO 클래스를 사용할 수 있습니다. 그러나 친숙 한 클래스 및 메서드를 불구 하 고 iOS 생성 하거나 액세스할 수 있는 파일에 몇 가지 제한 사항이 구현 및 특별 한 기능도 제공 특정 디렉터리. 이 문서에서는 이러한 제한 사항 및 기능에 설명 하 고 파일 액세스 Xamarin.iOS 응용 프로그램에서 작동 하는 방법을 보여 줍니다.

## <a name="creating-ios-applications-in-codeiosapp-fundamentalsios-code-onlymd"></a>[코드에서 iOS 응용 프로그램 만들기](~/ios/app-fundamentals/ios-code-only.md)

이 문서 검사 Mac.에 대 한 Visual Studio 및 Visual Studio를 사용 하 여 코드에서 완전히 iOS 응용 프로그램을 만드는 방법 UIKit에서 보기의 계층 구조를 만들어서는 컨트롤러에는 응용 프로그램 화면을 작성 하기 위한 빈 프로젝트 템플릿을에서 시작 하는 방법을 보여 줍니다. 그런 다음는 컨트롤러에서 로드할 수 있는 사용자 지정 보기를 만드는 방법을 설명 합니다.

## <a name="working-with-user-defaultsiosapp-fundamentalsuser-defaultsmd"></a>[사용자 기본값 사용](~/ios/app-fundamentals/user-defaults.md)

`NSUserDefaults` ios 앱 및 확장 프로그래밍 방식으로 시스템 차원의 기본 시스템과 상호 작용 하는 방법을 제공 합니다. 기본적으로 시스템을 사용 하 여 사용자는 앱의 동작이 나 (응용 프로그램의 디자인에 기반)의 기본 설정을 충족 하기 위해 스타일을 지정 구성할 수 있습니다. 예를 들어, 제국 측정 단위가 미터법 vs에서 데이터를 표시 하거나 특정된 UI 테마를 선택 합니다.

## <a name="touchiosapp-fundamentalstouchindexmd"></a>[터치](~/ios/app-fundamentals/touch/index.md)

여러 오늘날의 장치에 터치 스크린이 빠르고 효율적으로 상호 작용할 수 장치와 자연스럽 고 직관적인 방식입니다. 이 상호 작용 간단한 터치 검색만 제한 되지 않습니다. – 제스처도 사용할 수 있습니다. 예를 들어 핀치 확대/축소 제스처는 두 손가락 사용자 수 확대 / 축소 된 화면의 일부를 모으는으로 매우 일반적인 예 –이입니다. 이 가이드는 터치 및 제스처 iOS에서 검사합니다.

## <a name="working-with-security-and-privacyiosapp-fundamentalssecurity-privacymd"></a>[보안 및 개인 정보 사용](~/ios/app-fundamentals/security-privacy.md)

Apple는 개발자가 앱의 보안을 개선 하 고 최종 사용자의 개인 정보 보호에 도움이 되는 보안과 iOS 10 (및 큰)의 개인 정보에 몇 가지 개선 되어를 했습니다. 이 문서에서는 Xamarin.iOS 앱에서 이러한 기능을 구현 설명 합니다.

##  <a name="localizationiosapp-fundamentalslocalizationindexmd"></a>[지역화](~/ios/app-fundamentals/localization/index.md)

이 가이드에서 설명 된 인코딩 국제화 지원 하기 위해 Xamarin.iOS 응용 프로그램에 추가 합니다.