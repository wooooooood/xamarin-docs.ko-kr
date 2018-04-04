---
title: 프로젝트 참조
description: IOS 응용 프로그램, watch 앱 및 조사식 확장 간의 관계를 설명 합니다.
ms.prod: xamarin
ms.assetid: C366E062-C33D-406A-B3FF-CBE82E5D1E7E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/13/2016
ms.openlocfilehash: f3573e8b578ca567ea9d7360eb132aead4c24f37
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="project-references"></a>프로젝트 참조

_IOS 응용 프로그램, watch 앱 및 조사식 확장 간의 관계를 설명 합니다._

WatchOS 솔루션의 세 가지 프로젝트는 *자동으로 구성 된* 를 빌드하고 올바르게 bundled watchOS 3 앱에 대 한 특정 한 방식으로 서로 참조 합니다. 이러한 프로젝트 참조 및 번들 식별자 설정은 아래 참조에 대 한 설명 되어 있습니다.

## <a name="project-references"></a>프로젝트 참조

각 프로젝트에 대 한 참조 노드를 두 번 클릭 하 여 참조를 참조 하십시오.

- **iPhone 앱** 참조 **Watch 앱**

![](project-references-images/catalog-reference1.png "iPhone 앱 Watch 앱 참조")

- **응용 프로그램을 시청** 참조 **조사식 응용 프로그램 확장**

![](project-references-images/catalog-reference2.png "iPhone 앱 Watch 앱 참조")


 - **조사식 응용 프로그램 확장** 다른 프로젝트 중 하나를 참조 하지 않습니다

![](project-references-images/catalog-reference3.png "조사식 응용 프로그램 확장 다른 프로젝트를 참조 하지 않습니다.")



## <a name="bundle-identifiers"></a>번들 식별자

또한 있는지 확인 해야 하면 **번들 식별자** 올바른지 합니다.
세 프로젝트 모두 있어야는 *동일한* id 접두사의 확장에 미리 정의 되어 있는 두 개의 조사식 프로젝트와 함께 `watchkitextension` 및 `watchkitapp`다음과 같이 (에 대 한는 **WatchKitCatalog** 예):

 - Xamarin.iOS 통합 프로젝트- `com.xamarin.WatchKitCatalog`

 - -WatchKit 확장 프로젝트 `com.xamarin.WatchKitCatalog.watchkitextension`

 - 조사식 응용 프로그램 프로젝트- `com.xamarin.WatchKitCatalog.watchkitapp`

되었는지도 확인 이러한 **Info.plist** 설정이 올바른지:

 - Watch 앱 프로젝트의 `WKCompanionAppBundleIdentifier` 부모/컨테이너 응용 프로그램의 번들 ID와 일치 (ie. iPhone에서 실행 되는 것);

 - 조사식 키트 확장 프로젝트 **WKApp 번들 ID** 일치 Watch 앱 프로젝트의 번들 id입니다.

두 번 클릭 하 여 식별자를 편집할 수 있습니다는 **Info.plist** 각 프로젝트에 파일이 있습니다.

이 스크린샷에서는 **조사식 확장** Info.plist 파일을 보여 주는 **Watch 앱** 도 식별자:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)
    
![](project-references-images/infoplist-extension.png "이 스크린샷에서 조사식 확장의 Info.plist 파일")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)
    
![](project-references-images/infoplist-extension-vs.png "이 스크린샷에서 조사식 확장의 Info.plist 파일")

-----

이 스크린샷에서는 **Watch 앱** Info.plist 파일입니다.
현재 **조사식 OS** 버전은 8.2 하므로 **배포 대상** Watch 앱 이어야 합니다 **8.2**합니다. Xcode 6.3 설치 되어 있는 경우 8.3으로 잘립니다.이 값을 설정할 수 있습니다-변경 해야 하는 참고 8.2 합니다.

![](project-references-images/infoplist-watchapp.png "조사식 Info.plist 파일")

Watch 앱에 대 한 배포 대상 iOS 앱 고 조사식 확장에서 다를 수 있습니다.

