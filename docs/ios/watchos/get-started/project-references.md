---
title: Xamarin에서 watchOS 프로젝트 참조
description: 이 문서에서는 iOS 앱, watch 앱 및 watch 앱 확장 간의 관계를 설명 합니다. 프로젝트 참조 및 번들 식별자에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: C366E062-C33D-406A-B3FF-CBE82E5D1E7E
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 09/13/2016
ms.openlocfilehash: dcadb5146df39aa4887e28b65078acc9454f3d34
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70767982"
---
# <a name="watchos-project-references-in-xamarin"></a>Xamarin에서 watchOS 프로젝트 참조

_IOS 앱, 시청 앱 및 조사식 확장 간의 관계에 대 한 설명입니다._

WatchOS 솔루션의 세 프로젝트는 watchOS 3 앱이 올바르게 빌드되고 함께 제공 되는 특정 방식으로 서로를 참조 하도록 *자동으로 구성* 됩니다. 이러한 프로젝트 참조 및 번들 식별자 설정은 아래에서 참조할 수 있도록 설명 합니다.

## <a name="project-references"></a>프로젝트 참조

각 프로젝트에 대 한 참조 노드를 두 번 클릭 하 여 참조를 봅니다.

- **iPhone 앱** 참조 **시청 앱**

  ![](project-references-images/catalog-reference1.png "iPhone 앱 참조 시청 앱")

- **시청 앱** 참조 시청 **앱 확장**

  ![](project-references-images/catalog-reference2.png "iPhone 앱 참조 시청 앱")

- **Watch 앱 확장이** 다른 프로젝트 중 하나를 참조 하지 않습니다.

  ![](project-references-images/catalog-reference3.png "Watch 앱 확장이 다른 프로젝트를 참조 하지 않습니다.")

## <a name="bundle-identifiers"></a>번들 식별자

또한 **번들 식별자** 가 올바른지 확인 해야 합니다.
세 프로젝트 모두에서 다음과 같이 및 `watchkitextension` `watchkitapp`의 확장을 미리 정의 하 여 두 개의 조사식 프로젝트에 동일한 식별자 접두사를 사용 해야 합니다 ( **WatchKitCatalog** 예의 경우).

- Xamarin.ios 통합 프로젝트-`com.xamarin.WatchKitCatalog`

- WatchKit 확장 프로젝트-`com.xamarin.WatchKitCatalog.watchkitextension`

- 응용 프로그램 프로젝트 보기-`com.xamarin.WatchKitCatalog.watchkitapp`

또한 이러한 info.plist 설정이 올바른지 확인 **합니다** .

- Watch 앱 프로젝트 `WKCompanionAppBundleIdentifier` 는 부모/컨테이너 앱의 번들 ID (즉, iPhone에서 실행 되는 번들 ID)와 일치 합니다.

- Watch Kit 확장 프로젝트의 **WKApp 번들 id** 는 시청 앱 프로젝트의 번들 id와 일치 합니다.

각 프로젝트에서 **info.plist** 파일을 두 번 클릭 하 여 식별자를 편집할 수 있습니다.

이 스크린샷은 watch **앱의** 식별자도 보여 주는 **시청 확장의** info.plist 파일입니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![](project-references-images/infoplist-extension.png "이 스크린샷은 시청 확장의 info.plist 파일입니다.")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![](project-references-images/infoplist-extension-vs.png "이 스크린샷은 시청 확장의 info.plist 파일입니다.")

-----

이 스크린샷은 **Watch 앱의** info.plist 파일입니다.
현재 **WATCH OS** 버전은 8.2 이므로 watch 앱의 **배포 대상은** **8.2**이어야 합니다. Xcode 6.3가 설치 된 경우이 값을 8.3로 설정할 수 있습니다 .이 값을 8.2로 변경 해야 합니다.

![](project-references-images/infoplist-watchapp.png "Watch info.plist 파일")

Watch 앱의 배포 대상은 조사식 확장 및 iOS 앱과 다를 수 있습니다.
