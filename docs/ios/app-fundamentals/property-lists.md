---
title: "속성 목록 사용"
description: "이 문서에서는 Info.plist Entitlements.plist와 작업에 대 한 Mac의 그래픽 및 고급 속성 목록 (.plist) 편집기에 대 한 Visual Studio를 소개 합니다. 설정 아이콘 및 시작 이미지에서 iOS 응용 프로그램에 대 한 설명 하는지 Mac.에 대 한 Visual Studio 내"
ms.topic: article
ms.prod: xamarin
ms.assetid: 5E687043-0443-377C-9A12-9C5A05958646
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 778e70f6817b71e5910aa85425d46261dfe9c803
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="working-with-property-lists"></a>속성 목록 사용

_이 문서에서는 Info.plist Entitlements.plist와 작업에 대 한 Mac의 그래픽 및 고급 속성 목록 (.plist) 편집기에 대 한 Visual Studio를 소개 합니다. 설정 아이콘 및 시작 이미지에서 iOS 응용 프로그램에 대 한 설명 하는지 Mac.에 대 한 Visual Studio 내_

Mac 용 visual Studio 응용 프로그램 속성 편집 및 보다 쉽게 기능에 수행 하는 그래픽.plist 편집기를 기능을 제공 합니다. Mac 용 visual Studio에 두 개의.plists- `Info.plist` 응용 프로그램 속성 편집 및 아이콘에 대 한 및 `Entitlements.plist` 앱 기능을 관리 합니다. 이 가이드는 Info.plists를 소개 하 고 Mac.에 대 한 Visual Studio에서 이러한 작업의 개요를 제공 합니다. Entitlements.plist 대 한 자세한 내용은 참조는 [자격과 작업](~/ios/deploy-test/provisioning/entitlements.md) 가이드입니다.

## <a name="infoplist"></a>Info.plist

정보 속성 목록 ( `Info.plist`)는 필요한 iOS 파일 시스템에 응용 프로그램의 구성에 대 한 정보를 제공입니다. Mac 용 visual Studio의 사용자 지정 `Info.plist` 편집기 창의 맨 아래에 탭으로 제어 하는 패널 세 개 왼쪽 편집기 기능:

 [![](property-lists-images/tabs.png "편집기 창의 왼쪽 맨 아래에 Info.plist 편집기 탭")](property-lists-images/tabs.png#lightbox)

아래 설명 된 대로 각 패널에서 서로 다른 속성과 제어 합니다.

-  **응용 프로그램 패널** -일반적인 응용 프로그램 속성으로 아이콘 및 시작 설정 하는 그래픽 인터페이스 이미지에 지도 통합 및 backgrounding 모드를 지정 합니다.
-  **패널 고급** -고급 패널이 지원 되는 문서 종류, UTIs를 지정 하는 위치 및 URL 형식입니다.
-  **패널 원본** -소스 패널 일반적이 지 속성 뿐만 아니라 응용 프로그램에 대 한 사용자 지정 속성을 제어 합니다.


다음 3 개의 섹션에서 자세히 각 패널의 기능을 검토 합니다.

## <a name="application-panel"></a>응용 프로그램 패널

Mac 용 visual Studio 기능 일반적인 편집 하기 위한 그래픽 인터페이스 `Info.plist` 응용 프로그램에 대 한 항목:

1.  응용 프로그램 속성
1.  지원 되는 장치 유형
1.  각 장치 유형에 대 한 지원 방향
1.  상태 표시줄 스타일 및 색
1.  아이콘 및 시작 화면
1.  지도 및 백그라운드 모드


이러한 작업은 다음 섹션에서 자세히 설명 되어 있습니다.

 <a name="iOS_Application_Target" />


### <a name="ios-application-target"></a>iOS 응용 프로그램 대상

이 섹션에는 응용 프로그램을 설명 하는 중요 한 정보가 포함 되어 있습니다.
**식별자** 저장 여기에 iTunes Connect (앱 스토어 앱)에 대 한 뿐 아니라 iOS 프로비저닝 포털 응용 프로그램 Id 목록 및 개발 및 배포 하는 인증서에 입력 된 번들 식별자와 일치 해야 합니다.

 [![](property-lists-images/image24.png "iOS 응용 프로그램 대상")](property-lists-images/image24.png#lightbox)

### <a name="device-deployment"></a>장치 배포

 [![](property-lists-images/deployment.png "장치 배포")](property-lists-images/deployment.png#lightbox)

장치 **배포** 정보 섹션에서 선택한 내용에 따라 선택적으로 표시 됩니다는 **장치** 드롭다운에는 **응용 프로그램 대상** 위의 섹션. **주 인터페이스** 드롭 다운으로 설정 되어 **MainStoryboard** 스토리 보드 기반 응용 프로그램에서입니다. 사용자 인터페이스를 완전히 코드에서 작성 한 경우 다음이 수는 비워 둘 수 있습니다.

### <a name="supported-device-orientations"></a>지원 되는 장치 방향

 **장치 방향을 지원** 응용 프로그램 장치 회전과에 응답 하는 방법을 제어 합니다. IPhone/iPad 앱만 지원 하기 위해 매우 일반적 이기 **세로**, 모든 항목 또는 하지만 **거꾸로**합니다. 일반적으로 게임을 제외한 모든 iPad 응용 프로그램에는 모든 방향 지원 해야 합니다.

### <a name="status-bar-styles"></a>상태 표시줄 스타일

**상태 막대 스타일** 섹션은 응용 프로그램의 편집 하기 위한 그래픽 인터페이스 `UIStatusBarStyle`:

 [![](property-lists-images/status.png "상태 표시줄 스타일")](property-lists-images/status.png#lightbox)

 <a name="Icons" />


### <a name="icons-launch-images-and-itunes-artwork"></a>아이콘, 시작 이미지 및 아트 워크 iTunes

아이콘, 이미지 및 아트 워크를 사용 하 여 Info.plist 파일에 대 한 정보를 찾을 수 있습니다는 [이미지 작업](~/ios/app-fundamentals/images-icons/index.md) 가이드입니다.




### <a name="maps-integration-and-background-modes"></a>백그라운드 모드와 지도 통합

`Info.plist` 지도 통합 및 backgrounding 모드를 지정 하는 특별 한 섹션이 포함 되어 있습니다. 지원 하려는 옵션을 선택 하기 위해 응용 프로그램에 필요한 속성을 추가 합니다.

 [![](property-lists-images/maps.png "지도 통합")](property-lists-images/maps.png#lightbox)

지도 사용한 작업에 대 한 자세한 내용은 참조는 xamarin [iOS 지도](~/ios/user-interface/controls/ios-maps/index.md) 가이드입니다.

 [![](property-lists-images/bging.png "백그라운드 모드")](property-lists-images/bging.png#lightbox)

백그라운드 모드에 대 한 자세한 내용은 참조는 xamarin [iOS에서 Backgrounding](~/ios/app-fundamentals/backgrounding/introduction-to-backgrounding-in-ios.md) 가이드입니다.

## <a name="advanced-panel"></a>고급 패널

고급 패널 문서 형식 및 응용 프로그램 지원 URL 스키마를 제어 합니다.

 [![](property-lists-images/image34.png "고급 패널")](property-lists-images/image34.png#lightbox)

 <a name="Document_Types" />


## <a name="document-types"></a>문서 유형

IOS 응용 프로그램을 지 원하는 특정 파일 형식을 열기에 대 한 제공 된 `CFBundleDocumentTypes` 키입니다. 응용 프로그램-예를 들어 Pdf-특정 알려진된 파일 형식을 지원 하기 위해 원하는 경우 PDF 값 키에 추가할 것입니다. 이 섹션에 저장 되는 데이터를 입력 하는 편리한 방법을 제공는 `CFBundleDocumentTypes` 키에 `Info.plist` 파일입니다.

설명서를 참조 [등록 하는 파일 형식 Your 응용 프로그램이 지 원하는](http://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/DocumentInteraction_TopicsForIOS/Articles/RegisteringtheFileTypesYourAppSupports.html) 이러한 값을 구성 하는 방법에 대 한 자세한 내용은 합니다.

## <a name="utis"></a>UTIs

경우에 따라 응용 프로그램 사용자 지정 파일 형식을 열기를 지원 해야 합니다. 예를 들어 사용자 지정 확장 된 이미지 파일 열 해야 할 *.xam*합니다. 사용자 지정 파일 종류를 지정 하려면 만들겠습니다 사용 하 여 사용자 지정-유니버설 유형 식별자-유틸리티는 `UIExportedTypeDeclarations` 키입니다. 아래 스크린샷에서.xam 확장에 대 한 사용자 지정 유틸리티를 만드는 방법을 보여 줍니다.

 [![](property-lists-images/uti.png "UTIs 편집기")](property-lists-images/uti.png#lightbox)

방금 같이 내보낸된 형식 UTIs 응용 프로그램에 특정 사용자 지정 UTIs 지정는 *UTIs 형식을 가져온* ( `UIImportedTypeDeclarations` 키) 지원 되지만 응용 프로그램에서 소유 하지 사용자 지정 형식을 지정 합니다.

사용자 지정 UTIs 사용에 대 한 자세한 내용은 Apple의를 참조 [파일 형식 등록 Your 응용 프로그램이 지 원하는](https://developer.apple.com/library/ios/documentation/FileManagement/Conceptual/understanding_utis/understand_utis_declare/understand_utis_declare.html#//apple_ref/doc/uid/TP40001319-CH204-SW1) 가이드입니다.

## <a name="custom-urls"></a>사용자 지정 Url

URL 구성표 이름 (프로토콜 라고도 함)에 URL의 첫 번째 부분입니다. 예를 들어 `http://` 및 `https://` 일반적인 URL 구성표가 있습니다. 응용 프로그램에 대 한 사용자 지정 URL 구성표를 만들어 하는 옵션이 있습니다. URL 스키마의 사용자 지정 통신 하 고 다른 응용 프로그램 간에 데이터를 보내는 데 사용 됩니다. 다음 스크린샷은 라는 새 사용자 지정 URL 체계를 만드는 과정을 보여 줍니다. `monkeys://`:

 [![](property-lists-images/url.png "사용자 지정 Url")](property-lists-images/url.png#lightbox)



사용자 지정 URL 스키마의 구현에 대 한 자세한 내용은 Apple의를 참조 [이 가이드의 섹션 사용자 지정 URL 스키마의 구현](https://developer.apple.com/library/ios/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/AdvancedAppTricks/AdvancedAppTricks.html)

## <a name="source-panel"></a>소스 패널

**소스** 탭은 `Info.plist` 파일 사용자 지정 값을 추가 하거나 편집할 수 있습니다. Mac 용 visual Studio에는 가장 일반적인 속성 목록을 제공합니다.

 [![](property-lists-images/image31.png "드롭다운에서 새 속성 추가")](property-lists-images/image31.png#lightbox)

Mac 용 Visual Studio의 알려진된 속성에 대 한 유효한 값 목록이 다음 스크린샷에 표시 된 것 처럼을 수행 합니다.

 [![](property-lists-images/image32.png "알려진 값 목록에서 값을 선택 합니다.")](property-lists-images/image32.png#lightbox)

표시 된 대로 Mac 용 visual Studio는 또한 속성 유형 검색:

 [![](property-lists-images/image33.png "사용 가능한 속성 형식")](property-lists-images/image33.png#lightbox)

Apple의 검토 [앱 관련 리소스](http://developer.apple.com/library/ios/#DOCUMENTATION/iPhone/Conceptual/iPhoneOSProgrammingGuide/App-RelatedResources/App-RelatedResources.html) 옵션 속성에 대 한 추가 정보에 대 한 링크입니다.

 <a name="Entitlements" />

## <a name="summary"></a>요약

이 문서는 그래픽 및 고급.plist 편집기를 사용 하 여 아이콘을 지정 하 고 시작 이미지에 대 한 일반적인 응용 프로그램 구성을 편집 하려면 명시 합니다. 것도 도입 했는데이 `Entitlements.plist` 추가 및 관리 응용 프로그램 기능에 대 한 합니다.


## <a name="related-links"></a>관련 링크

- [IDE](https://developer.xamarin.com/recipes/cross-platform/ide)
- [응용 프로그램 관련 리소스](http://developer.apple.com/library/ios/#DOCUMENTATION/iPhone/Conceptual/iPhoneOSProgrammingGuide/App-RelatedResources/App-RelatedResources.html)
- [사용자 응용 프로그램이 지 원하는 형식 파일을 등록](http://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/DocumentInteraction_TopicsForIOS/Articles/RegisteringtheFileTypesYourAppSupports.html)
- [사용자 지정 URL 체계를 구현합니다.](https://developer.apple.com/library/ios/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/AdvancedAppTricks/AdvancedAppTricks.html)
- [자산 카탈로그에 대 한](https://developer.apple.com/library/ioshttps://developer.xamarin.com/recipes/xcode_help-image_catalog-1.0/Recipe.html)
