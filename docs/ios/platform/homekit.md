---
title: Xamarin.iOS에서 HomeKit
description: HomeKit는 홈 자동화 장치를 제어 하는 것에 대 한 Apple 프레임 워크. 이 문서는 HomeKit를 소개 하 고 HomeKit 액세서리 시뮬레이터 및 간단한 Xamarin.iOS 앱을 작성을 이러한 보조 프로그램을 조작할 구성 테스트 accessories를 다룹니다.
ms.prod: xamarin
ms.assetid: 90C0C553-916B-46B1-AD52-1E7332792283
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/22/2017
ms.openlocfilehash: 9daedbe9bba5a2923a247104c4e69ae2e1b635aa
ms.sourcegitcommit: 7ccc7a9223cd1d3c42cd03ddfc28050a8ea776c2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/13/2019
ms.locfileid: "67865780"
---
# <a name="homekit-in-xamarinios"></a>Xamarin.iOS에서 HomeKit

_HomeKit는 홈 자동화 장치를 제어 하는 것에 대 한 Apple 프레임 워크. 이 문서는 HomeKit를 소개 하 고 HomeKit 액세서리 시뮬레이터 및 간단한 Xamarin.iOS 앱을 작성을 이러한 보조 프로그램을 조작할 구성 테스트 accessories를 다룹니다._

[![](homekit-images/accessory01.png "예로 HomeKit 사용 앱 사용")](homekit-images/accessory01.png#lightbox)

Apple ios 8 하나의 단위로 여러 공급 업체에서 여러 홈 자동화 장치를 원활 하 게 통합 하는 방법으로 HomeKit를 도입 했습니다. 검색에 대 한 일반적인 프로토콜을 촉진 하 여 구성 및 홈 자동화 장치 제어 HomeKit 장치에서에서 허용 함께 작동 하 여 작업을 조정 하지 않아도 개별 공급 업체 없이 공급 업체와 관련 되지 않은 합니다.

HomeKit를 사용 하 여 앱 또는 Api 제공 공급 업체를 사용 하지 않고 모든 HomeKit 사용 장치를 제어 하는 Xamarin.iOS 앱을 만들 수 있습니다. HomeKit를 사용 하면 다음을 수행할 수 있습니다.:

- 새 HomeKit 사용 홈 자동화 장치를 검색 하 고 모든 사용자의 iOS 장치에서 지속 되는 데이터베이스에 추가 합니다.
- 설치, 구성, 표시 및 모든 장치는 HomeKit에서 제어 _구성 데이터베이스 홈_합니다.
- 미리 구성 된 HomeKit 장치와 통신 하 고 개별 작업을 수행 하거나 모든 주방에서 표시등 켜기 등의 공동에서 작업 하도록 명령 합니다.

장치 HomeKit 사용 앱 홈 구성 데이터베이스에서 사용자에 게 제공 하는 것 외에도 HomeKit Siri 음성 명령에 대 한 액세스를 제공 합니다. HomeKit 설치를 적절 하 게 구성 된 지정 된, 사용자 명령을 실행할 수 있습니다 음성와 같은 "Siri 거실에 전등 켜기."

<a name="Home-Configuration-Database" />

## <a name="the-home-configuration-database"></a>홈 구성 데이터베이스

HomeKit 홈 컬렉션에 지정된 된 위치에서 모든 automation 장치를 구성합니다. 이 컬렉션에는 사용자가 홈 자동화 장치 의미를 이해 하기 쉬운 레이블 사용 하 여 논리적으로 정렬 된 단위로 그룹화 하는 방법을 제공 합니다.

홈 컬렉션 홈 구성 데이터베이스를 자동으로 되는 백업 및 동기화 된 모든 사용자의 iOS 장치에 저장 됩니다. HomeKit 홈 구성 데이터베이스를 사용 하 여 작업에 대 한 다음 클래스를 제공 합니다.

- `HMHome` -이 (예: 단일 물리적 위치에서 모든 정보 및 모든 홈 자동화 장치에 대 한 구성을 포함 하는 최상위 컨테이너 단일 제품군 거주). 사용자는 자신의 기본 홈 휴가 집 등 둘 이상의 거주가 있을 수 있습니다. 또는 다른 "보관" 동일한 속성에는 차고 통해 게스트 집 주 집 등을 가질 수 있습니다. 하나 이상의 어느 `HMHome` 개체 _해야_ 설치 되며 저장 기타 HomeKit 정보를 입력할 수 있습니다.
- `HMRoom` -동안 선택적를 `HMRoom` 홈 내에서 특정 대화방을 정의할 수 있습니다 (`HMHome`)와 같은: 주방, 욕실, 차고 또는 가정용 합니다. 홈 자동화 장치에 자신의 집에서 특정 위치에 모든 사용자 그룹 수를 `HMRoom` 단위로 따라 작업을 수행 합니다. 예를 들어, Siri garage 광원 해제를 요청 합니다.
- `HMAccessory` -이 나타내는 개인, 실제 HomeKit 사용 자동화 장치 (예: 스마트 자동 온도 조절기) 사용자의 거주지에 설치 된. 각 `HMAccessory` 에 할당 되는 `HMRoom`합니다. 사용자 모든 대화방을 구성 하지 않은 경우 HomeKit 액세서리 특별 기본 대화방에 할당 합니다.
- `HMService` -제공 하는 서비스를 나타내는 지정 된 `HMAccessory`, 광원의 색 (지원 되는 색 변경) 하는 경우 설정/해제 상태와 같은 합니다. 각 `HMAccessory` 도 광원을 포함 하는 차고 도어 열기 등의 서비스가 둘 이상 있을 수 있습니다. 또한는 주어진 `HMAccessory` 서비스 펌웨어 업데이트와 같은 사용자 정의 컨트롤의 외부에 있는 있을 수 있습니다.
- `HMZone` -컬렉션을 그룹화 할 수 있습니다. `HMRoom` Upstairs, Downstairs 지하실 등의 논리 영역으로는 개체입니다. 선택 사항 이지만, 이렇게 하면 Siri 요청 같은 상호 작용에 대 한 해제 downstairs 광원의 모든 설정.

<a name="Provisioning-a-HomeKit-App" />

## <a name="provisioning-a-homekit-app"></a>HomeKit 앱을 프로 비전

HomeKit 따른 보안 요구 사항으로 인해 HomeKit 프레임 워크를 사용 하 여 Xamarin.iOS 앱을 제대로 구성 해야 Xamarin.iOS 프로젝트 파일에서 Apple 개발자 포털에 합니다.

다음을 수행합니다.

1. 에 로그인 합니다 [Apple Developer 포털](https://developer.apple.com)합니다.
2. 클릭할 **인증서, 식별자 및 프로필**합니다.
3. 이미 않았다면 클릭할 **식별자** 앱 ID를 만듭니다 (예: `com.company.appname`), 다른 기존 ID를 편집
4. 있는지 확인 합니다 **HomeKit** 서비스에 지정된 된 ID에 대 한 확인: 

    [![](homekit-images/provision01.png "지정된 된 ID에 대 한 HomeKit 서비스를 사용 하도록 설정")](homekit-images/provision01.png#lightbox)
5. 변경 내용을 저장합니다.
6. 클릭할 **프로 비전 프로필** > **개발** 새 개발 프로 비전 앱에 대 한 프로필을 만듭니다. 

    [![](homekit-images/provision02.png "새 개발 프로 비전 앱에 대 한 프로필 만들기")](homekit-images/provision02.png#lightbox)
7. 다운로드 및 새 프로 비전 프로필을 설치 또는 Xcode를 사용 하 여 다운로드 하 고 프로필을 설치 합니다.
8. Xamarin.iOS 프로젝트 옵션을 편집 하 고 방금 만든 프로 비전 프로필을 사용 하 고 있는지 확인 합니다. 

    [![](homekit-images/provision03.png "방금 만든 프로 비전 프로필 선택")](homekit-images/provision03.png#lightbox)
9. 다음으로, 편집 하 **Info.plist** 파일을 프로 비전 프로필을 만드는 데 사용 된 앱 ID를 사용 하 고 있는지 확인: 

    [![](homekit-images/provision04.png "앱 ID를 설정 합니다. ")](homekit-images/provision04.png#lightbox)
10. 마지막으로 편집 하 **Entitlements.plist** 파일을 확인 합니다 **HomeKit** 자격 선정 된: 

    [![](homekit-images/provision05.png "HomeKit 자격을 사용 하도록 설정")](homekit-images/provision05.png#lightbox)
11. 모든 파일에 변경 내용을 저장 합니다.

현재 위치에서 이러한 설정을 사용 하 여 응용 프로그램이 HomeKit 프레임 워크 Api에 액세스할 준비가 되었습니다. 프로 비전에 대 한 자세한 내용은 참조 하십시오 우리의 [Device Provisioning](~/ios/get-started/installation/device-provisioning/index.md) 하 고 [앱 프로 비전](~/ios/get-started/installation/device-provisioning/index.md) 가이드입니다.

> [!IMPORTANT]
> HomeKit 사용 앱을 테스트 개발에 대 한 제대로 프로 비전 하는 실제 iOS 장치에 필요 합니다. HomeKit는 iOS 시뮬레이터에서에서 테스트할 수 없습니다.

## <a name="the-homekit-accessory-simulator"></a>HomeKit 액세서리 시뮬레이터

물리적 장치를 설치 하지 않더라도 모든 가능한 홈 자동화 장치 및 서비스를 테스트 하는 방법을 제공 하기 위해 Apple 만듭니다는 _HomeKit 액세서리 시뮬레이터_합니다. 이 시뮬레이터를 사용 하 여, 설치 수 있으며 가상 HomeKit 장치를 구성할 수도 있습니다.

### <a name="installing-the-simulator"></a>설치는 시뮬레이터

계속 하기 전에 설치 해야 하므로 Apple HomeKit 액세서리 시뮬레이터 별도 다운로드로 Xcode에서 제공 합니다.

다음을 수행합니다.

1. 웹 브라우저에서 방문 [Apple 개발자를 위한 다운로드](https://developer.apple.com/download/more/?name=for%20Xcode)
2. 다운로드 합니다 **Xcode xxx에 대 한 추가 도구** (여기서 xxx는 설치 된 Xcode의 버전): 

    [![](homekit-images/simulator01.png "Xcode에 대 한 추가 도구를 다운로드 합니다.")](homekit-images/simulator01.png#lightbox)
3. 디스크 이미지를 열고 도구를 설치 하 **응용 프로그램** 디렉터리입니다.

설치 HomeKit 액세서리 시뮬레이터를 사용 하 여 테스트를 위해 가상 보조 프로그램을 만들 수 있습니다.

### <a name="creating-virtual-accessories"></a>가상 보조 프로그램 만들기

HomeKit 액세서리 시뮬레이터를 시작 하 고를 몇 가지 가상 accessories 만들기 하려면 다음을 수행 합니다.

1. 응용 프로그램 폴더에서 HomeKit 액세서리 시뮬레이터를 시작 합니다. 

    [![](homekit-images/simulator02.png "HomeKit 액세서리 시뮬레이터")](homekit-images/simulator02.png#lightbox)
2. 클릭 합니다 **+** 단추를 선택 **새 액세서리 중...** : 

    [![](homekit-images/simulator03.png "새 보조를 추가 합니다.")](homekit-images/simulator03.png#lightbox)
3. 새 접근자에 대 한 정보를 입력 하 고 클릭 합니다 **완료** 단추: 

    [![](homekit-images/simulator04.png "새 접근자에 대 한 정보를 입력합니다")](homekit-images/simulator04.png#lightbox)
4. 클릭 된 **서비스 추가...** 단추 및 드롭다운 목록에서 서비스 유형 선택: 

    [![](homekit-images/simulator05.png "드롭다운 목록에서 서비스 유형 선택")](homekit-images/simulator05.png#lightbox)
5. 제공을 **이름** 서비스를 클릭 합니다 **마침** 단추: 

    [![](homekit-images/simulator06.png "서비스의 이름을 입력 합니다.")](homekit-images/simulator06.png#lightbox)
6. 클릭 하 여 서비스에 대 한 선택적 특성을 제공할 수 있습니다는 **특성 추가** 단추 및 필요한 설정을 구성 합니다. 

    [![](homekit-images/simulator07.png "필요한 설정 구성")](homekit-images/simulator07.png#lightbox)
7. 각 유형의 HomeKit 지 원하는 가상 홈 자동화 장치 중 하나를 만들려면 위의 단계를 반복 합니다.

일부 샘플 가상 HomeKit 액세서리 생성 및 구성에 사용 하 여 사용 하 고 Xamarin.iOS 앱에서 이러한 장치를 제어 합니다. 이제 수 있습니다.

## <a name="configuring-the-infoplist-file"></a>Info.plist 파일 구성

IOS 10의 새로운 기능 (이상)에 개발자 추가 해야 합니다 `NSHomeKitUsageDescription` 앱의 키 `Info.plist` 파일 및 앱 사용자의 HomeKit 데이터베이스에 액세스 하려고 하는 이유는 무엇을 선언 문자열을 제공 합니다. 이 문자열은 사용자가 처음에 앱을 실행 하기에 표시 됩니다.

[![](homekit-images/info01.png "HomeKit 사용 권한 대화 상자")](homekit-images/info01.png#lightbox)

이 키를 설정 하려면 다음을 수행 합니다.

1. 두 번 클릭 합니다 `Info.plist` 파일을 **솔루션 탐색기** 을 편집용으로 엽니다.
2. 화면 아래쪽으로 전환 합니다 **원본** 보기.
3. 새 **항목이** 목록에 있습니다.
4. 드롭다운 목록에서 선택 **개인 정보-HomeKit 사용 설명**: 

    [![](homekit-images/info02.png "개인 정보-HomeKit 사용 설명 선택")](homekit-images/info02.png#lightbox)
5. 앱 사용자의 HomeKit 데이터베이스에 액세스 하려고 하는 이유는 무엇에 대 한 설명을 입력 합니다. 

    [![](homekit-images/info03.png "설명을 입력합니다")](homekit-images/info03.png#lightbox)
6. 파일의 변경 내용을 저장합니다.

> [!IMPORTANT]
> 설정 하지 못했습니다 합니다 `NSHomeKitUsageDescription` 키를 `Info.plist` 파일은 앱에서 발생 합니다 _자동으로 실패_ (되 런타임 시 시스템에 의해 닫힘) iOS 10 이상에서 실행 하는 경우 오류 없이 합니다.

## <a name="connecting-to-homekit"></a>HomeKit에 연결

HomeKit와 통신 하려면 Xamarin.iOS 앱의 인스턴스를 인스턴스화하고 먼저 해야는 `HMHomeManager` 클래스입니다. 홈 Manager HomeKit 중앙 진입점 이며 사용 가능한 집의 목록을 제공 하는 업데이트 및 해당 목록을 유지 관리 하 고 사용자의 반환 _기본 홈_합니다.

`HMHome` 방, 그룹 또는 설치 된 모든 가정 자동화 액세서리와 함께 해당가 포함할 수 있는 영역 등 제공 홈에 대 한 모든 정보를 포함 하는 개체입니다. HomeKit 하나 이상에서 모든 작업을 수행 하려면 `HMHome` 만들어지고 기본 홈으로 지정 해야 합니다.

앱이 기본 홈 있는지 확인 하 고 만들고 없으면 하나를 할당 하는 일을 담당 합니다.

### <a name="adding-a-home-manager"></a>홈 관리자 추가

HomeKit 인식 Xamarin.iOS 앱에 추가 하려면 편집 합니다 **AppDelegate.cs** 파일을 편집 하 고 다음과 같이 표시 되도록 합니다.

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

응용 프로그램을 처음 실행할 때 사용자가 묻습니다 HomeKit 정보에 액세스할 수 있도록 하려는 경우:

[![](homekit-images/home01.png "사용자는 자신의 HomeKit 정보에 액세스할 수 있도록 하려는 경우 묻는")](homekit-images/home01.png#lightbox)

사용자가 대답 **확인**, 응용 프로그램은 해당 HomeKit 액세서리와 함께 사용할 수 됩니다 하지는 그렇지 않은 경우 및 HomeKit 호출 오류와 함께 실패 합니다.

다음 위치에서 홈 관리자와이 응용 프로그램 기본 홈을 구성한 경우 보고 그렇지 않은 경우 하나를 만들고 사용자에 대 한 방법을 제공 해야 합니다.

### <a name="accessing-the-primary-home"></a>기본 홈에 액세스

위에서 설명한 대로 기본 홈을 생성 하 고 HomeKit를 사용할 수 없으면 기본 홈을 만들고 사용자에 대 한 방법을 제공 해야 하는 앱의 존재 하지 않는 것이 먼저 구성 해야 합니다.

모니터링 해야 앱 먼저 시작, 백그라운드에서 반환 하는 경우는 `DidUpdateHomes` 의 이벤트는 `HMHomeManager` 주 집의 존재 여부를 확인 하는 클래스입니다. 존재 하지 않는 경우 만드는 사용자에 대 한 인터페이스를 제공 해야 합니다.

다음 코드는 기본 홈에 대 한 확인 하는 뷰 컨트롤러를 추가할 수 있습니다.

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

홈 Manager HomeKit를를 연결할 때는 `DidUpdateHomes` 이벤트가 발생 하 고 모든 기존 집 집의 관리자의 컬렉션에 로드할 기본 홈이 사용 가능한 경우 로드할 수 있습니다.

### <a name="adding-a-primary-home"></a>기본 홈 추가

경우는 `PrimaryHome` 의 속성 합니다 `HMHomeManager` 는 `null` 후를 `DidUpdateHomes` 계속 하기 전에 기본 홈을 만들고 사용자에 대 한 방법을 제공 해야 하는 경우.

일반적으로 앱 사용자가 다음 기본 홈으로 설정 하는 홈 관리자에 전달 되는 새 홈에 대 한 양식을 제공 합니다. 에 대 한 합니다 **HomeKitIntro** 모달 뷰 샘플 앱 IOS 디자이너에서 생성 되어 호출한는 `AddHomeSegue` 앱의 기본 인터페이스에서 segue 합니다.

새 홈 및 홈을 추가 하는 단추에 대 한 이름을 입력 하 여 사용자에 대 한 텍스트 필드를 제공 합니다. 사용자가 누를 때 합니다 **추가 홈** 단추, 다음 코드를 홈 추가할 홈 관리자를 호출 합니다.

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

`AddHome` 메서드는 새 홈을 만들고 지정 된 콜백 루틴을 반환 하려고 합니다. 경우는 `error` 속성은 `null`사용자에 게 표시 되 고 오류가 발생 했습니다. 가장 일반적인 오류는는 고유 하지 않은 홈 이름 또는 HomeKit 통신할 수 없는 홈 관리자를 기준으로 발생 합니다.

호출 해야 하는 홈 성공적으로 만들어졌으면는 `UpdatePrimaryHome` 새 홈 기본 홈으로 설정 하는 방법입니다. 마찬가지로 경우 합니다 `error` 속성은 `null`사용자에 게 표시 되 고 오류가 발생 했습니다.

홈 관리자의 모니터링 해야 `DidAddHome` 고 `DidRemoveHome` 필요에 따라 앱의 사용자 인터페이스 이벤트 및 업데이트 합니다.

> [!IMPORTANT]
> `AlertView.PresentOKAlert` 위의 샘플 코드에 사용 되는 메서드는 작업할 수 있게 iOS를 사용 하 여 경고 쉽게 HomeKitIntro 응용 프로그램에는 도우미 클래스입니다.


## <a name="finding-new-accessories"></a>새 보조 프로그램 찾기

Xamarin.iOS 앱을 호출할 수 있습니다 기본 홈 정의 되었거나 홈 관리자에서 로드 되 면는 `HMAccessoryBrowser` 모든 새 홈 자동화 accessories 찾아 집에 추가 합니다.

호출 된 `StartSearchingForNewAccessories` 새 accessories에 대 한 확인을 시작 하는 방법 및 `StopSearchingForNewAccessories` 완료 되 면 메서드.

> [!IMPORTANT]
> `StartSearchingForNewAccessories` 하지 두어야 배터리와 iOS 장치의 성능에 부정적인 영향이 있으므로 오랜 시간 동안 실행 합니다. Apple에서 제안 하 호출 `StopSearchingForNewAccessories` 후 1 분 또는 찾을 액세서리 UI를 사용자에 게 표시 되 면만 검색 합니다.

`DidFindNewAccessory` 새 보조 프로그램은 검색 되며를 추가할 때 호출 될 이벤트는 `DiscoveredAccessories` 액세서리 브라우저에서 목록입니다.

`DiscoveredAccessories` 의 컬렉션이 포함 된 경우 목록 `HMAccessory` 홈 자동화 장치 및 표시등 또는 garage 도어 컨트롤 같은 해당 제공 서비스 주어진 HomeKit를 정의 하는 개체에 사용 하도록 설정 합니다.

새 액세서리를 찾으면 사용자에 게 표시 되 고 선택 하 고 홈에 추가할 수 있으므로 합니다. 예제:

[![](homekit-images/accessory01.png "새 보조 찾기")](homekit-images/accessory01.png#lightbox)

호출 된 `AddAccessory` 선택한 액세서리 홈의 컬렉션에 추가 하는 방법입니다. 예:

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

경우는 `err` 속성은 `null`사용자에 게 표시 되 고 오류가 발생 했습니다. 이 고, 그렇지 사용자는 추가 장치에 대 한 설치 코드를 입력 하 라는 메시지가 표시 수 있습니다:

[![](homekit-images/accessory02.png "추가 장치에 대 한 설치 코드를 입력 합니다.")](homekit-images/accessory02.png#lightbox)

HomeKit 액세서리 시뮬레이터가이 숫자에서 찾을 수 있습니다 합니다 **설치 코드** 필드:

[![](homekit-images/accessory03.png "HomeKit 액세서리 시뮬레이터에서 설치 코드 필드")](homekit-images/accessory03.png#lightbox)

실제 HomeKit accessories에 대해 설정 된 코드는 장치 자체에 접근자의 설명서 또는 제품 상자에에서 레이블을 인쇄 하거나 됩니다.

액세서리 브라우저의 모니터링 `DidRemoveNewAccessory` 이벤트 및 업데이트 사용자 인터페이스 사용자가 해당 홈 해당 컬렉션에 추가 되 면 사용 가능한 목록의 보조 프로그램을 제거 합니다.

## <a name="working-with-accessories"></a>보조 사용

기본 홈 한 번 설정 하 고 보조 프로그램을 추가한를 사용 하려면 사용자에 대 한 accessories (및 필요에 따라 실)의 목록을 제공할 수 있습니다.

`HMRoom` 여기에 속한 모든 accessories 및 지정된 된 대화방에 대 한 모든 정보를 포함 하는 개체입니다. 필요에 따라 하나 이상의 영역으로 대화방을 구성할 수 있습니다. `HMZone` 지정 된 영역에 대 한 모든 정보 및 여기에 속한 대화방의 모든 포함 되어 있습니다.

이 예제의 경우에서는 됩니다 수 유지 작업 간단 하 고 방 또는 영역 구조로 구성 하는 대신 집의 보조 프로그램을 직접 작동 합니다.

합니다 `HMHome` 개체에 할당 된 액세서리 사용자에 게 표시할 수 있는 목록이 해당 `Accessories` 속성입니다. 예를 들어:

[![](homekit-images/accessory04.png "예제에서는 보조 프로그램")](homekit-images/accessory04.png#lightbox)

다음 양식에서 사용자 수는 지정 된 보조를 선택 하 고 제공 하는 서비스를 사용 하 여 작동 합니다.

## <a name="working-with-services"></a>서비스 작업

지정된 된 HomeKit 사용된 홈 자동화 장치를 사용 하 여 사용자 상호 작용을 때 일반적으로 제공 하는 서비스를 통해. `Services` 의 속성을 `HMAccessory` 클래스의 컬렉션을 포함 `HMService` 개체 서비스를 정의 하는 장치를 제공 합니다.

서비스는 조명, 온도, 차고문 개폐, 스위치 또는 잠금 등입니다. (예: garage 도어 열기) 일부 장치는 광원 및 열기 또는 닫기는 도어 수와 같은 하나 이상의 서비스를 제공 합니다.

특정 서비스는 지정 된 보조 제공 하는 것 외에도 각 액세서리 포함는 `Information Service` 이름, 제조업체, 모델 및 일련 번호와 같은 속성을 정의 하는 합니다.

### <a name="accessory-service-types"></a>보조 서비스 유형

다음 서비스 형식을 통해 사용할 수는 `HMServiceType` 열거형:

- **AccessoryInformation** -지정 된 홈 자동화 장치 (보조)에 대 한 정보를 제공 합니다.
- **AirQualitySensor** -는 공기 품질 센서를 정의 합니다.
- **배터리** -액세서리의 배터리 상태를 정의 합니다.
- **CarbonDioxideSensor** -탄소 이산화 센서를 정의 합니다.
- **CarbonMonoxideSensor** -일산화 센서를 정의 합니다.
- **ContactSensor** -연락처 센서 (예: 열거나 닫을 창)을 정의 합니다.
- **도어** -도어 상태 센서 (예: 열기 또는 닫기)를 정의 합니다.
- **팬** -원격 제어 팬을 정의 합니다.
- **GarageDoorOpener** -garage 도어 열기를 정의 합니다.
- **HumiditySensor** -습도 센서를 정의 합니다.
- **LeakSensor** -누수 센서 (온수 기 또는 세척기와 같이)를 정의 합니다.
- **전구** -는 독립 실행형 또는 다른 액세서리 (예: garage 도어 열기)의 일부인 표시등이 정의 합니다.
- **LightSensor** -광원 센서를 정의 합니다.
- **LockManagement** -한 자동화 된 도어 잠금을 관리 하는 서비스를 정의 합니다.
- **LockMechanism** -(예: 도어 잠금) 원격 제어 잠금을 정의 합니다.
- **MotionSensor** -동작 센서를 정의 합니다.
- **OccupancySensor** -점유율 센서를 정의 합니다.
- **출 선** -원격 제어 콘센트를 정의 합니다.
- **SecuritySystem** -홈 보안 시스템을 정의 합니다.
- **StatefulProgrammableSwitch** -(예: 플립 스위치)를 한 번 트리거 제공 상태를 유지 하는 프로그래밍 가능 스위치를 정의 합니다.
- **StatelessProgrammableSwitch** -(예: 누름 단추) 트리거된 후 초기 상태를 반환 하는 프로그래밍 가능 스위치를 정의 합니다.
- **SmokeSensor** -스모크 센서를 정의 합니다.
- **전환** -표준 벽 스위치 처럼 켜기/끄기 스위치를 정의 합니다.
- **TemperatureSensor** -온도 센서를 정의 합니다.
- **자동 온도 조절기** -HVAC 시스템을 제어 하는 데는 스마트 자동 온도 조절기를 정의 합니다.
- **창** -는 케 인 원격으로 열거나 닫지 않아야 하는 자동화 된 창을 정의 합니다.
- **WindowCovering** -원격 제어 창을 열거나 닫을 수 있는 블라인드 등을 다루는 정의 합니다.

### <a name="displaying-service-information"></a>서비스 정보를 표시합니다.

로드 한 후는 `HMAccessory` 개별을 쿼리할 수 있습니다 `HNService` 개체를 제공 하 고 사용자에 게 해당 정보를 표시 합니다.

[![](homekit-images/accessory05.png "서비스 정보를 표시합니다.")](homekit-images/accessory05.png#lightbox)

해야 항상 확인 해야 합니다 `Reachable` 의 속성을 `HMAccessory` 에 다시 시도 하기 전에 합니다. 보조 프로그램에 연결할 수 없는 사용자가 장치의 범위 내에서 또는 경우 중단 된 연결 수 있습니다.

서비스를 선택한 후 사용자 보거나 해당 서비스를 모니터링 하거나 특정된 홈 자동화 장치 제어의 하나 이상의 특성을 수정할 수 있습니다.

<a name="Working-with-Characteristics" />

## <a name="working-with-characteristics"></a>특성 사용

각 `HMService` 개체의 컬렉션을 포함할 수 있습니다 `HMCharacteristic` (예: 도어를 열거나 닫을) 서비스의 상태에 대 한 정보를 제공 하거나 (예: 광원의 색을 설정) 된 상태를 조정 하는 데 사용할 수 있는 개체입니다.

`HMCharacteristic` 또한를 통해 상태를 사용 하 여 작업 하기 위한 메서드를 제공 하지만 특성 및 해당 상태에 대 한 정보를 제공할 뿐 아니라 _특성은 메타 데이터_ (`HMCharacteristisMetadata`). 이 메타 데이터는 사용자 또는 있도록 상태를 수정 하 게 정보를 표시 하는 경우에 유용 하는 속성 (예: 최소 및 최대 값 범위)를 제공할 수 있습니다.

`HMCharacteristicType` 열거형 정의 하거나 다음과 같이 수정할 수 있는 특성 메타 데이터 값의 집합을 제공 합니다.

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
- Model
- MotionDetected
- 이름
- ObstructionDetected
- OccupancyDetected
- OutletInUse
- OutputState
- PositionState
- PowerState
- RotationDirection
- RotationSpeed
- 채도
- SerialNumber
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

### <a name="working-with-a-characteristics-value"></a>특성의 값을 사용 하 여 작업

호출 앱이 지정 된 특성의 최신 상태가 되도록 합니다 `ReadValue` 메서드는 `HMCharacteristic` 클래스입니다. 경우는 `err` 속성은 `null`, 오류가 발생 했습니다 수도 있고 그렇지 사용자에 게 표시 되지 않을 수 있습니다.

특성의 `Value` 속성으로 지정 된 특성의 현재 상태를 포함 합니다.는 `NSObject`, 하 고 따라서 없습니다 수에서 직접 C#합니다.

다음 도우미 클래스에 추가 된 값을 읽으려면 합니다 **HomeKitIntro** 샘플 응용 프로그램:

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

`NSObjectConverter` 응용 프로그램의 현재 상태를 파악 해야 할 때마다 사용 됩니다. 예를 들면 다음과 같습니다.

```csharp
var value = NSObjectConverter.ToFloat (characteristic.Value);
```

위의 줄에 값을 변환 된 `float` 사용할 수 있는 다음 Xamarin에서 C# 코드.

수정 하는 `HMCharacteristic`, 호출 해당 `WriteValue` 메서드의 새 값을 래핑합니다를 `NSObject.FromObject` 호출 합니다. 예를 들면 다음과 같습니다.

```csharp
Characteristic.WriteValue(NSObject.FromObject(value),(err) =>{
    // Was there an error?
    if (err!=null) {
        // Yes, inform user
        AlertView.PresentOKAlert("Update Error",err.LocalizedDescription,Controller);
    }
});
```

경우는 `err` 속성은 `null`, 오류가 발생 했으며 사용자에 게 표시 합니다.

### <a name="testing-characteristic-value-changes"></a>테스트 특성 값 변경

작업할 때 `HMCharacteristics` 시뮬레이션 된 보조 프로그램을 수정 합니다 `Value` HomeKit 액세서리 시뮬레이터 내에서 속성을 모니터링할 수 있습니다.

사용 하 여 합니다 **HomeKitIntro** HomeKit 액세서리 시뮬레이터에서 앱 실제 iOS 장치 하드웨어를 특성의 값으로 변경 될 때까지 실행 거의 즉시 이해 되어야 합니다. 예를 들어 iOS 앱에서 광원의 상태를 변경 합니다.

[![](homekit-images/test01.png "IOS 앱에서 광원의 상태를 변경합니다.")](homekit-images/test01.png#lightbox)

HomeKit 액세서리 시뮬레이터에서 광원의 상태를 변경 해야 합니다. 값 변경 되지 않는 경우 새 특성 값을 작성 하는 경우 오류 메시지의 상태를 확인 하 고 접근자에 계속 연결할 수 있는지 확인 합니다.

## <a name="advanced-homekit-features"></a>고급 HomeKit 기능

이 문서에서는 Xamarin.iOS 앱에 HomeKit 액세서리를 사용 하는 데 필요한 기본 기능 설명 했습니다. 그러나이 소개에서 다루지 않는 HomeKit의 몇 가지 고급 기능이 있습니다.

- **대화방** -최종 사용자가 회의실에 사용 하도록 설정 하는 HomeKit 액세서리 구성 필요에 따라 수 있습니다. 이렇게 하면 쉽게 이해 하 고 사용 하 여 작업 사용자는 있는 accessories HomeKit. 만들고 방 유지 관리에 대 한 자세한 내용은 Apple의을 참조 하세요 [HMRoom](https://developer.apple.com/library/prerelease/ios/documentation/HomeKit/Reference/HMRoom_Class/index.html#//apple_ref/occ/cl/HMRoom) 설명서.
- **영역** -방 수 최종 사용자가 필요에 따라 영역으로 구성 하는 수입니다. 영역은 사용자는 하나의 단위로 처리할 수 있습니다 하는 대화방의 컬렉션을 가리킵니다. 예: Upstairs Downstairs 또는 지하실 합니다. 마찬가지로 HomeKit을 제공 하 여 최종 사용자에 게 가장 적합 한 방식으로 보조 프로그램을 사용 하 여 작업할 수 있습니다. 만들고 영역을 유지 관리에 대 한 자세한 내용은 Apple의을 참조 하세요 [HMZone](https://developer.apple.com/library/prerelease/ios/documentation/HomeKit/Reference/HMZone_Class/index.html#//apple_ref/occ/cl/HMZone) 설명서.
- **작업 및 작업 설정** -작업 액세서리 서비스 특성을 수정 하 고 집합으로 그룹화 할 수 있습니다. 작업 집합 보조 프로그램 그룹을 제어 하 고 해당 작업을 조정 하는 스크립트와 작동 합니다. 예를 들어, "조사식 TV" 스크립트를 수 있습니다는 블라인드, 조명 dim 닫고는 텔레비전과 해당 사운드 시스템을 켭니다. 만들기 및 작업 및 작업 집합을 유지 관리 하는 방법에 대 한 자세한 내용은 Apple의을 참조 하세요 [HMAction](https://developer.apple.com/library/prerelease/ios/documentation/HomeKit/Reference/HMAction_Class/index.html#//apple_ref/occ/cl/HMAction) 하 고 [HMActionSet](https://developer.apple.com/library/prerelease/ios/documentation/HomeKit/Reference/HMActionSet_Class/index.html#//apple_ref/occ/cl/HMActionSet) 설명서.
- **트리거** -하나 트리거를 활성화할 수 있습니다 또는 자세한 동작 설정할 때 지정된 된 조건 집합이 충족 합니다. 예를 들어 portch light 설정 하 고 외부 어두운 가져오면 모든 외부 도어가 잠금. 만들기 및 트리거를 유지 관리 하는 방법에 대 한 자세한 내용은 Apple의을 참조 하세요 [HMTrigger](https://developer.apple.com/library/prerelease/ios/documentation/HomeKit/Reference/HMTrigger_Class/index.html#//apple_ref/occ/cl/HMTrigger) 설명서.

다음 Apple로 쉽게 구현할 수 해야 하므로 위에서 설명한 동일한 기술을 사용 하는 이러한 기능을 [HomeKitDeveloper 가이드](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/HomeKitDeveloperGuide/Introduction/Introduction.html)하십시오 [HomeKit 사용자 인터페이스 지침](https://developer.apple.com/homekit/ui-guidelines/) 고 [ HomeKit 프레임 워크 참조](https://developer.apple.com/library/ios/home_kit_framework_ref)합니다.

## <a name="homekit-app-review-guidelines"></a>HomeKit 앱 검토 지침

ITunes 앱 스토어에에서 릴리스할 iTunes Connect에 설정 된 Xamarin.iOS 앱을 HomeKit를 제출 하기 전에 HomeKit 사용 앱에 대 한 Apple의 지침을 따르는 확인 합니다.

- 앱의 주 목적은 _해야_ HomeKit 프레임 워크를 사용 하는 경우 홈 자동화 되어야 합니다.
- 앱 마케팅 텍스트 HomeKit 사용 되 고 개인 정보 취급 방침을 제공 해야 합니다는 사용자에 알려야 합니다.
- 사용자 정보를 수집 하거나 HomeKit를 사용 하 여 광고를 위해 엄격 하 게 사용할 수 없습니다.

전체에 대 한 지침을 검토, Apple의를 참조 하세요 [앱 스토어 검토 지침](https://developer.apple.com/app-store/review/guidelines/)합니다.

## <a name="whats-new-in-ios-9"></a>새로운 iOS 9의 기능

Apple에에 대 한 변경 및 추가 HomeKit ios 9:

- **기존 개체를 유지 관리할** -홈 관리자는 기존 액세서리 수정 하는 경우 (`HMHomeManager`) 수정 된 특정 항목의 알려줍니다.
- **영구 식별자** -이제 모든 관련 HomeKit 클래스를 포함 한 `UniqueIdentifier` HomeKit에서 지정된 된 항목을 고유 하 게 식별 하는 속성이 앱 (또는 동일한 앱의 인스턴스)를 사용 합니다.
- **사용자 관리** -주 사용자의 홈 HomeKit 장치에 액세스할 수 있는 사용자를 통해 사용자 관리를 제공 하는 기본 제공 뷰 컨트롤러를 추가 합니다.
- **사용자 기능** -HomeKit 사용자에 게 이제 HomeKit를 사용 하는 기능을 제어 하는 권한 집합이 고 HomeKit 액세서리를 사용 하도록 설정 합니다. 앱만 현재 사용자에 게 관련 기능을 표시 됩니다. 예를 들어 한 관리자만 다른 사용자를 유지 하기 위해 수 있어야 합니다.
- **백그라운드에서 미리 정의 된** -평균 HomeKit 사용자에 대해 발생 하는 4 개의 일반적인 이벤트에 대 한 미리 정의 된 자동으로 만들었습니다. 시작, 유지, 반환, 침대 가십시오. 이러한 미리 정의 된 장면 집에서 삭제할 수 없습니다.
- **장면 및 Siri** -Siri iOS 9 및 수의 백그라운드에서 HomeKit에 정의 된 모든 장면 이름을 알 지원이 향상 되었습니다. 사용자는 해당 이름을 Siri 말하기 하면 장면을 실행할 수 있습니다.
- **보조 범주** -미리 정의 된 범주 집합을 모든 Accessories 및 홈에 추가 되 고 액세서리의 형식을 식별 하는 데 도움이 됩니다에 추가 되었거나 앱 내에서 작업 합니다. 이러한 새 범주는 액세서리 설치 하는 동안 사용할 수 있습니다.
- **Apple Watch 지원** -HomeKit watchOS에 대 한 출시 되었습니다. 및 Apple Watch 시계 근처 중인 iPhone 없이 HomeKit 사용 장치 컨트롤 수 있게 됩니다. WatchOS 용 HomeKit는 다음과 같은 기능을 지원합니다. 보기 집, 보조 프로그램을 제어 하 고 백그라운드에서 실행 합니다.
- **새 이벤트 트리거 유형은** -8, iOS 9 지원 이벤트 트리거 액세서리 상태 (예: 센서 데이터) 또는 지리적 위치에 따라 이제 iOS에서 지원 되는 타이머 형식 트리거 외에도 합니다. 이벤트 트리거를 사용 하 여 `NSPredicates` 실행을 위한 조건을 설정 합니다.
- **원격 액세스** -사용 하 여 원격 액세스, 사용자가 제어할 수 이제 해당 HomeKit 원격 위치에 있는 집와 멀리 떨어져 있을 때 Automation Accessories 홈 사용 하도록 설정 합니다. IOS 8에서에서 3 세대 집에서 Apple TV를 사용자가 경우에 지원 되었습니다이 합니다. Ios 9에서이 제한을 리프트 및 원격 액세스 iCloud 및 HomeKit 액세서리 프로토콜 (HAP)을 통해 지원 됩니다.
- **새 에너지 BLE (Bluetooth Low) 기능** -HomeKit는 이제 에너지 BLE (Bluetooth Low) 프로토콜을 통해 통신할 수 있는 더 많은 액세서리 형식을 지원 합니다. HAP Secure Tunneling 사용 HomeKit 액세서리 노출할 수 있습니다 다른 Bluetooth 액세서리 Wi-fi 상에 서 (범위를 벗어나므로 Bluetooth) 하는 경우. IOS 9, BLE Accessories에는 알림 및 메타 데이터에 대 한 완벽 하 게 지원 합니다.
- **새 액세서리 범주** -Apple iOS 9에서에서 다음 새 보조 범주를 추가 합니다. 창 Coverings, 고 문 및 Windows, 경보 시스템, 센서 및 프로그래밍 가능 스위치입니다.

HomeKit ios 9의 새로운 기능에 대 한 자세한 내용은 Apple의를 참조 하세요 [HomeKit 인덱스](https://developer.apple.com/homekit/) 하 고 [What's New in HomeKit](https://developer.apple.com/videos/wwdc/2015/?id=210) 비디오.

## <a name="summary"></a>요약

이 문서는 Apple HomeKit 가정 자동화 프레임 워크를 도입 했습니다. 설정 하 고 HomeKit 액세서리 시뮬레이터를 사용 하 여 테스트 장치를 구성 하는 방법 및 검색, 통신 및 HomeKit를 사용 하 여 홈 자동화 장치를 제어 하기 위한 간단한 Xamarin.iOS 앱을 만드는 방법에 알아보았습니다.



## <a name="related-links"></a>관련 링크

- [iOS 9 샘플](https://developer.xamarin.com/samples/ios/iOS9/)
- [개발자를 위한 iOS 9](https://developer.apple.com/ios/pre-release/)
- [새로운 iOS 9.0에서](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [HomeKitDeveloper 가이드](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/HomeKitDeveloperGuide/Introduction/Introduction.html)
- [HomeKit 사용자 인터페이스 지침](https://developer.apple.com/homekit/ui-guidelines/)
- [HomeKit 프레임 워크 참조](https://developer.apple.com/library/ios/home_kit_framework_ref)
