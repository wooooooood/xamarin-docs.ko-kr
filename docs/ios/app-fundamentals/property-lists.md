---
title: Xamarin.ios에서 속성 목록 작업
description: 이 문서에서는 info.plist 및 info.plist를 사용 하기 위한 Mac용 Visual Studio의 그래픽 및 고급 속성 목록 (. info.plist) 편집기를 소개 합니다. Mac용 Visual Studio 내에서 iOS 응용 프로그램에 대 한 아이콘 및 시작 이미지를 설정 하는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: 5E687043-0443-377C-9A12-9C5A05958646
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/18/2017
ms.openlocfilehash: 6c2b5869f647f65b932b6ec92f359f8a79402c8f
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84569298"
---
# <a name="working-with-property-lists-in-xamarinios"></a>Xamarin.ios에서 속성 목록 작업

_이 문서에서는 info.plist 및 info.plist를 사용 하기 위한 Mac용 Visual Studio의 그래픽 및 고급 속성 목록 (. info.plist) 편집기를 소개 합니다. Mac용 Visual Studio 내에서 iOS 응용 프로그램에 대 한 아이콘 및 시작 이미지를 설정 하는 방법을 보여 줍니다._

Mac용 Visual Studio에는 앱 속성과 기능을 보다 쉽게 편집할 수 있도록 하는 info.plist 편집기가 있습니다. Mac용 Visual Studio에는 `Info.plist` 앱 속성 및 아이콘을 편집 하 고 `Entitlements.plist` 앱 기능을 관리 하기 위한 plists가 있습니다. 이 가이드에서는 plists를 소개 하 고 Mac용 Visual Studio에서 작업에 대 한 개요를 제공 합니다. Info.plist에 대 한 자세한 내용은 [자격 사용](~/ios/deploy-test/provisioning/entitlements.md) 가이드를 참조 하세요.

## <a name="infoplist"></a>Info.plist

정보 속성 목록 ( `Info.plist` )은 시스템에 응용 프로그램의 구성에 대 한 정보를 제공 하는 필수 iOS 파일입니다. Mac용 Visual Studio의 사용자 지정 `Info.plist` 편집기 기능은 편집기 창의 왼쪽 아래에 탭으로 제어 되는 세 개의 패널을 제공 합니다.

 [![](property-lists-images/tabs.png "The Info.plist editor tabs at the bottom left of the editor window")](property-lists-images/tabs.png#lightbox)

각 패널은 아래에 설명 된 대로 서로 다른 속성을 제어 합니다.

- **응용 프로그램 패널** -일반적인 응용 프로그램 속성 뿐만 아니라 아이콘 및 시작 이미지를 설정 하는 그래픽 인터페이스입니다. 지도 통합 및 backgrounding 모드를 지정 합니다.
- **고급 패널** -고급 패널은 지원 되는 문서 유형, UTIS 및 URL 유형을 지정 하는 장소입니다.
- **원본 패널** -원본 패널은 응용 프로그램에 대 한 사용자 지정 속성 뿐만 아니라 더 낮은 공용 속성을 제어 합니다.

다음 세 개의 섹션에서는 각 패널의 기능을 자세히 조사 합니다.

## <a name="application-panel"></a>응용 프로그램 패널

Mac용 Visual Studio는 응용 프로그램의 공통 항목을 편집 하기 위한 그래픽 인터페이스를 제공 합니다 `Info.plist` .

1. 애플리케이션 속성
1. 지원되는 디바이스 유형
1. 각 장치 유형에 대 한 지원 방향
1. 상태 표시줄 스타일 및 색
1. 아이콘 및 시작 화면
1. 지도 및 백그라운드 모드

이러한 내용은 다음 섹션에 자세히 설명 되어 있습니다.

 <a name="iOS_Application_Target"></a>

### <a name="ios-application-target"></a>iOS 응용 프로그램 대상

이 섹션에는 응용 프로그램을 설명 하는 중요 한 정보가 포함 되어 있습니다.
여기에 저장 된 **식별자** 는 iTunes Connect (앱 스토어 앱의 경우) 및 IOS 프로 비전 포털 앱 id 목록 및 개발 및 배포 인증서에 입력 된 번들 식별자와 일치 해야 합니다.

 [![](property-lists-images/image24.png "iOS Application Target")](property-lists-images/image24.png#lightbox)

### <a name="device-deployment"></a>장치 배포

 [![](property-lists-images/deployment.png "Device Deployment")](property-lists-images/deployment.png#lightbox)

장치 **배포** 정보 섹션은 위의 **응용 프로그램 대상** 섹션에서 **장치** 드롭다운의 선택에 따라 선택적으로 표시 됩니다. **주 인터페이스** 드롭다운은 Storyboard 기반 응용 프로그램에서 **mainstoryboard.storyboard** 로 설정 됩니다. 사용자 인터페이스가 코드로 완전히 작성 된 경우 비워 둘 수 있습니다.

### <a name="supported-device-orientations"></a>지원 되는 장치 방향

 **지원 되는 장치 방향은** 앱이 장치 회전에 응답 하는 방식을 제어 합니다. IPhone/iPad 앱에서 **세로**또는 모든 항목을 지 원하는 것은 매우 일반적입니다 **.** 일반적으로 게임을 제외한 모든 iPad 응용 프로그램은 모든 방향을 지원 해야 합니다.

### <a name="status-bar-styles"></a>상태 표시줄 스타일

**상태 표시줄 스타일** 섹션은 응용 프로그램을 편집 하기 위한 그래픽 인터페이스입니다 `UIStatusBarStyle` .

 [![](property-lists-images/status.png "Status Bar Styles")](property-lists-images/status.png#lightbox)

 <a name="Icons"></a>

### <a name="icons-launch-images-and-itunes-artwork"></a>아이콘, 시작 이미지 및 iTunes 아트 워크

Info.plist 파일에서 아이콘, 이미지 및 아트 워크를 사용 하는 방법에 대 한 정보는 [이미지 작업](~/ios/app-fundamentals/images-icons/index.md) 가이드에서 찾을 수 있습니다.

### <a name="maps-integration-and-background-modes"></a>지도 통합 및 백그라운드 모드

에는 `Info.plist` 지도 통합 및 backgrounding 모드를 지정 하기 위한 특수 섹션이 포함 되어 있습니다. 지원 하려는 옵션을 선택 하면 응용 프로그램에 필요한 속성이 추가 됩니다.

 [![](property-lists-images/maps.png "Maps Integration")](property-lists-images/maps.png#lightbox)

맵 사용에 대 한 자세한 내용은 Xamarin [IOS maps](~/ios/user-interface/controls/ios-maps/index.md) 가이드를 참조 하세요.

 [![](property-lists-images/bging.png "Background Modes")](property-lists-images/bging.png#lightbox)

백그라운드 모드에 대 한 자세한 내용은 [iOS의 Xamarin Backgrounding](~/ios/app-fundamentals/backgrounding/introduction-to-backgrounding-in-ios.md) 가이드를 참조 하세요.

## <a name="advanced-panel"></a>고급 패널

고급 패널은 응용 프로그램에서 지 원하는 문서 유형 및 URL 스키마를 제어 합니다.

 [![](property-lists-images/image34.png "Advanced Panel")](property-lists-images/image34.png#lightbox)

 <a name="Document_Types"></a>

## <a name="document-types"></a>문서 유형

특정 형식의 파일을 여는 기능을 지 원하는 응용 프로그램의 경우 iOS에서 키를 제공 합니다 `CFBundleDocumentTypes` . 응용 프로그램에서 알려진 특정 파일 형식 (예: Pdf)을 지원 하려는 경우 PDF 값을 키에 추가 합니다. 이 섹션에서는 파일의 키에 저장 되는 데이터를 편리 하 게 입력할 수 있는 방법을 제공 `CFBundleDocumentTypes` `Info.plist` 합니다.

이러한 값을 구성 하는 방법에 대 한 자세한 내용은 [앱이 지 원하는 파일 형식을 등록](https://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/DocumentInteraction_TopicsForIOS/Articles/RegisteringtheFileTypesYourAppSupports.html) 하는 방법에 대 한 설명서를 참조 하세요.

## <a name="utis"></a>Uti

응용 프로그램에서 사용자 지정 파일 형식 열기를 지원 해야 하는 경우가 있습니다. 예를 들어 사용자 지정 확장 *. xam*을 사용 하 여 이미지 파일을 열 수 있습니다. 사용자 지정 파일 형식을 지정 하려면 키를 사용 하 여 사용자 지정 UTI-범용 형식 식별자를 만듭니다 `UIExportedTypeDeclarations` . 아래 스크린샷에서는 .xam 확장에 대 한 사용자 지정 UTI을 만드는 방법을 보여 줍니다.

 [![](property-lists-images/uti.png "UTIs Editor")](property-lists-images/uti.png#lightbox)

내보낸 형식 UTIs는 사용자 지정 UTIs를 앱에 특정 하 게 지정 하는 것 처럼, *가져온 형식 utis* ( `UIImportedTypeDeclarations` 키)는 지원 되지만 응용 프로그램이 소유 하지 않은 사용자 지정 형식을 지정 합니다.

사용자 지정 UTIs를 사용 하는 방법에 대 한 자세한 내용은 Apple의 [앱에서 지 원하는 파일 형식 등록](https://developer.apple.com/library/ios/documentation/FileManagement/Conceptual/understanding_utis/understand_utis_declare/understand_utis_declare.html#//apple_ref/doc/uid/TP40001319-CH204-SW1) 가이드를 참조 하세요.

## <a name="custom-urls"></a>사용자 지정 URL

Url 체계 이름 (프로토콜이 라고도 함)은 URL의 첫 번째 부분입니다. 예를 들어 `http://` 및 `https://` 는 일반적인 URL 체계입니다. 응용 프로그램에 대 한 사용자 지정 URL 체계를 만들 수 있습니다. 사용자 지정 URL 체계는 다른 응용 프로그램과 주고받는 데이터를 주고받는 데 사용 됩니다. 다음 스크린샷에서는 이라는 새 사용자 지정 URL 구성표를 만드는 방법을 보여 줍니다 `monkeys://` .

 [![](property-lists-images/url.png "Custom URLs")](property-lists-images/url.png#lightbox)

사용자 지정 URL 체계를 구현 하는 방법에 대 한 자세한 내용은 [이 가이드의 Apple의 사용자 지정 Url 구성표 구현 섹션](https://developer.apple.com/library/ios/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/AdvancedAppTricks/AdvancedAppTricks.html) 을 참조 하세요.

## <a name="source-panel"></a>원본 패널

파일의 **원본** 탭을 `Info.plist` 사용 하 여 사용자 지정 값을 추가 하거나 편집할 수 있습니다. Mac용 Visual Studio는 가장 일반적인 속성 목록을 제공 합니다.

 [![](property-lists-images/image31.png "Adding a new property from a dropdown")](property-lists-images/image31.png#lightbox)

알려진 속성의 경우 다음 스크린샷에 표시 된 것 처럼 Mac용 Visual Studio는 유효한 값의 목록입니다.

 [![](property-lists-images/image32.png "Select a value from a know value list")](property-lists-images/image32.png#lightbox)

Mac용 Visual Studio는 다음과 같이 속성 형식도 검색 합니다.

 [![](property-lists-images/image33.png "The available property types")](property-lists-images/image33.png#lightbox)

옵션 속성에 대 한 자세한 내용은 Apple의 [앱 관련 리소스](https://developer.apple.com/library/ios/#DOCUMENTATION/iPhone/Conceptual/iPhoneOSProgrammingGuide/App-RelatedResources/App-RelatedResources.html) 링크를 검토 하세요.

 <a name="Entitlements"></a>

## <a name="summary"></a>요약

이 문서에서는 그래픽 및 info.plist 편집기를 사용 하 여 일반 앱 구성을 편집 하 고 아이콘을 지정 하 고 이미지를 시작 하는 방법을 보여 주었습니다. `Entitlements.plist`응용 프로그램 기능을 추가 하 고 관리 하기 위한도 도입 되었습니다.

## <a name="related-links"></a>관련 링크

- [IDE](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide)
- [앱 관련 리소스](https://developer.apple.com/library/ios/#DOCUMENTATION/iPhone/Conceptual/iPhoneOSProgrammingGuide/App-RelatedResources/App-RelatedResources.html)
- [앱에서 지 원하는 파일 형식 등록](https://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/DocumentInteraction_TopicsForIOS/Articles/RegisteringtheFileTypesYourAppSupports.html)
- [사용자 지정 URL 스키마 구현](https://developer.apple.com/library/ios/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/AdvancedAppTricks/AdvancedAppTricks.html)
- [자산 카탈로그 형식 참조](https://developer.apple.com/library/archive/documentation/Xcode/Reference/xcode_ref-Asset_Catalog_Format/index.html#//apple_ref/doc/uid/TP40015170-CH18-SW1)
