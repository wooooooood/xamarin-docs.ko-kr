---
title: Xamarin.iOS에 HomeKit
description: HomeKit은 홈 자동화 장치 제어에 대 한 Apple의 프레임 워크. 이 문서는 HomeKit를 소개 하 고 이러한 액세서리와 상호 작용할 수 HomeKit 액세서리 시뮬레이터 및 간단한 Xamarin.iOS 앱 작성에 테스트 액세서리 구성에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: 90C0C553-916B-46B1-AD52-1E7332792283
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: 0dfc6e9ba5098df66a72292d6c8b89ea1bbd1f97
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787463"
---
# <a name="homekit-in-xamarinios"></a>Xamarin.iOS에 HomeKit

_HomeKit은 홈 자동화 장치 제어에 대 한 Apple의 프레임 워크. 이 문서는 HomeKit를 소개 하 고 이러한 액세서리와 상호 작용할 수 HomeKit 액세서리 시뮬레이터 및 간단한 Xamarin.iOS 앱 작성에 테스트 액세서리 구성에 대해 설명 합니다._

[![](homekit-images/accessory01.png "HomeKit 예제 응용 프로그램을 사용 하도록 설정")](homekit-images/accessory01.png#lightbox)

Apple는 iOS 8에서에서 원활 하 게 하나의 단위로 다양 한 공급 업체의 여러 가정 자동화 장치를 통합 하는 방법으로 HomeKit를 도입 되었습니다. 검색 하기 위한 일반적인 프로토콜 올려서 구성 하 고 홈 자동화 장치 제어 HomeKit 있습니다 장치를 함께 작동 하 여 개별 공급 업체를 조정 하기 위한 필요 없이 모두 관련 되지 않은 공급 업체에서.

HomeKit, 응용 프로그램이 나 Api에 제공 된 공급 업체를 사용 하 여 없이 모든 HomeKit 사용 장치를 제어 하는 Xamarin.iOS 앱을 만들 수 있습니다. HomeKit를 사용 하면 다음을 수행할 수 있습니다.:

- 새 사용 HomeKit 가정 자동화 장치 검색을 모든 사용자의 iOS 장치에서 지속 되는 데이터베이스에 추가 합니다.
- 설치, 구성, 표시 및 모든 장치는 HomeKit에서 제어할 _구성 데이터베이스 홈_합니다.
- 미리 구성 된 모든 HomeKit 장치와 통신 하 고 설정 된 모든 주방에서 광원 등의 공동에서 작업 또는 개별 작업을 수행 하도록 명령 합니다.

홈 구성 데이터베이스를 사용 하도록 설정 하는 HomeKit 앱에서 장치를 처리 하는 것 외에도 HomeKit Siri 음성 명령에 대 한 액세스를 제공 합니다. 적절 하 게 구성 된 HomeKit 설치 프로그램을 지정 하면 명령을 실행할 수 음성와 같은 "Siri, 끊임없이 변화 공간에서 조명 설정 합니다."

<a name="Home-Configuration-Database" />

## <a name="the-home-configuration-database"></a>홈 구성 데이터베이스

HomeKit 지정된 된 위치를 홈 컬렉션에 있는 모든 자동화 장치를 구성합니다. 이 컬렉션에 있는 사용자가 홈 자동화 장치 레이블이 의미 있는, 사람이 읽을 수 있는 논리적으로 정렬 된 단위를 그룹화 할 방법을 제공 합니다.

홈 컬렉션 홈 구성 데이터베이스를 자동으로 됩니다 백업 및 동기화 된 모든 사용자의 iOS 장치에 저장 됩니다. HomeKit 홈 구성 데이터베이스를 사용 하기 위한 다음과 같은 클래스를 제공 합니다.

- `HMHome` -이 모든 정보 및 모든 가정 자동화 장치에 대 한 구성을 않음 (예: 단일 물리적 위치에 포함 되는 최상위 컨테이너 하나의 제품군 거주지). 사용자의 주 가정용 등 휴가 집 둘 이상의 거주지가 있을 수 있습니다. 또는 다른 "보관"는 동일한 속성에 주 집 등의 차고를 통해 게스트 하우스를 가질 수 있습니다. 어느 방법을 하나 이상 `HMHome` 개체 _해야_ 설치 되 고 다른 HomeKit 정보를 입력할 수 있는 전에 저장 합니다.
- `HMRoom` -While 선택적는 `HMRoom` 홈 내의 특정 대화방을 정의할 수 있습니다 (`HMHome`)와 같은: 주방, 욕실, 차고 또는 가정용 합니다. 사용자의 홈 자동화 장치에 자신의 집에서 특정 위치에 있는 모든 그룹화 수는 `HMRoom` 를 하나의 단위로 그에 따라 작업을 수행 합니다. 예를 들어 Siri 차고 lights 해제를 요청 합니다.
- `HMAccessory` -이 개인을 나타냅니다으로 실제 HomeKit 사용자의 거주지 (예: 스마트 자동 온도 조절기)에 설치 된 장치를 자동화 합니다. 각 `HMAccessory` 에 할당 되는 `HMRoom`합니다. 사용자 모든 대화방을 구성 하지 않은 경우 HomeKit accessories 한 특수 기본 대화방에 할당 합니다.
- `HMService` -제공 하는 서비스를 나타내는 주어진 `HMAccessory`, 광원 또는 (색 변경도 지원 됨) 하는 경우 색의 설정/해제 상태 등입니다. 각 `HMAccessory` 광원을 포함 하는 차고 도어 열기와 같은 하나 이상의 서비스를 가질 수 있습니다. 또한는 주어진 `HMAccessory` 서비스 펌웨어 업데이트와 같은 사용자 정의 컨트롤 외부에 있을 수 있습니다.
- `HMZone` -사용자의 컬렉션을 그룹화 할 수 있도록 허용 `HMRoom` Upstairs, Downstairs 또는 지하실 같은 논리 영역으로 개체입니다. 선택적 동안 이렇게 하면 Siri 묻는 같은 상호 작용에 대 한 오프 downstairs 광원의 모든 설정.

<a name="Provisioning-a-HomeKit-App" />

## <a name="provisioning-a-homekit-app"></a>HomeKit 응용 프로그램을 프로 비전

HomeKit에서 설정한 보안 요구 사항으로 인해 HomeKit 프레임 워크를 사용 하는 Xamarin.iOS 앱 올바로 구성 되어야 함 Apple 개발자 포털 및 Xamarin.iOS 프로젝트 파일입니다.

다음을 수행합니다.

1. 에 로그인 된 [Apple 개발자 포털](http://developer.apple.com)합니다.
2. 클릭 **인증서, 식별자 및 프로필**합니다.
3. 그렇게 이미 하지 않은 경우 클릭 **식별자** 응용 프로그램에 대 한 ID를 만듭니다 (예: `com.company.appname`), 프로그램 기존 id입니다. 그렇지 않으면 편집
4. 확인 된 **HomeKit** 서비스가 지정된 된 ID에 대 한 검사 된: 

    [![](homekit-images/provision01.png "지정된 된 ID에 대 한 HomeKit 서비스를 사용 하도록 설정")](homekit-images/provision01.png#lightbox)
5. 변경 내용을 저장합니다.
4. 클릭 **프로 비전 프로필** > **개발** 프로비저닝 프로필 응용 프로그램에 대 한 새로운 개발을 만듭니다. 

    [![](homekit-images/provision02.png "프로비저닝 프로필에서 앱에 대 한 새로운 개발 만들기")](homekit-images/provision02.png#lightbox)
5. 다운로드 및 새 프로비저닝 프로필을 설치 하거나 사용 Xcode를 다운로드 하 여 프로필을 설치 하십시오.
6. Xamarin.iOS 프로젝트 옵션을 편집 하 고 방금 만든 프로 비전 프로필을 사용 하 고 있는지 확인 하십시오. 

    [![](homekit-images/provision03.png "방금 만든 프로비저닝 프로필을 선택 합니다.")](homekit-images/provision03.png#lightbox)
7. 다음에 편집 프로그램 **Info.plist** 파일을 프로 비전 프로필을 만드는 데 사용 하는 응용 프로그램 ID를 사용 하 고 있는지 확인 하십시오. 

    [![](homekit-images/provision04.png "앱 ID를 설정 합니다. ")](homekit-images/provision04.png#lightbox)
8. 마지막으로 편집 프로그램 **Entitlements.plist** 파일을 확인 하는 **HomeKit** 권한 선택: 

    [![](homekit-images/provision05.png "HomeKit 자격을 사용 하도록 설정")](homekit-images/provision05.png#lightbox)
9. 모든 파일에 변경 내용을 저장 합니다.

현재 위치를이 설정 하 여 응용 프로그램 HomeKit 프레임 워크 Api에 액세스할 준비가 되었습니다. 에 대 한 자세한 내용은 프로 비전을 참조 하십시오 우리의 [장치 프로 비전](~/ios/get-started/installation/device-provisioning/index.md) 및 [앱을 프로 비전](~/ios/get-started/installation/device-provisioning/index.md) 가이드입니다.

> [!IMPORTANT]
> 테스트 설정 HomeKit 앱 개발을 위한 제대로 프로 비전 하는 실제 iOS 장치가 필요 합니다. HomeKit은 iOS 시뮬레이터에서에서 테스트할 수 없습니다.

## <a name="the-homekit-accessory-simulator"></a>HomeKit 액세서리 시뮬레이터

물리적 장치를 사용 하지 않고도 모든 가능한 가정 자동화 장치 및 서비스를 테스트 하는 방법을 제공 하기 위해 Apple 만듭니다는 _HomeKit 액세서리 시뮬레이터_합니다. 이 시뮬레이터를 사용 하 여 설치 하 고 가상 HomeKit 장치를 구성할 수 있습니다.

### <a name="installing-the-simulator"></a>시뮬레이터를 설치합니다.

계속 하기 전에 설치 해야 하므로 Apple HomeKit 액세서리 시뮬레이터 별도 다운로드로, Xcode에서 제공 합니다.

다음을 수행합니다.

1. 웹 브라우저에서 방문 [Apple 개발자를 위한 다운로드](https://developer.apple.com/download/more/?name=for%20Xcode)
2. 다운로드는 **Xcode xxx 위한 도구가 추가로** (xxx는 사용자가 설치한 Xcode의 버전): 

    [![](homekit-images/simulator01.png "Xcode에 대 한 추가 도구 다운로드")](homekit-images/simulator01.png#lightbox)
3. 디스크 이미지를 열고의 도구를 설치 하면 **응용 프로그램** 디렉터리입니다.

설치 HomeKit 액세서리 시뮬레이터와 테스트를 위해 가상 보조 프로그램을 만들 수 있습니다.

### <a name="creating-virtual-accessories"></a>가상 보조 프로그램 만들기

HomeKit 액세서리 시뮬레이터를 시작 하 고 몇 가지 가상 보조 프로그램 만들기를 하려면 다음을 수행 합니다.

1. 응용 프로그램 폴더에서 HomeKit 액세서리 시뮬레이터를 시작 합니다. 

    [![](homekit-images/simulator02.png "HomeKit 액세서리 시뮬레이터")](homekit-images/simulator02.png#lightbox)
2. 클릭는 **+** 선택한 단추 **새 액세서리 중...** : 

    [![](homekit-images/simulator03.png "새 보조를 추가 합니다.")](homekit-images/simulator03.png#lightbox)
3. 새 접근자에 대 한 정보를 입력 하 고 클릭 하 고 **마침** 단추: 

    [![](homekit-images/simulator04.png "새 보조에 대 한 정보 입력")](homekit-images/simulator04.png#lightbox)
4. 클릭는 **서비스 추가...** 단추를 하 고 드롭다운 목록에서 서비스 종류를 선택 합니다. 

    [![](homekit-images/simulator05.png "드롭다운 목록에서 서비스 유형 선택")](homekit-images/simulator05.png#lightbox)
5. 제공 된 **이름** 서비스와 클릭에 대 한는 **마침** 단추: 

    [![](homekit-images/simulator06.png "서비스에 대 한 이름을 입력 합니다.")](homekit-images/simulator06.png#lightbox)
6. 클릭 하 여 서비스에 대 한 선택적 특성을 제공할 수 있습니다는 **추가 특성** 단추 및 필요한 설정을 구성 합니다. 

    [![](homekit-images/simulator07.png "필요한 설정 구성")](homekit-images/simulator07.png#lightbox)
7. 각 유형의 가상 가정 자동화 HomeKit 지 원하는 장치를 새로 만들려면 위의 단계를 반복 합니다.

일부 샘플 가상 HomeKit 차례로 만들고 구성한 지금 사용 하 고 사용할 수 Xamarin.iOS 앱에서 이러한 장치를 제어 합니다.

## <a name="configuring-the-infoplist-file"></a>Info.plist 파일 구성

개발자를 추가 해야 합니다 iOS 10에 대 한 새 (이상)는 `NSHomeKitUsageDescription` 응용 프로그램의 키 `Info.plist` 파일 및 응용 프로그램 사용자의 HomeKit 데이터베이스에 액세스 하려는 이유 선언 문자열을 제공 합니다. 이 문자열은 응용 프로그램을 실행 하는 사용자는 첫 번째 시간에 나타납니다.

[![](homekit-images/info01.png "HomeKit 권한 대화 상자")](homekit-images/info01.png#lightbox)

이 키를 설정 하려면 다음을 수행 합니다.

1. 두 번 클릭는 `Info.plist` 파일에 **솔루션 탐색기** 를 편집 하기 위해 엽니다.
2. 화면 맨 아래에 전환 하는 **소스** 보기.
3. 새로 추가 **항목** 목록에 있습니다.
4. 드롭다운 목록에서 선택 **개인정보 취급 방침-HomeKit 사용 설명을**: 

    [![](homekit-images/info02.png "개인정보 취급 방침-HomeKit 사용 설명을 선택합니다")](homekit-images/info02.png#lightbox)
5. 응용 프로그램 사용자의 HomeKit 데이터베이스에 액세스 하려는 이유에 대 한 설명을 입력 합니다. 

    [![](homekit-images/info03.png "설명을 입력합니다")](homekit-images/info03.png#lightbox)
6. 파일의 변경 내용을 저장합니다.

> [!IMPORTANT]
> 설정 실패는 `NSHomeKitUsageDescription` 키에 `Info.plist` 파일은 앱에서 발생 합니다 _자동으로 실패_ (되 런타임 시 시스템에 의해 닫힘) iOS 10 (또는 그 이상)에서 실행할 때 오류 없이 합니다.

## <a name="connecting-to-homekit"></a>HomeKit에 연결

Xamarin.iOS 앱 필요한 HomeKit으로 통신 하려면 먼저의 인스턴스를 인스턴스화할 수는 `HMHomeManager` 클래스입니다. 홈 관리자 HomeKit의 중앙 진입점 이며 사용 가능한 집의 목록을 제공 해야 업데이트 및 해당 목록을 유지 관리 하 고 사용자의 반환 _기본 홈_합니다.

`HMHome` 모든 대화방, 그룹 또는 포함 될 수, 함께 설치 된 모든 가정 자동화 액세서리는 영역을 포함 하 여 제공 홈에 대 한 모든 정보를 포함 하는 개체입니다. HomeKit 하나 이상에서 모든 작업을 수행 하려면 먼저 `HMHome` 만들어지고 기본 홈으로 지정 해야 합니다.

앱은 기본 홈 존재 하는지 확인 하 고 만들고 그렇지 않은 경우 하나를 할당 하는 일을 담당 합니다.

### <a name="adding-a-home-manager"></a>홈 관리자 추가

HomeKit 인식 Xamarin.iOS 앱을 추가 하려면 편집는 **AppDelegate.cs** 파일을 편집 하 고 다음과 비슷하게 표시:

```csharp
using HomeKit;
...

public HMHomeManager HomeManager { get; set; }
...

public override void FinishedLaunching (UIApplication application)
{
    // Attach to the Home Manager
    HomeManager = new HMHomeManager ();
    Console.WriteLine ("{0} Home(s) defined in the Home Manager", HomeManager.Homes.Count());

    // Wire-up Home Manager Events
    HomeManager.DidAddHome += (sender, e) => {
        Console.WriteLine("Manager Added Home: {0}",e.Home);
    };

    HomeManager.DidRemoveHome += (sender, e) => {
        Console.WriteLine("Manager Removed Home: {0}",e.Home);
    };
    HomeManager.DidUpdateHomes += (sender, e) => {
        Console.WriteLine("Manager Updated Homes");
    };
    HomeManager.DidUpdatePrimaryHome += (sender, e) => {
        Console.WriteLine("Manager Updated Primary Home");
    };
}
```

응용 프로그램을 처음 실행할 때 사용자가 묻습니다 자신의 HomeKit 정보에 액세스할 수 있도록 하려는 경우:

[![](homekit-images/home01.png "원하는 경우 자신의 HomeKit 정보에 액세스할 수 있도록 허용 하는 사용자는 요청")](homekit-images/home01.png#lightbox)

사용자가 대답 **확인**, 응용 프로그램의 HomeKit Accessories 작업할 수 없습니다는 그렇지 않은 경우 및 HomeKit에 대 한 모든 호출 오류와 함께 실패 합니다.

다음 홈에서 관리자와 위치를 응용 프로그램 기본 홈을 구성한 경우 참조를 그렇지 않은 경우에 사용자를 만들고 할당 하나 있는 방법을 제공 해야 합니다.

### <a name="accessing-the-primary-home"></a>기본 홈에 액세스

위에서 설명한 대로 기본 홈을 생성 하 고 HomeKit를 사용할 수는 방법을 제공 하는 사용자를 만들어 할당 한 경우 기본 홈에 대 한 응용 프로그램의 책임 존재 하지 않는 것이 먼저 구성 해야 합니다.

모니터링할 필요한 응용 프로그램 먼저 시작 하거나 백그라운드에서을 반환 하는 경우는 `DidUpdateHomes` 의 이벤트는 `HMHomeManager` 클래스 기본 홈 있는지 확인할 수 있습니다. 존재 하지 않는 경우를 만드는 사용자에 대 한 인터페이스를 제공 해야 합니다.

다음 코드는 기본 홈에 대 한 확인 하려면 뷰 컨트롤러를 추가할 수 있습니다.

```csharp
using HomeKit;
...

public AppDelegate ThisApp {
    get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
}
...

// Wireup events
ThisApp.HomeManager.DidUpdateHomes += (sender, e) => {

    // Was a primary home found?
    if (ThisApp.HomeManager.PrimaryHome == null) {
        // Ask user to add a home
        PerformSegue("AddHomeSegue",this);
    }
};
```

홈 관리자가 HomeKit에 대 한 연결을 수행 하는 경우는 `DidUpdateHomes` 이벤트가 발생 합니다, 모든 기존 가정 이나 집의 관리자의 컬렉션에 로드 되 고 기본 홈 사용 가능한 경우 로드할 수 있습니다.

### <a name="adding-a-primary-home"></a>기본 홈 추가

경우는 `PrimaryHome` 의 속성은 `HMHomeManager` 은 `null` 후는 `DidUpdateHomes` 사용자를 만들고 기본 홈 계속 하기 전에 할당 하는 방법을 제공 해야 하는 이벤트입니다.

일반적으로 응용 프로그램에는 사용자가 홈 기본 홈으로 설치 관리자에 전달 되는 새 홈에 대 한 양식을 제공 합니다. 에 대 한는 **HomeKitIntro** 샘플 응용 프로그램을 모달 뷰 IOS 디자이너에서 생성 되어에 의해 호출 된 `AddHomeSegue` 응용 프로그램의 기본 인터페이스에서 segue 합니다.

새 홈 및 추가 가정 하는 단추에 대 한 이름을 입력 하 고 사용자에 대 한 텍스트 필드를 제공 합니다. 사용자가 누를 때는 **추가 홈** 단추, 다음 코드 추가 가정 하 홈 관리자를 호출 합니다.

```csharp
// Add new home to HomeKit
ThisApp.HomeManager.AddHome(HomeName.Text,(home,error) =>{
    // Did an error occur
    if (error!=null) {
        // Yes, inform user
        AlertView.PresentOKAlert("Add Home Error",string.Format("Error adding {0}: {1}",HomeName.Text,error.LocalizedDescription),this);
        return;
    }

    // Make the primary house
    ThisApp.HomeManager.UpdatePrimaryHome(home,(err) => {
        // Error?
        if (err!=null) {
            // Inform user of error
            AlertView.PresentOKAlert("Add Home Error",string.Format("Unable to make this the primary home: {0}",err.LocalizedDescription),this);
            return ;
        }
    });

    // Close the window when the home is created
    DismissViewController(true,null);
});
```

`AddHome` 메서드는 새 홈을 만들고 지정 된 콜백 루틴에 반환 하려고 합니다. 경우는 `error` 속성은 `null`오류가 발생 하 고 사용자에 게 표시 되어야 합니다. 가장 일반적인 오류는 고유 하지 않은 홈 이름 또는 HomeKit와 통신할 수 없는 홈 관리자 발생 합니다.

호출 해야 하는 홈을 만들었으면는 `UpdatePrimaryHome` 새 홈 기본 홈으로 설정 하는 메서드. 다시, 하는 경우는 `error` 속성은 `null`오류가 발생 하 고 사용자에 게 표시 되어야 합니다.

홈 관리자의 모니터링 해야 `DidAddHome` 및 `DidRemoveHome` 필요에 따라 응용 프로그램의 사용자 인터페이스 이벤트 및 업데이트 합니다.

> [!IMPORTANT]
> `AlertView.PresentOKAlert` 위의 샘플 코드에 사용 되는 방법을 사용 하는 iOS 경고 쉽게 있도록 HomeKitIntro 응용 프로그램에는 도우미 클래스입니다.


## <a name="finding-new-accessories"></a>새 보조 프로그램 찾기

기본 홈 정의 된 또는 홈 관리자에서 로드 된 Xamarin.iOS 앱 호출할 수 있습니다는 `HMAccessoryBrowser` 모든 새 홈 자동화 액세서리를 찾아 홈에 추가 합니다.

호출의 `StartSearchingForNewAccessories` 새로운 주변 장치를 찾고 시작 하는 메서드 및 `StopSearchingForNewAccessories` 완료 되 면 메서드.

> [!IMPORTANT]
> `StartSearchingForNewAccessories` 하지 두어야 부정적인 영향을 주므로 배터리 수명 및 iOS 장치 성능에 모두 오랜 시간 동안 실행 합니다. 호출을 제안 하는 Apple `StopSearchingForNewAccessories` 후 1 분 또는 찾기 액세서리 UI는 사용자에 게 제공 될 때만 검색 합니다.

`DidFindNewAccessory` 이벤트 새로운 기능이 발견 되 고에 추가 됩니다 때 호출 되는 `DiscoveredAccessories` 액세서리 브라우저 목록입니다.

`DiscoveredAccessories` 목록 컬렉션에 포함 됩니다 `HMAccessory` 가정 자동화 장치와 lights 또는 차고 도어 컨트롤 등의 사용 가능한 서비스 제공 HomeKit를 정의 하는 개체에 사용 하도록 설정 합니다.

새 액세서리 발견 되 면 사용자에 게 표시 되어야 합니다 및 있으므로 선택 수 고 홈에 추가 합니다. 예제:

[![](homekit-images/accessory01.png "새 액세서리 찾기")](homekit-images/accessory01.png#lightbox)

호출의 `AddAccessory` 메서드를 선택한 액세서리 홈의 컬렉션에 추가 합니다. 예를 들어:

```csharp
// Add the requested accessory to the home
ThisApp.HomeManager.PrimaryHome.AddAccessory (_controller.AccessoryBrowser.DiscoveredAccessories [indexPath.Row], (err) => {
    // Did an error occur
    if (err !=null) {
        // Inform user of error
        AlertView.PresentOKAlert("Add Accessory Error",err.LocalizedDescription,_controller);
    }
});
```

경우는 `err` 속성은 `null`오류가 발생 하 고 사용자에 게 표시 되어야 합니다. 그렇지 않으면 사용자를 추가 하려면 장치에 대 한 설치 코드를 입력 하 라는 표시 됩니다.

[![](homekit-images/accessory02.png "추가 하려면 장치에 대 한 설치 코드를 입력 합니다.")](homekit-images/accessory02.png#lightbox)

HomeKit 액세서리 시뮬레이터가이 숫자에서 찾을 수 있습니다는 **설치 코드** 필드:

[![](homekit-images/accessory03.png "HomeKit 액세서리 시뮬레이터에 있는 설치 코드 필드")](homekit-images/accessory03.png#lightbox)

실제 HomeKit accessories에 대 한 설치 코드 장치 자체에, 제품 상자 또는 액세서리의 설명서에서 레이블을 인쇄 하거나 됩니다.

액세서리 브라우저의 경우 모니터링 `DidRemoveNewAccessory` 사용자 자신의 홈 컬렉션에 추가 했습니다 후 사용 가능한 목록에서 보조 프로그램을 제거 하려면 이벤트와 업데이트 사용자 인터페이스입니다.

## <a name="working-with-accessories"></a>보조 사용

기본 홈 한 번이 고 설정 되어 및 액세서리에 추가 된를 작성 하려면 사용자에 대 한 accessories (및 필요에 따라 룸)의 목록을 제공할 수 있습니다.

`HMRoom` 개체에 지정된 된 대화방에 대 한 모든 정보 및 속해 있는 모든 액세서리 포함 합니다. 필요에 따라 하나 이상의 영역에 대화방을 구성할 수 있습니다. A `HMZone` 와 모든 대화방에 속하는 지정 된 영역에 대 한 모든 정보를 포함 합니다.

이 예제에서는에서는 합니다 될 구성을 간단 하 고 방 또는 영역으로 구성 하는 대신는 홈 액세서리를 직접 작동 합니다.

`HMHome` 개체에 할당 된 액세서리 사용자에 게 표시 될 수 있는 목록이 포함 되어 해당 `Accessories` 속성입니다. 예를 들어:

[![](homekit-images/accessory04.png "예제 액세서리")](homekit-images/accessory04.png#lightbox)

여기 양식에서 사용자 지정된 액세서리를 선택할 수 있으며 서비스를 제공 하는 작업을 수행 합니다.

## <a name="working-with-services"></a>서비스 작업

지정된 된 HomeKit 활성화 가정 자동화 장치와 상호 작용할 경우에 일반적으로 제공 되는 서비스를 통해 있습니다. `Services` 의 속성은 `HMAccessory` 클래스의 컬렉션을 포함 `HMService` 서비스를 정의 하는 개체는 장치를 제공 합니다.

서비스는 광원, 자동 온도, 차고문 개폐, 스위치 또는 잠금 등의 작업입니다. 일부 장치 (예: 차고 도어 열기)는 밝은 테마와 열기 또는 닫기 문을 하는 기능 등 하나 이상의 서비스를 제공 합니다.

주어진된 액세서리를 제공 하는 특정 서비스 외에 각 액세서리 포함는 `Information Service` 이름, 제조업체, 모델 및 일련 번호와 같은 속성을 정의 하는 합니다.

### <a name="accessory-service-types"></a>액세서리 서비스 유형

다음 서비스 형식을 통해 사용할 수 있습니다는 `HMServiceType` enum:

- **AccessoryInformation** -주어진된 가정 자동화 장치 (보조)에 대 한 정보를 제공 합니다.
- **AirQualitySensor** -무선 품질 센서를 정의 합니다.
- **배터리** -는 액세서리 배터리의 상태를 정의 합니다.
- **CarbonDioxideSensor** -탄소 이산화 센서를 정의 합니다.
- **CarbonMonoxideSensor** -일산화 센서를 정의 합니다.
- **ContactSensor** -연락처는 센서 (예: 열리거나 닫힐 창)을 정의 합니다.
- **문** -도어 상태 센서 (예: 열리거나 닫힐)을 정의 합니다.
- **팬** -원격 제어 팬을 정의 합니다.
- **GarageDoorOpener** -차고 도어 열기를 정의 합니다.
- **HumiditySensor** -습도 센서를 정의 합니다.
- **LeakSensor** -누수 센서 (핫 물이 열기 또는 세척기에 대 한 like)을 정의 합니다.
- **전구** -는 독립 실행형 또는 다른 액세서리 (예: 차고 도어 열기)의 일부인 표시등이 정의 합니다.
- **LightSensor** -광원 센서를 정의 합니다.
- **LockManagement** -자동화 된 도어 잠금을 관리 하는 서비스를 정의 합니다.
- **LockMechanism** -(예: 도어 잠금) 원격 제어 잠금을 정의 합니다.
- **MotionSensor** -는 동작 센서를 정의 합니다.
- **OccupancySensor** -점유율 센서를 정의 합니다.
- **콘센트** -원격 제어 콘센트를 정의 합니다.
- **SecuritySystem** -홈 보안 시스템을 정의 합니다.
- **StatefulProgrammableSwitch** -(예: 플립 스위치) 두 번 트리거됩니다 제공 상태를 유지 하는 프로그래밍 가능한 스위치를 정의 합니다.
- **StatelessProgrammableSwitch** -(예: 누름 단추) 트리거된 후 초기 상태로 반환 하는 프로그래밍 가능한 스위치를 정의 합니다.
- **SmokeSensor** -스모크 센서를 정의 합니다.
- **전환** -표준 벽 스위치와 마찬가지로 켜기/끄기 스위치를 정의 합니다.
- **TemperatureSensor** -온도 센서를 정의 합니다.
- **자동 온도 조절기** -HVAC 시스템을 제어 하는 데 서머 스마트 스 탯을 정의 합니다.
- **창** -정의 된 자동화 된 창을 케 인 원격으로 열거나 닫지 않아야 합니다.
- **WindowCovering** -차양 열거나 닫을 수 있는 등을 포함 하는 원격 제어 창을 정의 합니다.

### <a name="displaying-service-information"></a>서비스 정보를 표시합니다.

로드 한 후에 `HMAccessory` 개별을 쿼리할 수 있습니다 `HNService` 개체를 제공 하 고 사용자에 게 해당 정보를 표시 합니다.

[![](homekit-images/accessory05.png "서비스 정보를 표시합니다.")](homekit-images/accessory05.png#lightbox)

항상 해야 확인 해야는 `Reachable` 의 속성을 `HMAccessory` 작업을 시도 하기 전에. 액세서리는 연결할 수 없습니다. 사용자가 장치의 범위 내에서 또는 경우 해제 된 연결 수 있습니다.

서비스를 선택 하 고 나면 사용자 보거나를 모니터링 하거나 특정된 가정 자동화 장치를 제어 하는 서비스의 하나 이상의 특성을 수정할 수 있습니다.

<a name="Working-with-Characteristics" />

## <a name="working-with-characteristics"></a>특성 사용

각 `HMService` 개체의 컬렉션을 포함할 수 있습니다 `HMCharacteristic` (예: 도어를 열거나 닫을) 서비스의 상태에 대 한 정보를 제공 하거나 사용자 (예: 광원의 색을 설정) 상태를 조정할 수 있게 하는 개체입니다.

`HMCharacteristic` 뿐만 아니라 특성 및 해당 상태에 대 한 정보를 제공 하지만 통해 상태를 사용 하기 위한 메서드도 제공 _특성 메타 데이터_ (`HMCharacteristisMetadata`). 이 메타 데이터 상태를 수정 하는 사용자 또는 수 있도록에 정보를 표시할 때 도움이 되는 속성 (예: 최소 및 최대 값 범위)를 제공할 수 있습니다.

`HMCharacteristicType` 열거형에 정의 된 또는 다음과 같이 수정 될 수 있는 특성 메타 데이터 값의 집합을 제공 합니다.

 - AdminOnlyAccess
 - AirParticulateDensity
 - AirParticulateSize
 - AirQuality
 - AudioFeedback
 - BatteryLevel
 - 밝기
 - CarbonDioxideDetected
 - CarbonDioxideLevel
 - CarbonDioxidePeakLevel
 - CarbonMonoxideDetected
 - CarbonMonoxideLevel
 - CarbonMonoxidePeakLevel
 - ChargingState
 - ContactState
 - CoolingThreshold
 - CurrentDoorState
 - CurrentHeatingCooling
 - CurrentHorizontalTilt
 - CurrentLightLevel
 - CurrentLockMechanismState
 - CurrentPosition
 - CurrentRelativeHumidity
 - CurrentSecuritySystemState
 - CurrentTemperature
 - CurrentVerticalTilt
 - FirmwareVersion
 - HardwareVersion
 - HeatingCoolingStatus
 - HeatingThreshold
 - HoldPosition
 - 색상
 - Identify
 - InputEvent
 - LeakDetected
 - LockManagementAutoSecureTimeout
 - LockManagementControlPoint
 - LockMechanismLastKnownAction
 - 로그
 - 제조업체
 - 모델
 - MotionDetected
 - name
 - ObstructionDetected
 - OccupancyDetected
 - OutletInUse
 - OutputState
 - PositionState
 - PowerState
 - RotationDirection
 - RotationSpeed
 - 채도
 - 일련 번호
 - SmokeDetected
 - SoftwareVersion
 - StatusActive
 - StatusFault
 - StatusJammed
 - StatusLowBattery
 - StatusTampered
 - TargetDoorState
 - TargetHeatingCooling
 - TargetHorizontalTilt
 - TargetLockMechanismState
 - TargetPosition
 - TargetRelativeHumidity
 - TargetSecuritySystemState
 - TargetTemperature
 - TargetVerticalTilt
 - TemperatureUnits
 - 버전

### <a name="working-with-a-characteristics-value"></a>특성의 값으로 작업

호출을 보장 하기 위해 해당 하면 응용 프로그램에 지정 된 특성의 최신 상태는 `ReadValue` 의 메서드는 `HMCharacteristic` 클래스입니다. 경우는 `err` 속성은 `null`, 오류가 발생 했습니다 수도 있고 그렇지 사용자에 게 표시 되지 않을 수 있습니다.

특성의 `Value` 속성으로 지정 된 특성의 현재 상태를 포함 한 `NSObject`, 따라서 풀 수 없습니다와 직접에서 C# 및 합니다.

다음과 같은 도우미 클래스가에 추가 된 값을 읽을 수는 **HomeKitIntro** 샘플 응용 프로그램:

```csharp
using System;
using Foundation;
using System.Globalization;
using CoreGraphics;

namespace HomeKitIntro
{
    /// <summary>
    /// NS object converter is a helper class that helps to convert NSObjects into
    /// C# objects
    /// </summary>
    public static class NSObjectConverter
    {
        #region Static Methods
        /// <summary>
        /// Converts to an object.
        /// </summary>
        /// <returns>The object.</returns>
        /// <param name="nsO">Ns o.</param>
        /// <param name="targetType">Target type.</param>
        public static Object ToObject (NSObject nsO, Type targetType)
        {
            if (nsO is NSString) {
                return nsO.ToString ();
            }

            if (nsO is NSDate) {
                var nsDate = (NSDate)nsO;
                return DateTime.SpecifyKind ((DateTime)nsDate, DateTimeKind.Unspecified);
            }

            if (nsO is NSDecimalNumber) {
                return decimal.Parse (nsO.ToString (), CultureInfo.InvariantCulture);
            }

            if (nsO is NSNumber) {
                var x = (NSNumber)nsO;

                switch (Type.GetTypeCode (targetType)) {
                case TypeCode.Boolean:
                    return x.BoolValue;
                case TypeCode.Char:
                    return Convert.ToChar (x.ByteValue);
                case TypeCode.SByte:
                    return x.SByteValue;
                case TypeCode.Byte:
                    return x.ByteValue;
                case TypeCode.Int16:
                    return x.Int16Value;
                case TypeCode.UInt16:
                    return x.UInt16Value;
                case TypeCode.Int32:
                    return x.Int32Value;
                case TypeCode.UInt32:
                    return x.UInt32Value;
                case TypeCode.Int64:
                    return x.Int64Value;
                case TypeCode.UInt64:
                    return x.UInt64Value;
                case TypeCode.Single:
                    return x.FloatValue;
                case TypeCode.Double:
                    return x.DoubleValue;
                }
            }

            if (nsO is NSValue) {
                var v = (NSValue)nsO;

                if (targetType == typeof(IntPtr)) {
                    return v.PointerValue;
                }

                if (targetType == typeof(CGSize)) {
                    return v.SizeFValue;
                }

                if (targetType == typeof(CGRect)) {
                    return v.RectangleFValue;
                }

                if (targetType == typeof(CGPoint)) {
                    return v.PointFValue;
                }
            }

            return nsO;
        }

        /// <summary>
        /// Convert to string
        /// </summary>
        /// <returns>The string.</returns>
        /// <param name="nsO">Ns o.</param>
        public static string ToString(NSObject nsO) {
            return (string)ToObject (nsO, typeof(string));
        }

        /// <summary>
        /// Convert to date time
        /// </summary>
        /// <returns>The date time.</returns>
        /// <param name="nsO">Ns o.</param>
        public static DateTime ToDateTime(NSObject nsO){
            return (DateTime)ToObject (nsO, typeof(DateTime));
        }

        /// <summary>
        /// Convert to decimal number
        /// </summary>
        /// <returns>The decimal.</returns>
        /// <param name="nsO">Ns o.</param>
        public static decimal ToDecimal(NSObject nsO){
            return (decimal)ToObject (nsO, typeof(decimal));
        }

        /// <summary>
        /// Convert to boolean
        /// </summary>
        /// <returns><c>true</c>, if bool was toed, <c>false</c> otherwise.</returns>
        /// <param name="nsO">Ns o.</param>
        public static bool ToBool(NSObject nsO){
            return (bool)ToObject (nsO, typeof(bool));
        }

        /// <summary>
        /// Convert to character
        /// </summary>
        /// <returns>The char.</returns>
        /// <param name="nsO">Ns o.</param>
        public static char ToChar(NSObject nsO){
            return (char)ToObject (nsO, typeof(char));
        }

        /// <summary>
        /// Convert to integer
        /// </summary>
        /// <returns>The int.</returns>
        /// <param name="nsO">Ns o.</param>
        public static int ToInt(NSObject nsO){
            return (int)ToObject (nsO, typeof(int));
        }

        /// <summary>
        /// Convert to float
        /// </summary>
        /// <returns>The float.</returns>
        /// <param name="nsO">Ns o.</param>
        public static float ToFloat(NSObject nsO){
            return (float)ToObject (nsO, typeof(float));
        }

        /// <summary>
        /// Converts to double
        /// </summary>
        /// <returns>The double.</returns>
        /// <param name="nsO">Ns o.</param>
        public static double ToDouble(NSObject nsO){
            return (double)ToObject (nsO, typeof(double));
        }
        #endregion
    }
}
```

`NSObjectConverter` 특성의 현재 상태를 읽이 필요가 있는지 응용 프로그램 때마다 사용 됩니다. 예를 들면 다음과 같습니다.

```csharp
var value = NSObjectConverter.ToFloat (characteristic.Value);
```

에 값을 변환 하는 위의 코드는 `float` 사용할 수 있는 다음 Xamarin C# 코드에서입니다.

수정 하는 `HMCharacteristic`, 호출의 `WriteValue` 메서드에 새 값을 래핑합니다는 `NSObject.FromObject` 호출 합니다. 예를 들면 다음과 같습니다.

```csharp
Characteristic.WriteValue(NSObject.FromObject(value),(err) =>{
    // Was there an error?
    if (err!=null) {
        // Yes, inform user
        AlertView.PresentOKAlert("Update Error",err.LocalizedDescription,Controller);
    }
});
```

경우는 `err` 속성은 `null`에서 오류가 발생 했으며 사용자에 게 표시 되어야 합니다.

### <a name="testing-characteristic-value-changes"></a>특성 값 변경 내용을 테스트

작업할 때 `HMCharacteristics` 시뮬레이션된 액세서리, 수정 및는 `Value` HomeKit 액세서리 시뮬레이터 내 속성을 모니터링할 수 있습니다.

와 **HomeKitIntro** HomeKit 액세서리 시뮬레이터에 있는 실제 iOS 장치 하드웨어에는 특성 값 변경에서 실행 중인 앱을 거의 즉시 이해 되어야 합니다. 예를 들어 iOS 앱의 광원의 상태 변경:

[![](homekit-images/test01.png "IOS 앱의 광원의 상태 변경")](homekit-images/test01.png#lightbox)

HomeKit 액세서리 시뮬레이터에 있는 광원의 상태를 변경 해야 합니다. 값 변경 되지 않는 경우 새 특성 값을 쓸 때 오류 메시지의 상태를 확인 하 고 접근자 계속 연결할 수 있는지 확인 합니다.

## <a name="advanced-homekit-features"></a>고급 HomeKit 기능

이 문서 HomeKit 액세서리 Xamarin.iOS 앱에서 사용 하는 데 필요한 기본 기능 검사가 수행 됩니다. 그러나이 소개에 포함 되지 않는 HomeKit의 몇 가지 고급 기능 있습니다.

- **방** -최종 사용자가을 사용 하도록 설정 하는 HomeKit 액세서리 구성 필요에 따라 수 있습니다. 따라서 HomeKit을 이해 하 고 작업을 사용자에 대 한 쉬운 방식으로 현재 보조 프로그램을 수 있습니다. 만들고 방 유지 관리에 대 한 자세한 내용은 Apple의을 참조 하십시오. [HMRoom](https://developer.apple.com/library/prerelease/ios/documentation/HomeKit/Reference/HMRoom_Class/index.html#//apple_ref/occ/cl/HMRoom) 설명서입니다.
- **영역** -방 필요에 따라 영역으로 최종 사용자가 구성이 가능 합니다. 영역의 방 사용자가 하나의 단위로 처리할 수 있는 컬렉션을 참조 합니다. 예를 들어: Upstairs, Downstairs 또는 지하실 합니다. 다시, HomeKit을 나타내고 accessories 최종 사용자에 게 의미 있는 방식으로 작업할 수 있습니다. 만들기 및 영역을 유지 관리에 대 한 자세한 내용은 Apple의을 참조 하십시오. [HMZone](https://developer.apple.com/library/prerelease/ios/documentation/HomeKit/Reference/HMZone_Class/index.html#//apple_ref/occ/cl/HMZone) 설명서입니다.
- **작업 및 작업을 설정** -작업 액세서리 서비스 특성을 수정 하 고 집합으로 그룹화 할 수 있습니다. 작업 집합 액세서리의 그룹을 제어 하 고 상호 작용 하는 스크립트를 프록시로 합니다. 예를 들어 "TV 시청" 스크립트 수는 차양, 조명 dim를 닫고는 텔레비전 사운드 해당 시스템에 합니다. 만들기 및 작업 및 작업 집합을 유지 관리 하는 방법에 대 한 자세한 내용은 Apple의을 참조 하십시오. [HMAction](https://developer.apple.com/library/prerelease/ios/documentation/HomeKit/Reference/HMAction_Class/index.html#//apple_ref/occ/cl/HMAction) 및 [HMActionSet](https://developer.apple.com/library/prerelease/ios/documentation/HomeKit/Reference/HMActionSet_Class/index.html#//apple_ref/occ/cl/HMActionSet) 설명서입니다.
- **트리거** -트리거 하나를 사용할 수 또는 충족 되었을 때 지정 된 조건 집합이 더 많은 작업이 설정 합니다. 예를 들어 portch light 켜고 외부 어두운 가져오면 모든 외부 도어가 잠금. 만들기 및 트리거를 유지 관리 하는 방법에 대 한 자세한 내용은 Apple의을 참조 하십시오. [HMTrigger](https://developer.apple.com/library/prerelease/ios/documentation/HomeKit/Reference/HMTrigger_Class/index.html#//apple_ref/occ/cl/HMTrigger) 설명서입니다.

위에서 설명한 동일한 기법을 사용 하는 이러한 기능은 다음 Apple에 의해 구현 하기 쉬운 수 있어야 [HomeKitDeveloper 가이드](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/HomeKitDeveloperGuide/Introduction/Introduction.html), [HomeKit 사용자 인터페이스 지침](https://developer.apple.com/homekit/ui-guidelines/) 및 [ HomeKit 프레임 워크 참조](https://developer.apple.com/library/ios/home_kit_framework_ref)합니다.

## <a name="homekit-app-review-guidelines"></a>응용 프로그램 검토 HomeKit 지침

HomeKit 제출 하기 전에 활성화 된 Xamarin.iOS iTunes에 앱을 연결 iTunes App Store에서에서 릴리스 HomeKit 사용 앱에 대 한 Apple의 지침을 따를 것을 확인 합니다.

 - 응용 프로그램의 주 목적은 _해야_ HomeKit 프레임 워크를 사용 하는 경우 홈 자동화 수 있습니다.
 - 응용 프로그램의 마케팅 텍스트 HomeKit 사용량 및 개인 정보 취급 방침도 제공 해야 사용자가 알려야 합니다.
 - 사용자 정보를 수집 하거나 HomeKit를 사용 하 여 광고에 대 한 엄격 하 게 사용할 수 없습니다.

전체에 대 한 지침을 검토, Apple의를 참조 하세요 [앱 스토어 검토 지침](https://developer.apple.com/app-store/review/guidelines/)합니다.

## <a name="whats-new-in-ios-9"></a>IOS 9에 새로운 이란

Apple에 수행한 다음과 같은 변경 및 추가 HomeKit iOS 9에 대 한:

- **기존 개체를 유지 관리할** -홈 관리자는 기존 액세서리 수정 하는 경우 (`HMHomeManager`)으로 수정 된 특정 항목의 알려줍니다.
- **영구 식별자** -이제 모든 관련 HomeKit 클래스를 포함 한 `UniqueIdentifier` 속성이 HomeKit 간에 지정된 된 항목을 고유 하 게 식별 하도록 응용 프로그램 (또는 동일한 앱의 인스턴스)를 설정 합니다.
- **사용자 관리** -주 사용자의 홈 HomeKit 장치에 대 한 액세스 권한이 있는 사용자를 통해 사용자 관리를 제공 하는 기본 제공 뷰 컨트롤러를 추가 합니다.
- **사용자 기능** -HomeKit 사용자는 HomeKit에 사용할 수 있는 기능을 제어 하는 권한 집합을 되었습니다 및 HomeKit 보조 프로그램을 사용 하도록 설정 합니다. 앱 관련 기능 현재 사용자에 게 표시 해야 합니다. 예를 들어 한 관리자만 다른 사용자가 유지 하기 위해 수 있어야 합니다.
- **백그라운드에서 미리 정의 된** -평균 HomeKit 사용자에 대해 발생 하는 4 개의 일반적인 이벤트에 대 한 미리 정의 된 장면 만들었습니다: 준비, 유지, 반환, 평판으로 이동 합니다. 이러한 미리 정의 된 장면은 집에서 삭제할 수 없습니다.
- **장면 및 Siri** -Siri 장면 iOS 9 및 수에 HomeKit에 정의 된 모든 장면의 이름을 인식할 수에 대 한 지원이 향상을 있습니다. 사용자는 장면 Siri 하 고 이름을 말 하 여 실행할 수 있습니다.
- **보조 범주** -미리 정의 된 범주 집합에 모든 Accessories 및 홈에 추가 되 고 액세서리의 유형을 식별 하는 데 도움이 됩니다에 추가 되거나 앱 내에서 작업 합니다. 이러한 새 범주는 액세서리 설치 하는 동안 사용할 수 있습니다.
- **Apple Watch 지원** -HomeKit 이제 사용할 watchOS 및 Apple Watch 제어 시계 근처 되 고 iPhone 없이 장치를 사용 하는 HomeKit 할 수 있습니다. HomeKit watchOS에 대 한 다음과 같은 기능을 지원: 보기, 보조 프로그램 제어 가정과 백그라운드에서 실행 합니다.
- **새 이벤트 트리거 유형을** -8, iOS 9 지원 이벤트 트리거 액세서리 상태 (예: 센서 데이터) 또는 지리적 위치에 따라 이제 iOS에서 지원 되는 타이머 형식 트리거 뿐 아니라 합니다. 이벤트 트리거를 사용 하 여 `NSPredicates` 해당 실행에 대 한 조건을 설정 합니다.
- **원격 액세스** -와 원격 액세스를, 사용자가 제어할 수 이제 자신의 HomeKit 집 원격 위치에 자리를 비울 때 자동화 Accessories 홈 사용 하도록 설정 합니다. IOS 8에서에서 3 세대 가정에서 Apple TV a는 사용자가 경우에 지원 되었습니다이 있습니다. Ios 9에서는 이러한 제한이 적용 되지 않습니다 하 고 원격 액세스 iCloud와 HomeKit 액세서리 프로토콜 (HAP)을 통해 지원 됩니다.
- **새로운 Bluetooth 낮은 에너지 (배포용) 능력** -HomeKit Bluetooth 낮은 에너지 (배포용) 프로토콜을 통해 통신할 수 있는 더 많은 액세서리 유형을 지원 합니다. HAP Secure Tunneling를 사용 하는 HomeKit 보조 노출할 수 있습니다 다른 Bluetooth 액세서리 wi-fi (범위를 벗어나면 Bluetooth) 하는 경우. IOS 9 배포용 Accessories는 알림 및 메타 데이터에 대 한 전체를 지원 합니다.
- **새 보조 범주** -Apple iOS 9에에서 다음과 같은 새 액세서리 범주 추가: 창 Coverings, 한 문 및 Windows, 경보 시스템, 센서 및 프로그래밍 가능 스위치입니다.

IOS 9에에서 HomeKit의 새로운 기능에 대 한 자세한 내용은 Apple의를 참조 하십시오 [HomeKit 인덱스](https://developer.apple.com/homekit/) 및 [HomeKit의 새로운](https://developer.apple.com/videos/wwdc/2015/?id=210) 비디오.

## <a name="summary"></a>요약

이 문서에 Apple의 HomeKit 홈 자동화 프레임 워크를 도입 되었습니다. 설정 하 고 HomeKit 액세서리 시뮬레이터를 사용 하 여 테스트 장치를 구성 하는 방법과를 검색, 통신할 및 HomeKit를 사용 하 여 홈 자동화 장치를 제어 하는 간단한 Xamarin.iOS 앱을 만드는 방법에 알아보았습니다.



## <a name="related-links"></a>관련 링크

- [iOS 9 샘플](https://developer.xamarin.com/samples/ios/iOS9/)
- [개발자를 위한 iOS 9](https://developer.apple.com/ios/pre-release/)
- [IOS 9.0에 새로 이란](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [HomeKitDeveloper 가이드](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/HomeKitDeveloperGuide/Introduction/Introduction.html)
- [HomeKit 사용자 인터페이스 지침](https://developer.apple.com/homekit/ui-guidelines/)
- [HomeKit 프레임 워크 참조](https://developer.apple.com/library/ios/home_kit_framework_ref)
