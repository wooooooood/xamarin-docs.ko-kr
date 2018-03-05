---
title: "무선 배포"
description: "이 미리 보기 기능을 사용하면 네트워크 연결을 통해 iOS 또는 Apple TV 장치에 배포할 수 있습니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 5AB4C5A9-4FBB-4DCB-BD72-0022D5439E65
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 02/09/2018
ms.openlocfilehash: 11961a21a7c4188c505c822a35531036fd953405
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="wireless-deployment"></a>무선 배포

_이 미리 보기 기능을 사용하면 네트워크 연결을 통해 iOS 또는 Apple TV 장치에 배포할 수 있습니다._

![미리 보기 릴리스](~/media/shared/preview.png)

개발자 워크플로에서 중요한 부분은 장치에 배포하는 것입니다. Xcode 9은 앱을 배포하고 디버그할 때마다 장치를 유선으로 연결하지 않고 네트워크를 통해 iOS 장치 또는 Apple TV에 배포하는 옵션을 도입했습니다. 이 기능은 Mac용 Visual Studio 및 Visual Studio 15.6 릴리스에 도입되었으며, 현재는 미리 보기 상태입니다.

이 가이드에서는 네트워크를 통해 페어링하고 장치에 배포하는 방법을 설명합니다.

## <a name="requirements"></a>요구 사항

무선 배포는 Mac용 Visual Studio 및 Visual Studio에서 **미리 보기** 기능으로 제공됩니다.


무선 배포를 사용하려면 다음이 필요합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

- macOS 10.12.4
- Mac용 Visual Studio의 최신 미리 보기 버전 
    - 이 버전을 설치하려면 Mac용 Visual Studio에서 [알파 또는 베타 채널](https://docs.microsoft.com/en-us/visualstudio/mac/update)로 전환합니다.
- Xcode 9.0 이상
- iOS 11.0 또는 tvOS 11.0 이상이 설치된 장치

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

- Visual Studio 최신 [미리 보기 버전](https://www.visualstudio.com/vs/preview/)
- iOS 11.0 또는 tvOS 11.0 이상이 설치된 장치

Mac 빌드 호스트에 다음 구성 요소를 설치해야 합니다.

- macOS 10.12.4
- Mac용 Visual Studio 미리 보기
    - 설치하려면 Mac용 Visual Studio에서 [알파 또는 베타 채널](https://docs.microsoft.com/en-us/visualstudio/mac/update)로 전환합니다.
- Xcode 9.0 이상

-----

## <a name="connecting-a-device"></a>장치에 연결

장치에서 무선으로 배포하고 디버그하려면 iOS 장치 또는 Apple TV를 Mac의 Xcode와 페어링해야 합니다. 페어링한 후에는 Visual Studio의 장치 대상 목록에서 장치를 선택할 수 있습니다. 

다음 페어링 프로세스는 장치마다 한 번만 수행하면 됩니다. Xcode가 연결 설정을 그대로 유지합니다.

<a name="pair" />

### <a name="pairing-an-ios-device-with-xcode"></a>iOS 장치를 Xcode와 페어링

1. Xcode를 열고 **창 > 장치 및 시뮬레이터**로 이동합니다.
2. 번개 케이블을 사용하여 Mac에 iOS 장치를 연결합니다. 장치에서 **이 컴퓨터를 신뢰**를 선택해야 할 수도 있습니다.
3. 해당 장치를 선택한 다음, **네트워크를 통해 연결** 확인란을 선택하여 장치를 페어링합니다. ![네트워크 옵션을 통한 연결을 보여주는 장치 및 시뮬레이터 창](wireless-deployment-images/image2.png)

### <a name="pairing-an-apple-tv-with-xcode"></a>Apple TV를 Xcode와 페어링

1. Mac과 Apple TV가 동일한 네트워크에 연결되어 있는지 확인합니다.

2. Xcode를 열고 **창 > 장치 및 시뮬레이터**로 이동합니다.

3. Apple TV에서 **Settings > Remotes and Devices > Remote App and Devices**로 이동합니다.

4. Xcode의 **Discovered** 영역에서 Apple TV를 선택하고 Apple TV에 표시되는 확인 코드를 입력합니다.

5. **Connect** 단추를 클릭합니다. 성공적으로 페어링되면 Apple TV 옆에 네트워크 연결 아이콘이 나타납니다.

## <a name="deploy-to-a-device"></a>장치에 배포

장치가 무선으로 연결되고 배포에 사용할 준비가 완료되면 마치 장치가 USB를 통해 연결된 것처럼 장치 대상 목록에 나타납니다.

실제 장치에서 테스트하려면 해당 장치를 [프로비전](~/ios/get-started/installation/device-provisioning/index.md)해야 합니다. 장치에 배포하기 전에 이 작업을 수행해야 합니다. 

iOS 또는 tvOS 장치에 배포하려면 다음 단계를 수행합니다.

1. 배포 컴퓨터와 대상 장치가 동일한 무선 네트워크에 있는지 확인합니다. 

2. 대상 장치 목록에서 장치를 선택하고 응용 프로그램을 실행합니다.

2. 장치가 잠겨 있으면 장치의 잠금을 해제하라는 메시지가 표시됩니다. 장치의 잠금이 해지되면 앱이 장치에 배포됩니다.

무선 배포가 완료되면 자동으로 무선 디버깅이 사용되므로 늘 하던 것처럼 이전에 설정한 중단점을 사용하여 디버깅 워크플로를 계속 진행할 수 있습니다.

## <a name="troubleshooting"></a>문제 해결

1. 항상 iOS 장치 또는 Apple TV가 Mac과 동일한 네트워크에 연결되어 있는지 확인합니다.

2. 장치가 Visual Studio에서 보이지 않으면 Xcode의 **장치 및 시뮬레이터** 창을 선택합니다. 

    * Xcode가 해당 장치를 연결된 것으로 표시하지 **않으면** 장치를 다시 [페어링](#pair)합니다.

    * Xcode가 해당 장치를 연결된 것으로 표시하면 Visual Studio와 장치를 다시 시작합니다.

3. 아직 장치를 [프로비전](~/ios/get-started/installation/device-provisioning/index.md)하지 않았으면 지금 합니다.

4. 이 기능과 관련하여 앞에서 설명한 단계로 해결할 수 없는 문제가 있는 경우 [개발자 커뮤니티](https://developercommunity.visualstudio.com/spaces/41/index.html)에 문제를 제출하세요.

## <a name="related-links"></a>관련 링크

- [Xcode를 사용하여 무선 장치 페어링](https://help.apple.com/xcode/mac/9.0/index.html?localePath=en.lproj#/devbc48d1bad)
