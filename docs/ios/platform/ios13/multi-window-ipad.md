---
title: Xamarin.ios의 iPad 용 다중 창
description: IPad 응용 프로그램에 여러 창 지원을 추가 합니다.
ms.prod: xamarin
ms.assetid: 524b6f2e-dbdf-11e9-8a34-2a2ae2dbcce4
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 09/20/2019
ms.openlocfilehash: ce7df59d41efdd2d151fd2ea73cf26b40ee7fa10
ms.sourcegitcommit: 834466c9d9cf35e9659e467ce0123e5f5ade6138
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/22/2020
ms.locfileid: "85129911"
---
# <a name="multiple-windows-for-ipad"></a>IPad용 다중 창

이제 iOS 13은 iPad의 동일한 앱에 대해 side-by-side 창을 지원 합니다. 그러면 windows 간에 새로운 환경 및 끌어서 놓기 상호 작용을 수행할 수 있습니다. 이 문서에서는이 기능을 지원 하도록 응용 프로그램을 설정 하는 방법을 보여 주고 이러한 새로운 기능을 소개 합니다. 

## <a name="project-configuration"></a>프로젝트 구성

여러 창에 대해 프로젝트를 구성 하려면를 수정 하 여 `info.plist` iOS에 게 `NSUserActivityTypes` 이 형식의 활동을 처리 하 고 `UIApplicationSceneManifest` 여러 창에 대해를 사용 하도록 설정 하 `UIApplicationSupportsMultipleScenes` 고 `UISceneConfigurations` 장면을 스토리 보드와 연결 합니다.

```xml
<key>NSUserActivityTypes</key>
<array>
    <string>com.xamarin.Gallery.openDetail</string>
</array>
<key>UIApplicationSceneManifest</key>
<dict>
    <key>UIApplicationSupportsMultipleScenes</key>
    <true/>
    <key>UISceneConfigurations</key>
    <dict>
        <key>UIWindowSceneSessionRoleApplication</key>
        <array>
            <dict>
                <key>UISceneConfigurationName</key>
                <string>Default Configuration</string>
                <key>UISceneDelegateClassName</key>
                <string>SceneDelegate</string>
                <key>UISceneStoryboardFile</key>
                <string>Main</string>
            </dict>
        </array>
    </dict>
</dict>
```

## <a name="opening-multiple-windows"></a>여러 창 열기

앱이 열려 있고 iPad에서 실행 되는 경우 iOS에서 기본적으로 처리 하는 해당 앱의 여러 창을 여는 몇 가지 방법이 있습니다.

- 앱이 열려 있는 **동안 앱에서** dock의 앱 아이콘을 탭 하 여이 모드로 전환 합니다.
- **위로 이동** -실행 중인 앱의 맨 위에 있는 도크에서 앱 아이콘을 끌어 부동 창을 만듭니다.
- **화면 분할** -앱 아이콘을 iPad 화면에서 가장자리로 끌어서 놓아 새 side-by-side 창을 만듭니다.

여러 창 모드로 전환 하기 위한 추가 상호 작용은 응용 프로그램 내에서 사용할 수 있습니다.

**앱에서 끌어서** 놓기-앱 내에서 끌기 상호 작용을 사용 하 여 `NSUserActivity` 이전 예제에서 앱 아이콘을 끄는 것과 같은 새를 시작 합니다.

[끌어서 놓기 상호 작용][0]을 사용 하는 경우를 만들고 `NSUserActivity` 해당 데이터를 iOS에서 열도록 요청 하는 새 창에 연결 합니다.

```csharp
public UIDragItem [] GetItemsForBeginningDragSession (UICollectionView collectionView, IUIDragSession session, NSIndexPath indexPath)
{
    var selectedPhoto = GetPhoto (indexPath);

    var userActivity = selectedPhoto.OpenDetailUserActivity ();
    var itemProvider = new NSItemProvider (UIImage.FromFile (selectedPhoto.Name));
    itemProvider.RegisterObject (userActivity, NSItemProviderRepresentationVisibility.All);

    var dragItem = new UIDragItem (itemProvider) {
        LocalObject = selectedPhoto
    };

    return new [] { dragItem };
}
```

위의 코드에서 `selectedPhoto` 모델 개체에는 호출 된를 반환 하는 메서드가 `NSUserActivity` 있습니다 `OpenDetailUserActivity()` . 끌기 제스처가 완료 되 면을 통해를 사용 하 여를 `UIDragItem` 표시 합니다 `userActivity` `NSItemProvider` .

**명시적 작업** -단추나 링크의 사용자 제스처는 새 창을 열 수 있는 기능을 제공 합니다.

에서를 `UIApplication` 호출 하 여 새를 시작할 수 있습니다 `UISceneSession` `RequestSceneSessionActivation` . 기존 장면이 이미 있는 경우이를 사용 해야 합니다. 기본적으로 새 장면이 생성 됩니다.

```csharp
public void ItemSelected(UICollectionView collectionView, NSIndexPath indexPath)
{
    var userActivity = selectedPhoto.OpenDetailUserActivity ();

    UIApplication.SharedApplication.RequestSceneSessionActivation(
        sceneSession: null,
        userActivity: userActivity,
        options: null,
        errorHandler: null
    );
}
```

이 예제에서는 `userActivity` `RequestSceneSessionActivation` 명시적 사용자 작업을 기반으로 응용 프로그램의 새 창을 열기 위해 메서드에 전달 되는 유일한 값입니다 .이 경우 `ItemSelected` 의 처리기입니다 `UICollectionView` .

## <a name="related-links"></a>관련 링크

- [Xamarin.ios에서 끌어서 놓기][0]

[0]: ~/ios/platform/introduction-to-ios11/drag-and-drop.md
