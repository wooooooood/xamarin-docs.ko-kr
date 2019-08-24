---
title: Xamarin.iOS 및 tvOS 앱에 대한 무선 배포
description: 이 문서에서는 Mac용 Visual Studio 또는 Visual Studio 2019에서 iOS 디바이스에 Xamarin.iOS 앱을 무선으로 배포하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: 5AB4C5A9-4FBB-4DCB-BD72-0022D5439E65
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.custom: video
ms.date: 01/22/2019
ms.openlocfilehash: 0e7516f030955c9b0f89db6db11b93afd9b358de
ms.sourcegitcommit: 7ccc7a9223cd1d3c42cd03ddfc28050a8ea776c2
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/13/2019
ms.locfileid: "67865499"
---
# <a name="wireless-deployment-for-xamarinios-and-tvos-apps"></a>Xamarin.iOS 및 tvOS 앱에 대한 무선 배포

개발자 워크플로에서 중요한 부분은 디바이스에 배포하는 것입니다. Xcode 9은 앱을 배포하고 디버그할 때마다 디바이스를 유선으로 연결하지 않고 네트워크를 통해 iOS 디바이스 또는 Apple TV에 배포하는 옵션을 도입했습니다. 이 기능은 Mac용 Visual Studio 7.4 및 Visual Studio 15.6 릴리스에 도입되었습니다.

이 가이드에서는 네트워크를 통해 페어링하고 디바이스에 배포하는 방법을 설명합니다.

## <a name="requirements"></a>요구 사항

무선 배포는 Mac용 Visual Studio 및 Visual Studio에서 기능으로 제공됩니다.

무선 배포를 사용하려면 다음이 필요합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Mac용 Visual Studio](#tab/macos)

- macOS 10.12.4
- 최신 버전의 Mac용 Visual Studio
- Xcode 9.0 이상
- iOS 11.0 또는 tvOS 11.0 이상이 설치된 디바이스

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

- 최신 버전의 Visual Studio
- iOS 11.0 또는 tvOS 11.0 이상이 설치된 디바이스

Mac 빌드 호스트에 다음 구성 요소를 설치해야 합니다.

- macOS 10.12.4
- Mac용 Visual Studio
- Xcode 9.0 이상

-----

## <a name="connecting-a-device"></a>디바이스에 연결

디바이스에서 무선으로 배포하고 디버그하려면 iOS 디바이스 또는 Apple TV를 Mac의 Xcode와 페어링해야 합니다. 페어링한 후에는 Visual Studio의 디바이스 대상 목록에서 디바이스를 선택할 수 있습니다. 

다음 페어링 프로세스는 디바이스마다 한 번만 수행하면 됩니다. Xcode가 연결 설정을 그대로 유지합니다.

<a name="pair" />

### <a name="pairing-an-ios-device-with-xcode"></a>iOS 디바이스를 Xcode와 페어링

1. Xcode를 열고 **창 &gt; 디바이스 및 시뮬레이터**로 이동합니다.
2. 번개 케이블을 사용하여 Mac에 iOS 디바이스를 연결합니다. 디바이스에서 **이 컴퓨터를 신뢰**를 선택해야 할 수도 있습니다.
3. 디바이스를 선택한 다음 **Connect via network(네트워크를 통해 연결)** 확인란을 선택하여 디바이스를 페어링하세요.  ![네트워크를 통해 연결 옵션이 표시된 디바이스 및 시뮬레이터 창](wireless-deployment-images/image2.png)

### <a name="pairing-an-apple-tv-with-xcode"></a>Apple TV를 Xcode와 페어링

1. Mac과 Apple TV가 동일한 네트워크에 연결되어 있는지 확인합니다.

2. Xcode를 열고 **창 &gt; 디바이스 및 시뮬레이터**로 이동합니다.

3. Apple TV에서 **Settings > Remotes and Devices > Remote App and Devices**로 이동합니다.

4. Xcode의 **Discovered** 영역에서 Apple TV를 선택하고 Apple TV에 표시되는 확인 코드를 입력합니다.

5. **Connect** 단추를 클릭합니다. 성공적으로 페어링되면 Apple TV 옆에 네트워크 연결 아이콘이 나타납니다.

## <a name="deploy-to-a-device"></a>디바이스에 배포

디바이스가 무선으로 연결되고 배포에 사용할 준비가 완료되면 마치 디바이스가 USB를 통해 연결된 것처럼 디바이스 대상 목록에 나타납니다.

실제 디바이스에서 테스트하려면 해당 디바이스를 [프로비전](~/ios/get-started/installation/device-provisioning/index.md)해야 합니다. 디바이스에 배포하기 전에 이 작업을 수행해야 합니다. 

iOS 또는 tvOS 디바이스에 배포하려면 다음 단계를 수행합니다.

1. 배포 컴퓨터와 대상 디바이스가 동일한 무선 네트워크에 있는지 확인합니다. 

2. 대상 디바이스 목록에서 디바이스를 선택하고 애플리케이션을 실행합니다.

3. 디바이스가 잠겨 있으면 디바이스의 잠금을 해제하라는 메시지가 표시됩니다. 디바이스의 잠금이 해지되면 앱이 디바이스에 배포됩니다.

무선 배포가 완료되면 자동으로 무선 디버깅이 사용되므로 늘 하던 것처럼 이전에 설정한 중단점을 사용하여 디버깅 워크플로를 계속 진행할 수 있습니다.

## <a name="troubleshooting"></a>문제 해결

1. 항상 iOS 디바이스 또는 Apple TV가 Mac과 동일한 네트워크에 연결되어 있는지 확인합니다.

2. 디바이스가 Visual Studio에서 보이지 않으면 Xcode의 **디바이스 및 시뮬레이터** 창을 선택합니다. 

    * Xcode가 해당 디바이스를 연결된 것으로 표시하지 **않으면** 디바이스를 다시 [페어링](#pair)합니다.

    * Xcode가 해당 디바이스를 연결된 것으로 표시하면 Visual Studio와 디바이스를 다시 시작합니다.

3. 아직 디바이스를 [프로비전](~/ios/get-started/installation/device-provisioning/index.md)하지 않았으면 지금 합니다.

4. 이 기능과 관련하여 앞에서 설명한 단계로 해결할 수 없는 문제가 있는 경우 [개발자 커뮤니티](https://developercommunity.visualstudio.com/spaces/41/index.html)에 문제를 제출하세요.

## <a name="related-links"></a>관련 링크

- [Xcode를 사용하여 무선 디바이스 페어링](https://help.apple.com/xcode/mac/9.0/index.html?localePath=en.lproj#/devbc48d1bad)

## <a name="related-video"></a>관련 동영상

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Debug-to-iOS-Devices-Over-Wi-Fi/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
