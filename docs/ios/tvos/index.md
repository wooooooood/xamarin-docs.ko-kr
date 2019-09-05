---
title: Xamarin의 tvOS 소개
description: 이 문서에서는 Xamarin을 사용 하 여 tvOS apps를 빌드하는 방법을 보여 주는 다양 한 가이드 및 샘플에 연결 합니다. 이 가이드에서는 사용자 인터페이스 개발, 데이터 저장소, 아이콘 등의 다양 한 기능에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: 14345503-1742-41F5-B2EF-EE31AB7C3516
ms.technology: xamarin-ios
ms.custom: xamu-video
author: conceptdev
ms.author: crdun
ms.date: 02/02/2018
ms.openlocfilehash: 688b2b431ca385e8e5bc4ae721e3eaf4049a193d
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70283649"
---
# <a name="introduction-to-tvos-in-xamarin"></a>Xamarin의 tvOS 소개

## <a name="introducing-tvos"></a>TvOS 소개

Apple은 iOS 11에 기반 하 여 최신 버전의 tvOS 운영 체제를 실행 하는 apple tv 하드웨어 (Apple TV 4K)의 5 세대를 출시 했습니다.

Apple TV 플랫폼은 개발자에 게 열려 있어 풍부한 몰입 형 앱을 만들고 Apple TV의 기본 제공 앱 스토어를 통해 릴리스 합니다.

TvOS에 대 한 자세한 내용은 [시작](~/ios/tvos/get-started/index.md) 문서를 참조 하세요.

> [!VIDEO https://youtube.com/embed/Q04oIYymfGM]

**Xamarin 비디오를 사용 하 여 tvOS**

## <a name="documentation"></a>설명서

다음 문서는 Xamarin을 사용 하 여 tvOS apps 빌드를 시작 하는 데 도움이 됩니다.

- [TvOS 11 소개](~/ios/tvos/platform/introduction-to-tvos11.md) -이 문서에서는 tvOS 11 tvOS 개발자에 게 제공 되는 새로운 기능을 설명 합니다.
- [TvOS 10 소개](~/ios/tvos/platform/introduction-to-tvos10/index.md) -이 문서에서는 tvOS 10에서 tvOS 개발자에 게 제공 되는 새로운 api 및 수정 된 api 및 기능을 소개 합니다.
- [TvOS 9 소개](~/ios/tvos/platform/tvos9.md) -이 문서에서는 tvOS 9의 tvOS 개발자에 게 제공 되는 새로운 api 및 수정 된 api 및 기능을 소개 합니다. 
- [Hello, tvOS 빠른 시작 가이드](~/ios/tvos/get-started/hello-tvos.md) –이 가이드는 첫 tvOS 앱을 만드는 과정을 안내 하 고,이 프로세스에서는 Mac용 Visual Studio, Xcode 및 Interface Builder를 포함 하 여 development 도구 체인를 소개 합니다. 또한 코드에 UI 컨트롤을 노출 하는 출 선 및 작업을 소개 하 고, 마지막으로 tvOS 응용 프로그램을 빌드, 실행 및 테스트 하는 방법을 보여 줍니다.
- [아이콘 및 이미지 작업](~/ios/tvos/app-fundamentals/icons-images.md) -이 문서에서는 tvOS 앱 내에서 아이콘과 이미지를 디자인 하 고 사용 하는 방법을 설명 합니다.
- [탐색 및 포커스 작업](~/ios/tvos/app-fundamentals/navigation-focus.md) -이 문서에서는 tvOS 앱 내부에서의 탐색을 제공 하 고 처리 하는 데 중점의 개념과이를 사용 하는 방법을 설명 합니다.
- [리소스 및 데이터 저장소](~/ios/tvos/app-fundamentals/resources-data-storage.md) –이 문서에서는 tvOS 앱에서 리소스 및 영구 데이터 저장소를 사용 하는 방법을 설명 합니다.
- [Siri 원격 및 Bluetooth 컨트롤러](~/ios/tvos/platform/remote-bluetooth.md) –이 문서에서는 tvOS 앱에서 새 Siri 원격 및 bluetooth 게임 컨트롤러를 지 원하는 방법에 대해 설명 합니다.
- [사용자 인터페이스](~/ios/tvos/user-interface/index.md) – tvOS로 작업할 때 UI (사용자 인터페이스) 컨트롤을 포함 하 여 Xcode의 INTERFACE BUILDER 및 ux 디자인 원칙을 포함 하 여 일반 Ux (사용자 환경) 범위입니다.
- [배포, 테스트 및 메트릭](~/ios/tvos/deploy-test/index.md) -이 섹션에서는 응용 프로그램을 배포 하는 방법 뿐만 아니라 앱을 테스트 하는 데 사용 되는 항목을 설명 합니다. 이러한 항목에는 디버깅에 사용 되는 도구, 테스터에 게 배포 및 Apple TV 앱 스토어에 응용 프로그램을 게시 하는 방법이 포함 됩니다.
- [지원 되는 어셈블리](~/ios/tvos/internals/assemblies.md) – Xamarin에서 tvOS 앱에 대해 지 원하는 어셈블리의 목록입니다.
- [지원 되거나 지원 되지 않는 프레임 워크](~/ios/tvos/internals/frameworks.md) -Xamarin에서 tvOS 앱에 대해 지 원하는 프레임 워크의 목록입니다.

## <a name="sample-projects"></a>샘플 프로젝트

Xamarin을 사용 하 여 빌드된 샘플 tvOS 앱:

- [Hello, tvOS](https://docs.microsoft.com/samples/xamarin/ios-samples/tvos-hello-tvos) –이 샘플은 tvOS에서 간단한 "Hello World" 앱을 구현 하 고 tvOS 사용에 대 한 기본 사항을 제공 합니다.
- [Tvalerts](https://docs.microsoft.com/samples/xamarin/ios-samples/tvos-tvalerts) –이 샘플에서는 tvOS 앱에서 경고로 작업 하는 방법을 보여 줍니다.
- [Tvbuttons](https://docs.microsoft.com/samples/xamarin/ios-samples/tvos-tvbuttons) –이 샘플은 단추를 사용 하 여 작업 하는 방법을 보여 줍니다. tvOS 앱입니다.
- [Tvremote](https://docs.microsoft.com/samples/xamarin/ios-samples/tvos-tvremote) –이 샘플에서는 tvOS 앱이 Siri 원격과 상호 작용 하 여 사용자 인터페이스를 탐색할 수 있는 여러 가지 방법을 제공 합니다.
- [Tvcollection](https://docs.microsoft.com/samples/xamarin/ios-samples/tvos-tvcollection) –이 샘플에서는 tvOS 앱에서 컬렉션 뷰 컨트롤러를 사용 하는 방법을 보여 줍니다.
- [tvNavBars](https://docs.microsoft.com/samples/xamarin/ios-samples/tvos-tvnavbars) –이 샘플에서는 tvOS 앱에서 탐색 모음을 사용 하는 방법을 보여 줍니다.
- [Tvpages](https://docs.microsoft.com/samples/xamarin/ios-samples/tvos-tvpages) –이 샘플에서는 tvOS 앱에서 페이지 컨트롤을 사용 하는 방법을 보여 줍니다.
- [Tvprogress](https://docs.microsoft.com/samples/xamarin/ios-samples/tvos-tvprogress) –이 샘플에서는 tvOS 앱에서 진행률 표시기를 사용 하는 방법을 보여 줍니다.
- [Tvsplit](https://docs.microsoft.com/samples/xamarin/ios-samples/tvos-tvsplit) –이 샘플에서는 tvOS 앱에서 분할 뷰 컨트롤러를 사용 하는 방법을 보여 줍니다.
- [tvStackView](https://docs.microsoft.com/samples/xamarin/ios-samples/tvos-tvstackview) -이 샘플에서는 tvOS 앱에서 스택 뷰를 사용 하는 방법을 보여 줍니다.
- [UICatalog](https://docs.microsoft.com/samples/xamarin/ios-samples/tvos-uicatalog) – TvOS의 uikit 프레임 워크에서 다양 한 뷰 및 컨트롤을 사용 하는 방법을 보여 줍니다. 시스템에서 제공 하는 특정 컨트롤이 나 뷰를 찾으려는 경우이 샘플을 참조 하세요.

또한 Apple은 tvOS apps에 대 한 Xamarin의 지원으로 작업 C# 하기 위해로 트랜스 코딩 될 수 있는 다음과 같은 샘플 앱을 제공 합니다.

- [DemoBots: SpriteKit 및 GameplayKit를 사용 하 여 플랫폼 간 게임 빌드](https://developer.apple.com/library/prerelease/tvos/samplecode/DemoBots/)

## <a name="known-issues-and-troubleshooting"></a>알려진 문제 및 문제 해결

Xamarin을 사용 하 여 tvOS를 빌드하는 데 문제가 발생 하면 [릴리스 정보](https://docs.microsoft.com/xamarin/ios/release-notes/), [Xamarin.ios 포럼](https://forums.xamarin.com/categories/ios), [xamarin Bugzilla Tracker](https://bugzilla.xamarin.com/query.cgi?product=iOS)및 [GitHub](https://github.com/xamarin/xamarin-macios/issues) 에서 기존 문제를 확인 하세요.

[GitHub에서](https://github.com/xamarin/xamarin-macios/issues)새로운 문제 및 제안 사항을 보고 합니다.


## <a name="related-links"></a>관련 링크

- [tvOS 샘플](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS 휴먼 인터페이스 가이드](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS에 대 한 앱 프로그래밍 가이드](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
- [Xamarin을 사용 하 여 tvOS 용 앱 빌드 (비디오)](https://university.xamarin.com/lightninglectures/tvos-with-xamarin)
