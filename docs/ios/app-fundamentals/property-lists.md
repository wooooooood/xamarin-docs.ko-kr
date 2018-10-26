---
title: Xamarin.iOS에서 속성 목록 사용
description: 이 문서에서는 Entitlements.plist Info.plist와 작업에 대 한 Mac의 그래픽 및 고급 속성 목록 (.plist) 편집기에 대 한 Visual Studio를 소개 합니다. 설정 아이콘 및 시작 이미지에서 iOS 응용 프로그램에 대 한 설명 mac 용 Visual Studio 내에서
ms.prod: xamarin
ms.assetid: 5E687043-0443-377C-9A12-9C5A05958646
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: 7056f7beb623bee32c767a3f2827efa6eb2a6136
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50118801"
---
# <a name="working-with-property-lists-in-xamarinios"></a>Xamarin.iOS에서 속성 목록 사용

_이 문서에서는 Entitlements.plist Info.plist와 작업에 대 한 Mac의 그래픽 및 고급 속성 목록 (.plist) 편집기에 대 한 Visual Studio를 소개 합니다. 설정 아이콘 및 시작 이미지에서 iOS 응용 프로그램에 대 한 설명 mac 용 Visual Studio 내에서_

Mac 용 visual Studio 기능을 쉽게 기능과 앱 속성을 편집 하는 그래픽.plist 편집기입니다. Mac 용 visual Studio에는 두.plists- `Info.plist` 앱 속성 및 아이콘을 편집 및 `Entitlements.plist` 앱 기능을 관리 합니다. 이 가이드는 Info.plists를 소개 하 고 mac 용 Visual Studio에서 작업 하는 개요를 제공 합니다. Entitlements.plist에 대 한 내용은 참조는 [자격](~/ios/deploy-test/provisioning/entitlements.md) 가이드입니다.

## <a name="infoplist"></a>Info.plist

정보 속성 목록 ( `Info.plist`)는 필요한 iOS 파일 시스템에 응용 프로그램의 구성에 대 한 정보를 제공입니다. Mac 용 visual Studio의 사용자 지정 `Info.plist` 편집기 창의 맨 아래에 탭으로 제어 하는 패널 세 개 남아 편집기 기능:

 [![](property-lists-images/tabs.png "편집기 창의 왼쪽 맨 아래에서 Info.plist 편집기 탭")](property-lists-images/tabs.png#lightbox)

각 패널 아래에 설명 된 대로 서로 다른 속성을 제어 합니다.

-  **응용 프로그램 패널** -일반적인 응용 프로그램 속성 뿐만 아니라 아이콘 및 시작 설정 하기 위한 그래픽 인터페이스 이미지; 지도 통합 및 backgrounding 모드를 지정 합니다.
-  **패널 고급** -고급 패널이 지원 되는 문서 유형의 경우 Uti를 지정 하는 위치 및 URL 형식입니다.
-  **원본 패널** -원본 패널 응용 프로그램에 대 한 사용자 지정 속성 뿐만 아니라 덜 일반적인 속성을 제어 합니다.


다음 세 섹션 자세히 각 패널의 기능을 조사합니다.

## <a name="application-panel"></a>응용 프로그램 패널

Mac 용 visual Studio 기능 일반적인 편집 하기 위한 그래픽 인터페이스 `Info.plist` 응용 프로그램에 대 한 항목:

1.  응용 프로그램 속성
1.  지원 되는 장치 유형
1.  각 장치 유형에 대해 지원 방향
1.  상태 표시줄 스타일 및 색
1.  아이콘 및 시작 화면
1.  맵 및 백그라운드 모드


이러한 작업은 다음 섹션에서 자세히 설명 되어 있습니다.

 <a name="iOS_Application_Target" />


### <a name="ios-application-target"></a>iOS 응용 프로그램 대상

이 섹션에서는 응용 프로그램을 설명 하는 중요 한 정보를 포함 합니다.
합니다 **식별자** 저장 여기에 iTunes Connect (앱 스토어 앱) 및 개발 및 배포 인증서 iOS Provisioning Portal 앱 Id 목록에 입력 된 번들 식별자와 일치 해야 합니다.

 [![](property-lists-images/image24.png "iOS 응용 프로그램 대상")](property-lists-images/image24.png#lightbox)

### <a name="device-deployment"></a>장치 배포

 [![](property-lists-images/deployment.png "장치 배포")](property-lists-images/deployment.png#lightbox)

장치 **배포** info 섹션의 선택에 따라 선택적으로 표시 되는 **장치** 드롭다운에서 합니다 **응용 프로그램 대상** 위의 섹션. 합니다 **주 인터페이스** 드롭다운 목록으로 설정 됩니다 **MainStoryboard** 스토리 보드 기반 응용 프로그램에서입니다. 사용자 인터페이스는 코드로 완전히 작성 하는 경우 다음이 비워둘 수 있습니다.

### <a name="supported-device-orientations"></a>지원 되는 장치 방향

 **장치 방향 지원** 앱 장치 회전에 반응 하는 방법을 제어 합니다. IPhone/iPad 앱만 지원 하기 위해 매우 일반적 이기 **세로**, 또는 있지만 **거꾸로**합니다. 일반적으로 게임을 제외한 모든 iPad 응용 프로그램에는 모든 방향을 지원 해야 합니다.

### <a name="status-bar-styles"></a>상태 표시줄 스타일

합니다 **상태 표시줄 스타일** 섹션은 응용 프로그램을 편집 하기 위한 그래픽 인터페이스 `UIStatusBarStyle`:

 [![](property-lists-images/status.png "상태 표시줄 스타일")](property-lists-images/status.png#lightbox)

 <a name="Icons" />


### <a name="icons-launch-images-and-itunes-artwork"></a>아이콘, 시작 이미지 및 iTunes 아트 워크

아이콘, 이미지 및 아트 워크를 사용 하 여 Info.plist 파일에 대 한 정보를 찾을 수 있습니다 합니다 [이미지를 사용 하 여 작업](~/ios/app-fundamentals/images-icons/index.md) 가이드입니다.




### <a name="maps-integration-and-background-modes"></a>지도 통합 및 백그라운드 모드

`Info.plist` 지도 통합 및 backgrounding 모드를 지정 하는 특별 한 섹션이 포함 되어 있습니다. 지원 하려는 옵션을 선택할 수에 대 한 응용 프로그램에 필요한 속성을 추가 합니다.

 [![](property-lists-images/maps.png "지도 통합")](property-lists-images/maps.png#lightbox)

맵 사용 하 여 작업에 대 한 자세한 내용은 Xamarin 참조 [iOS Maps](~/ios/user-interface/controls/ios-maps/index.md) 가이드입니다.

 [![](property-lists-images/bging.png "백그라운드 모드")](property-lists-images/bging.png#lightbox)

백그라운드 모드에 대 한 자세한 내용은 Xamarin 참조 [iOS의 Backgrounding](~/ios/app-fundamentals/backgrounding/introduction-to-backgrounding-in-ios.md) 가이드입니다.

## <a name="advanced-panel"></a>고급 패널

[고급] 패널 문서 형식 및 응용 프로그램을 지 원하는 URL 스키마를 제어 합니다.

 [![](property-lists-images/image34.png "고급 패널")](property-lists-images/image34.png#lightbox)

 <a name="Document_Types" />


## <a name="document-types"></a>문서 형식

IOS 응용 프로그램 특정 파일 형식을 열기 지원에 대해 제공 된 `CFBundleDocumentTypes` 키입니다. -예를 들어 Pdf-특정 알려진된 파일 형식을 지원 하도록 응용 프로그램을 할까요 키 PDF 값의 연결을 추가할 것입니다. 이 섹션에 저장 되는 데이터를 입력 하는 편리한 방법을 제공 합니다 `CFBundleDocumentTypes` 키를 `Info.plist` 파일입니다.

설명서를 참조 하십시오 [파일 형식 Your App 지원 등록](http://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/DocumentInteraction_TopicsForIOS/Articles/RegisteringtheFileTypesYourAppSupports.html) 이러한 값을 구성 하는 방법에 대 한 자세한 내용은 합니다.

## <a name="utis"></a>Uti

경우에 따라 응용 프로그램을 사용자 지정 파일 형식을 열기를 지원 해야 합니다. 예를 들어, 하려는 사용자 지정 확장을 사용 하 여 이미지 파일을 엽니다 *.xam*합니다. 사용자 지정 파일 형식을 지정 하려면 만들어 사용 하 여 사용자 지정-범용 유형 식별자-UTI를 `UIExportedTypeDeclarations` 키입니다. 아래 스크린샷에서.xam 확장에 대 한 사용자 지정 UTI를 만드는 방법을 보여 줍니다.

 [![](property-lists-images/uti.png "Uti 편집기")](property-lists-images/uti.png#lightbox)

특정 앱에 사용자 지정 Uti를 지정 하는 것으로 내보낸된 형식 Uti 합니다 *가져온 형식 Uti* ( `UIImportedTypeDeclarations` 키) 지원 되지만 응용 프로그램을 소유 하지 않은 사용자 지정 형식을 지정 합니다.

사용자 지정 Uti를 사용 하 여에 대 한 자세한 내용은 Apple의 참조 [등록할 파일 형식을 사용자 앱에서 지 원하는](https://developer.apple.com/library/ios/documentation/FileManagement/Conceptual/understanding_utis/understand_utis_declare/understand_utis_declare.html#//apple_ref/doc/uid/TP40001319-CH204-SW1) 가이드.

## <a name="custom-urls"></a>사용자 지정 Url

URL의 첫 번째 부분은 (프로토콜 라고도 함) URL 구성표 이름입니다. 예를 들어 `http://` 고 `https://` 공용 URL 구성표가 있습니다. 응용 프로그램에 대 한 사용자 지정 URL 구성표를 만드는 옵션이 있습니다. 사용자 지정 URL 구성표는 앞뒤로 다른 응용 프로그램을 사용 하 여 데이터를 보내고 통신에 사용 됩니다. 다음 스크린 샷에서 라는 새 사용자 지정 URL 구성표를 만들 방법을 보여 줍니다 `monkeys://`:

 [![](property-lists-images/url.png "사용자 지정 Url")](property-lists-images/url.png#lightbox)



사용자 지정 URL 구성표의 구현에 대 한 자세한 내용은 Apple의 참조 [이 가이드의 섹션에서는 사용자 지정 URL 체계 구현](https://developer.apple.com/library/ios/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/AdvancedAppTricks/AdvancedAppTricks.html)

## <a name="source-panel"></a>원본 패널

**원본** 탭을 `Info.plist` 파일 추가 하거나 편집 하려면 사용자 지정 값을 허용 합니다. Mac 용 visual Studio는 가장 일반적인 속성의 목록을 제공합니다.

 [![](property-lists-images/image31.png "드롭다운에서 새 속성 추가")](property-lists-images/image31.png#lightbox)

다음 스크린샷에 표시 된 것으로 알려진된 속성 Mac 용 Visual Studio에 대 한 유효한 값 목록을 수행 됩니다.

 [![](property-lists-images/image32.png "알려진 값 목록에서 값을 선택 합니다.")](property-lists-images/image32.png#lightbox)

표시 된 대로 Mac 용 visual Studio는 또한 속성 형식 검색:

 [![](property-lists-images/image33.png "사용 가능한 속성 형식")](property-lists-images/image33.png#lightbox)

Apple 검토 [앱 관련 리소스](http://developer.apple.com/library/ios/#DOCUMENTATION/iPhone/Conceptual/iPhoneOSProgrammingGuide/App-RelatedResources/App-RelatedResources.html) 선택적 속성에 대 한 추가 정보에 대 한 링크입니다.

 <a name="Entitlements" />

## <a name="summary"></a>요약

이 문서에서는 지정 아이콘 및 시작 이미지에 대 한 일반적인 앱 구성을 편집 하려면 그래픽 및 고급.plist 편집기를 사용 하 여 보여 줍니다. 또한 도입 하는 `Entitlements.plist` 추가 및 관리 앱 기능에 대 한 합니다.


## <a name="related-links"></a>관련 링크

- [IDE](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide)
- [앱 관련 리소스](http://developer.apple.com/library/ios/#DOCUMENTATION/iPhone/Conceptual/iPhoneOSProgrammingGuide/App-RelatedResources/App-RelatedResources.html)
- [사용자 앱에서 지 원하는 형식 파일을 등록](http://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/DocumentInteraction_TopicsForIOS/Articles/RegisteringtheFileTypesYourAppSupports.html)
- [사용자 지정 URL 구성표를 구현합니다.](https://developer.apple.com/library/ios/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/AdvancedAppTricks/AdvancedAppTricks.html)
- [자산 카탈로그 형식 참조](https://developer.apple.com/library/archive/documentation/Xcode/Reference/xcode_ref-Asset_Catalog_Format/index.html#//apple_ref/doc/uid/TP40015170-CH18-SW1)
