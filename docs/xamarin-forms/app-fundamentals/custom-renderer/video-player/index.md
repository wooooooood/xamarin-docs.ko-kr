---
title: ''
description: 이 문서에서는 Xamarin.Forms를 사용하여 비디오 플레이어 애플리케이션을 구현하는 방법을 설명합니다.
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 08bfb86f040bfbce834df5a5d98231afae92e78d
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84133770"
---
# <a name="implementing-a-video-player"></a>비디오 플레이어 구현

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-videoplayerdemos)

Xamarin.Forms 애플리케이션에서 비디오 파일을 재생하는 것이 좋은 경우가 있습니다. 이 문서 시리즈에서는 `VideoPlayer`라는 이름의 Xamarin.Forms 클래스에 대해 iOS, Android 및 UWP(유니버설 Windows 플랫폼)용 사용자 지정 렌더러를 작성하는 방법을 설명합니다.

[**VideoPlayerDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-videoplayerdemos) 샘플에서 `VideoPlayer`를 구현하고 지원하는 모든 파일은 `FormsVideoLibrary` 폴더에 있으며 네임스페이스 `FormsVideoLibrary` 또는 `FormsVideoLibrary`로 시작되는 네임스페이스로 식별됩니다. 이러한 조직 및 명명을 통해 비디오 플레이어 파일을 사용자의 Xamarin.Forms 솔루션으로 쉽게 복사할 수 있습니다.

`VideoPlayer`는 세 가지 소스의 비디오 파일을 재생할 수 있습니다.

- URL을 사용한 인터넷
- 플랫폼 애플리케이션에 포함된 리소스
- 디바이스의 비디오 라이브러리

비디오 플레이어에는 *전송 컨트롤*이 필요합니다. 이것은 비디오를 재생 및 일시 중지하는 단추이고 비디오의 진행률을 표시하는 위치 지정 막대입니다. 사용자는 이것을 사용하여 다른 위치로 신속하게 건너뛸 수 있습니다. `VideoPlayer`에 플랫폼에서 제공되는 전송 컨트롤 및 위치 지정 막대를 사용하거나(아래 참조), 사용자 지정 전송 컨트롤과 위치 지정 막대를 사용자가 제공할 수 있습니다. 다음은 iOS, Android 및 유니버설 Windows 플랫폼에서 실행되는 프로그램입니다.

[![웹 비디오 재생](web-videos-images/playwebvideo-small.png "웹 비디오 재생")](web-videos-images/playwebvideo-large.png#lightbox "웹 비디오 재생")

물론 휴대폰을 옆으로 돌려서 더 크게 볼 수 있습니다.

보다 정교한 비디오 플레이에는 볼륨 제어, 전화가 걸려올 때 비디오를 중단하는 메커니즘 및 재생하는 동안 화면을 활성 상태로 유지하는 방법과 같은 추가적인 기능이 포함될 수 있습니다.

다음 문서 시리즈에서는 플랫폼 렌더러와 지원 클래스가 구축되는 방법을 점진적으로 보여줍니다.

## <a name="creating-the-platform-video-players"></a>[플랫폼 비디오 플레이어 만들기](player-creation.md)

각 플랫폼에는 플랫폼에서 지원되는 비디오 플레이어를 작성 및 유지 관리하는 `VideoPlayerRenderer` 클래스가 필요합니다. 이 문서에서는 렌더러 클래스의 구조와 플레이어가 작성되는 방법을 보여줍니다.

## <a name="playing-a-web-video"></a>[웹 비디오 재생](web-videos.md)

십중팔구 비디오 플레이어에 대한 가장 일반적인 비디오 소스는 인터넷입니다. 이 문서에서는 웹 비디오가 참조되고 비디오 플레이어의 소스로 사용되는 방법을 설명합니다.

## <a name="binding-video-sources-to-the-player"></a>[플레이어에 비디오 소스 바인딩](source-bindings.md)

이 문서에서는 `ListView`를 사용하여 재생할 비디오 컬렉션을 표시합니다. 한 프로그램에 코드 숨김 파일이 비디오 플레이어의 비디오 소스를 설정하는 방법이 표시되지만 두 번째 프로그램에는 `ListView`와 비디오 플레이어 간에 데이터 바인딩을 사용하는 방법이 표시됩니다.

## <a name="loading-application-resource-videos"></a>[애플리케이션 리소스 비디오 로드](loading-resources.md)

비디오는 플랫폼 프로젝트의 리소스로 포함될 수 있습니다. 이 문서에는 리소스를 저장했다가 나중에 프로그램으로 로드하여 비디오 플레이어에서 재생하는 방법을 보여줍니다.

## <a name="accessing-the-devices-video-library"></a>[디바이스의 비디오 라이브러리에 액세스](accessing-library.md)

디바이스의 카메라를 사용하여 비디오가 생성되면 디바이스의 이미지 라이브러리에 비디오 파일이 저장됩니다. 이 문서에서는 디바이스의 이미지 선택기에 액세스하여 비디오를 선택한 다음, 비디오 플레이어를 사용하여 재생하는 방법을 보여줍니다.

## <a name="custom-video-transport-controls"></a>[사용자 지정 비디오 전송 컨트롤](custom-transport.md)

각 플랫폼의 비디오 플레이어에 **재생** 및 **일시 중지**를 위한 단추의 형태로 자체 전송 제어가 제공되지만 이러한 단추를 표시하지 않고 자체적인 제어를 제공할 수 있습니다. 이 문서에서 그 방법을 보여줍니다.

## <a name="custom-video-positioning"></a>[사용자 지정 비디오 위치 지정](custom-positioning.md)

플랫폼 비디오 플레이어마다 비디오의 진행률을 나타내는 위치 지정 막대가 있으며 이것을 사용하여 앞쪽이나 뒤쪽의 특정 위치로 건너뛸 수 있습니다. 이 문서에서는 이 위치 지정 막대를 사용자 지정 컨트롤로 바꾸는 방법을 보여줍니다.

## <a name="related-links"></a>관련 링크

- [비디오 플레이어 데모(샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-videoplayerdemos)
