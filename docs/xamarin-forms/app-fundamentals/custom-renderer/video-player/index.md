---
title: "비디오 플레이어를 구현합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: CE9E955D-A9AC-4019-A5D7-6390D80DECA1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: e818bc3fa9793f093c10ac2617c5a822d08213d4
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="implementing-a-video-player"></a>비디오 플레이어를 구현합니다.

경우에 따라 Xamarin.Forms 응용 프로그램에서 비디오 파일 재생 하는 것이 좋습니다. 이러한 일련의 문서에서는 iOS, Android, 및의 Windows 플랫폼 UWP (유니버설) 라는 Xamarin.Forms 클래스에 대 한 사용자 지정 렌더러를 작성 하는 방법에 설명 `VideoPlayer`합니다.

에 [ **VideoPlayerDemos** ](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/) 샘플을 구현 및 지원 되는 모든 파일 `VideoPlayer` 명명 된 폴더에 있는 `FormsVideoLibrary` 지정 된 네임 스페이스를 식별 하 고 `FormsVideoLibrary` 또는 네임 스페이스 시작 하는 `FormsVideoLibrary`합니다. 이 조직 이름을 지정 해야 하기 쉽도록 비디오 플레이어 파일 고유한 Xamarin.Forms 솔루션을에 복사 하 고 있습니다.

`VideoPlayer` 세 가지 유형의 소스에서 비디오 파일을 재생할 수 있습니다.

- URL을 사용 하 여 인터넷
- 플랫폼 응용 프로그램에 리소스 포함
- 장치의 비디오 라이브러리

비디오 플레이어 필요 *전송 컨트롤*, 재생 및 비디오를 일시 중지에 대 한 개가 있는 있는데 비디오 통해 진행률을 표시 하는 바는 위치 지정 및 신속 하 게 다른 위치로 이동할 수 있습니다. `VideoPlayer` 전송 컨트롤을 사용 하 고 플랫폼 (아래와 같이) 또는 사용자가 제공 하는 위치 지정 모음 사용자 지정 전송 컨트롤 및 위치 지정 막대를 제공할 수 있습니다. 다음은 iOS, Android 및 유니버설 Windows 플랫폼에서 실행 중인 프로그램입니다.

[![웹 비디오 재생](web-videos-images/playwebvideo-small.png "웹 비디오 재생")](web-videos-images/playwebvideo-large.png "웹 비디오 재생")

물론, 크게 보려면 옆으로 전화를 설정할 수 있습니다.

보다 복잡 한 비디오 플레이어는 볼륨 제어, 전화 통화를 통해 전달 비디오를 중단 하는 메커니즘 및 화면을 재생 하는 동안 활성 상태로 유지 하는 방법을 같은 몇 가지 추가 기능을 것입니다.

다음 일련의 문서 점진적으로 플랫폼 렌더러 및 지원 클래스 작성 방법을 보여 줍니다.

## <a name="creating-the-platform-video-playersplayer-creationmd"></a>[플랫폼 비디오 플레이어를 만들기](player-creation.md)

각 플랫폼 필요는 `VideoPlayerRenderer` 만들어 플랫폼에서 지원 되는 비디오 플레이어 컨트롤을 유지 하는 클래스입니다. 이 문서는 클래스 및 선수 만들어지는 방식 렌더러의 구조를 보여줍니다.

## <a name="playing-a-web-videoweb-videosmd"></a>[웹 비디오를 재생](web-videos.md)

아마도 비디오 플레이어를 위한 비디오의 가장 일반적인 소스는 인터넷. 이 문서에는 웹 비디오 수 참조 및 비디오 플레이어에 대 한 원본으로 사용 방법을 설명 합니다.

## <a name="binding-video-sources-to-the-playersource-bindingsmd"></a>[플레이어에 게 비디오 소스 바인딩](source-bindings.md)

이 문서에서는 `ListView` 를 재생 하는 비디오의 컬렉션을 표시 합니다. 한 프로그램 코드 숨김 파일에서 비디오 플레이어의 비디오 원본을 설정 하는 방법을 보여 줍니다. 하지만 두 번째 프로그램 데이터 간의 바인딩을 사용 하는 방법을 보여 줍니다.는 `ListView` 및 비디오 플레이어입니다.

## <a name="loading-application-resource-videosloading-resourcesmd"></a>[응용 프로그램 리소스 비디오를 로드합니다.](loading-resources.md)

비디오는 플랫폼 프로젝트에 리소스로 포함 될 수 있습니다. 이 문서에는 해당 리소스를 저장 하 고 나중에 비디오 플레이어에서 재생할 수 프로그램에 로드 하는 방법을 보여 줍니다.

## <a name="accessing-the-devices-video-libraryaccessing-librarymd"></a>[장치의 비디오 라이브러리 액세스](accessing-library.md)

장치 카메라를 사용 하 여 비디오 만들어지면 비디오 파일 장치의 이미지 라이브러리에 저장 됩니다. 이 문서에 액세스 하려면 비디오를 선택한 후 비디오 플레이어를 사용 하 여 재생 장치의 이미지 선택 하는 방법을 보여 줍니다.

## <a name="custom-video-transport-controlscustom-transportmd"></a>[사용자 지정 비디오 전송 컨트롤](custom-transport.md)

각 플랫폼에서 비디오 플레이어 자체 전송 컨트롤에 대 한 단추가의 형태로 제공 하지만 **재생** 및 **일시 중지**, 이러한 단추를 표시 하지 않으려면 하 고 직접 제공할 수 있습니다. 이 문서 방법을 보여 줍니다.

## <a name="custom-video-positioningcustom-positioningmd"></a>[사용자 지정 비디오 위치 지정](custom-positioning.md)

각 플랫폼 비디오 플레이어에 위치 막대를 비디오의 진행률을 표시 하 고 계속 또는 다시 특정 위치로 건너뛸 수 있습니다. 이 문서에서는 사용자 지정 컨트롤을 위치 창을 바꿀 수 있습니다.





## <a name="related-links"></a>관련 링크

- [비디오 플레이어 데모 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
