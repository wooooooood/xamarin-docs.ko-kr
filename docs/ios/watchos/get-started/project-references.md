---
title: watchOS에서 Xamarin 프로젝트 참조
description: 이 문서는 iOS 앱, watch 앱 및 watch 앱 확장 간의 관계를 설명 합니다. 프로젝트 참조 및 번들에 설명 식별자입니다.
ms.prod: xamarin
ms.assetid: C366E062-C33D-406A-B3FF-CBE82E5D1E7E
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 09/13/2016
ms.openlocfilehash: 60eaad98e2d5469e9c43e6b4ad889080e1aa63ba
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67832059"
---
# <a name="watchos-project-references-in-xamarin"></a>watchOS에서 Xamarin 프로젝트 참조

_IOS 앱, watch 앱 및 watch 확장의 관계에 대 한 설명._

WatchOS 솔루션의 세 프로젝트가 *자동으로 구성 된* 빌드하고 올바르게 번들 3 watchOS 앱에 대 한 특정 한 방식으로 서로 참조 합니다. 이러한 프로젝트 참조 및 번들 식별자 설정은 아래 참조에 대 한 설명 되어 있습니다.

## <a name="project-references"></a>프로젝트 참조

각 프로젝트에 대 한 참조 노드를 두 번 클릭 하 여 참조를 참조 하십시오.

- **iPhone 앱** 참조가 **Watch 앱**

  ![](project-references-images/catalog-reference1.png "iPhone 앱 Watch 앱 참조")

- **앱을 시청** 참조가 **Watch 앱 확장**

  ![](project-references-images/catalog-reference2.png "iPhone 앱 Watch 앱 참조")


- 합니다 **Watch 앱 확장** 다른 프로젝트 중 하나를 참조 하지 않습니다

  ![](project-references-images/catalog-reference3.png "Watch 앱 확장 다른 프로젝트를 참조 하지 않습니다.")



## <a name="bundle-identifiers"></a>번들 식별자

확인 해야 하면 **번들 식별자** 올바른지 합니다.
세 프로젝트 모두 있어야 합니다 *동일한* 확장의 미리 정의 된 것 두 조사식 프로젝트 식별자 접두사 `watchkitextension` 및 `watchkitapp`같이 (에 대 한를 **WatchKitCatalog** 예):

- Xamarin.iOS 통합 프로젝트 `com.xamarin.WatchKitCatalog`

- WatchKit 확장 프로젝트 `com.xamarin.WatchKitCatalog.watchkitextension`

- Watch 앱 프로젝트 `com.xamarin.WatchKitCatalog.watchkitapp`

해야 이러한 **Info.plist** 설정이 올바릅니다.

- Watch 앱 프로젝트의 `WKCompanionAppBundleIdentifier` 부모/컨테이너 앱의 번들 ID와 일치 (ie. iPhone에서 실행 되는 것);

- 조사식 키트 확장 프로젝트의 **WKApp 번들 ID** Watch 앱 프로젝트의 번들 ID와 일치

두 번 클릭 하 여 식별자를 편집할 수 있습니다 합니다 **Info.plist** 각 프로젝트에는 파일입니다.

스크린샷 합니다 **조사식 확장** Info.plist 파일을 보여 주는 합니다 **Watch 앱** 식별자도:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)
    
![](project-references-images/infoplist-extension.png "이 스크린샷에서 조사식 확장의 Info.plist 파일")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)
    
![](project-references-images/infoplist-extension-vs.png "이 스크린샷에서 조사식 확장의 Info.plist 파일")

-----

이 스크린샷은 합니다 **Watch 앱** Info.plist 파일입니다.
현재 **Watch OS** 버전이 8.2 하므로 **배포 대상을** Watch 앱 이어야 합니다 **8.2**합니다. 설치 된 Xcode 6.3에 있는 경우이 값을 8.3으로 설정할 수 있습니다-변경 해야 하는 참고 8.2.

![](project-references-images/infoplist-watchapp.png "Info.plist 파일 보기")

Watch 앱의 배포 대상 iOS 앱 및 Watch 확장에서 다를 수 있습니다.

