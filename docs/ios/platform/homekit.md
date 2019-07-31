---
title: Xamarin.ios의 HomeKit
description: HomeKit은 홈 automation 장치를 제어 하기 위한 Apple의 프레임 워크입니다. 이 문서에서는 HomeKit를 소개 하 고 HomeKit 액세서리 시뮬레이터에서 테스트 액세서리를 구성 하 고 이러한 액세서리와 상호 작용 하는 간단한 Xamarin.ios 앱을 작성 하는 과정을 소개 합니다.
ms.prod: xamarin
ms.assetid: 90C0C553-916B-46B1-AD52-1E7332792283
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/22/2017
ms.openlocfilehash: e8acec18785ff5017aa012a646f40f8a866070f8
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68656623"
---
# <a name="homekit-in-xamarinios"></a>Xamarin.ios의 HomeKit

_HomeKit은 홈 automation 장치를 제어 하기 위한 Apple의 프레임 워크입니다. 이 문서에서는 HomeKit를 소개 하 고 HomeKit 액세서리 시뮬레이터에서 테스트 액세서리를 구성 하 고 이러한 액세서리와 상호 작용 하는 간단한 Xamarin.ios 앱을 작성 하는 과정을 소개 합니다._

[![](homekit-images/accessory01.png "예제 HomeKit 사용 앱")](homekit-images/accessory01.png#lightbox)

Apple은 다양 한 공급 업체의 여러 홈 automation 장치를 일관 된 단일 단위로 원활 하 게 통합 하는 방법으로 iOS 8에 HomeKit를 도입 했습니다. HomeKit는 일반 프로토콜을 사용 하 여 홈 자동화 장치를 검색, 구성 및 제어 하기 위해 개별 공급 업체가 작업을 조정 하지 않고도 관련 공급 업체의 장치를 함께 사용할 수 있도록 합니다.

HomeKit를 사용 하면 공급 업체에서 제공 하는 Api 또는 앱을 사용 하지 않고 모든 HomeKit 사용 장치를 제어 하는 Xamarin.ios 앱을 만들 수 있습니다. HomeKit를 사용 하 여 다음을 수행할 수 있습니다.

- 새 HomeKit 사용 홈 자동화 장치를 검색 하 고 모든 사용자의 iOS 장치에서 유지 되는 데이터베이스에 추가 합니다.
- HomeKit _홈 구성 데이터베이스_에서 모든 장치를 설치, 구성, 표시 및 제어 합니다.
- 미리 구성 된 HomeKit 장치와 통신 하 고 명령을 사용 하 여 개별 작업을 수행 하거나 작업을 수행 하 여 부엌의 모든 조명을 켜는 등의 작업을 수행 합니다.

HomeKit 사용 앱에 대 한 홈 구성 데이터베이스의 장치를 제공 하는 것 외에도 HomeKit는 Siri 음성 명령에 대 한 액세스를 제공 합니다. 적절 하 게 구성 된 HomeKit 설정에 따라 사용자는 "Siri, 거실에서 조명 켜기"와 같은 음성 명령을 실행할 수 있습니다.

<a name="Home-Configuration-Database" />

## <a name="the-home-configuration-database"></a>홈 구성 데이터베이스

HomeKit는 지정 된 위치의 모든 자동화 장치를 홈 컬렉션으로 구성 합니다. 이 컬렉션은 사용자가 의미 있는 사람이 읽을 수 있는 레이블을 사용 하 여 자신의 홈 automation 장치를 논리적으로 정렬 된 단위로 그룹화 하는 방법을 제공 합니다.

홈 컬렉션은 모든 사용자의 iOS 장치에서 자동으로 백업 및 동기화 되는 홈 구성 데이터베이스에 저장 됩니다. HomeKit에서는 홈 구성 데이터베이스를 사용 하기 위한 다음 클래스를 제공 합니다.

- `HMHome`-이 컨테이너는 모든 홈 automation 장치에 대 한 모든 정보와 구성을 단일 물리적 위치 (예: 단일 패밀리 거주지). 사용자에 게 기본 집 및 휴가 집과 같은 둘 이상의 거주지가 있을 수 있습니다. 또는 본사와 같은 속성에 서로 다른 "집"이 있을 수 있습니다. 어느 쪽이 든, 하나 `HMHome` 이상의 개체를 설정 하 고 저장 _해야_ 다른 HomeKit 정보를 입력할 수 있습니다.
- `HMRoom`-선택적으로,를 `HMRoom` 사용 하면 사용자는 다음과 같이 홈 (`HMHome`) 내에서 특정 대화방을 정의할 수 있습니다. 부엌, 욕실, 중고품 또는 거실입니다. 사용자는 사내 `HMRoom` 에 있는 특정 위치에 있는 모든 홈 automation 장치를로 그룹화 하 고 하나의 단위로 작업을 수행할 수 있습니다. 예를 들어 Siri에 게 중고품 조명을 끄도록 요청 합니다.
- `HMAccessory`-사용자의 거주지 (예: 스마트 자동 온도 조절기)에 설치 된 개별 물리적 HomeKit 사용 가능 자동화 장치를 나타냅니다. 각 `HMAccessory` 은에 할당 `HMRoom`됩니다. 사용자가 대화방을 구성 하지 않은 경우 HomeKit는 특수 한 기본 방에 액세서리를 할당 합니다.
- `HMService`-지정 `HMAccessory`된에서 제공 하는 서비스를 나타냅니다 (예: 조명의 설정/해제 상태, 색 변경이 지원 되는 경우). 각 `HMAccessory` 에는 두 개 이상의 서비스 (예: 열기 포함 하는 중고품 도어)가 있을 수 있습니다. 또한 지정 `HMAccessory` 된에는 사용자 제어 외부에 있는 펌웨어 업데이트와 같은 서비스가 있을 수 있습니다.
- `HMZone`-사용자가 개체의 `HMRoom` 컬렉션을 논리 영역 (예: Upstairs, Downstairs 또는 지하실)으로 그룹화 할 수 있습니다. 선택 사항 이지만,이를 통해 Siri와 같은 상호 작용을 통해 모든 라이트 downstairs을 끌 수 있습니다.

<a name="Provisioning-a-HomeKit-App" />

## <a name="provisioning-a-homekit-app"></a>HomeKit 앱 프로 비전

HomeKit에 의해 적용 되는 보안 요구 사항으로 인해 HomeKit 프레임 워크를 사용 하는 Xamarin.ios 앱은 Apple 개발자 포털과 Xamarin.ios 프로젝트 파일 모두에서 제대로 구성 해야 합니다.

다음을 수행합니다.

1. [Apple 개발자 포털](https://developer.apple.com)에 로그인 합니다.
2. **인증서, 식별자 & 프로필**을 클릭 합니다.
3. 아직 수행 하지 않은 경우 **식별자** 를 클릭 하 고 앱에 대 한 id (예: `com.company.appname`)를 만든 다음 기존 id를 편집 합니다.
4. 지정 된 ID에 대해 **HomeKit** 서비스를 확인 했는지 확인 합니다. 

    [![](homekit-images/provision01.png "지정 된 ID에 대해 HomeKit 서비스를 사용 하도록 설정 합니다.")](homekit-images/provision01.png#lightbox)
5. 변경 내용을 저장합니다.
6. 프로 **비전 프로필** > **개발** 을 클릭 하 고 앱에 대 한 새 개발 프로 비전 프로필을 만듭니다. 

    [![](homekit-images/provision02.png "앱에 대 한 새 개발 프로 비전 프로필 만들기")](homekit-images/provision02.png#lightbox)
7. 새 프로 비전 프로필을 다운로드 하 고 설치 하거나 Xcode를 사용 하 여 프로필을 다운로드 하 고 설치 합니다.
8. Xamarin.ios 프로젝트 옵션을 편집 하 고 방금 만든 프로 비전 프로필을 사용 하 고 있는지 확인 합니다. 

    [![](homekit-images/provision03.png "프로 비전 프로필을 지금 만듦을 선택 합니다.")](homekit-images/provision03.png#lightbox)
9. 그런 다음 **info.plist** 파일을 편집 하 고 프로 비전 프로필을 만드는 데 사용 된 앱 ID를 사용 하 고 있는지 확인 합니다. 

    [![](homekit-images/provision04.png "앱 ID 설정")](homekit-images/provision04.png#lightbox)
10. 마지막으로 **info.plist** 파일을 편집 하 고 **HomeKit** 자격을 선택 했는지 확인 합니다. 

    [![](homekit-images/provision05.png "HomeKit 자격 사용")](homekit-images/provision05.png#lightbox)
11. 모든 파일의 변경 내용을 저장 합니다.

이러한 설정이 적용 되 면 응용 프로그램은 이제 HomeKit Framework Api에 액세스할 준비가 되었습니다. 프로 비전에 대 한 자세한 내용은 [장치 프로 비전](~/ios/get-started/installation/device-provisioning/index.md) 및 [앱 가이드 프로 비전](~/ios/get-started/installation/device-provisioning/index.md) 을 참조 하세요.

> [!IMPORTANT]
> HomeKit 사용 앱을 테스트 하려면 개발용으로 적절히 프로 비전 된 실제 iOS 장치가 필요 합니다. IOS 시뮬레이터에서 HomeKit를 테스트할 수 없습니다.

## <a name="the-homekit-accessory-simulator"></a>HomeKit 액세서리 시뮬레이터

물리적 장치를 사용 하지 않고도 모든 가능한 home automation 장치 및 서비스를 테스트 하는 방법을 제공 하기 위해 Apple은 _HomeKit 액세서리 시뮬레이터_를 만들었습니다. 이 시뮬레이터를 사용 하 여 가상 HomeKit 장치를 설정 하 고 구성할 수 있습니다.

### <a name="installing-the-simulator"></a>시뮬레이터 설치

Apple은 Xcode에서 별도 다운로드로 HomeKit 액세서리 시뮬레이터를 제공 하므로 계속 하기 전에 설치 해야 합니다.

다음을 수행합니다.

1. 웹 브라우저에서 [Apple 개발자를 위한 다운로드](https://developer.apple.com/download/more/?name=for%20Xcode) 를 방문 하세요.
2. **Xcode xxx 용 추가 도구** 를 다운로드 합니다 (여기서 xxx는 설치한 Xcode의 버전). 

    [![](homekit-images/simulator01.png "Xcode 용 추가 도구 다운로드")](homekit-images/simulator01.png#lightbox)
3. 디스크 이미지를 열고 **응용 프로그램** 디렉터리에 도구를 설치 합니다.

HomeKit 액세서리 시뮬레이터를 설치한 경우 테스트용 가상 액세서리를 만들 수 있습니다.

### <a name="creating-virtual-accessories"></a>가상 액세서리 만들기

HomeKit 액세서리 시뮬레이터를 시작 하 고 몇 가지 가상 액세서리를 만들려면 다음을 수행 합니다.

1. 응용 프로그램 폴더에서 HomeKit 액세서리 시뮬레이터를 시작 합니다. 

    [![](homekit-images/simulator02.png "HomeKit 액세서리 시뮬레이터")](homekit-images/simulator02.png#lightbox)
2. 단추를 **+** 클릭 하 고 **새 액세서리 ...** 를 선택 합니다. 

    [![](homekit-images/simulator03.png "새 액세서리 추가")](homekit-images/simulator03.png#lightbox)
3. 새 액세서리에 대 한 정보를 입력 하 고 **마침** 단추를 클릭 합니다. 

    [![](homekit-images/simulator04.png "새 액세서리에 대 한 정보를 입력 합니다.")](homekit-images/simulator04.png#lightbox)
4. **서비스 추가** 를 클릭 합니다. 단추를 클릭 하 고 드롭다운에서 서비스 유형을 선택 합니다. 

    [![](homekit-images/simulator05.png "드롭다운에서 서비스 유형 선택")](homekit-images/simulator05.png#lightbox)
5. 서비스의 **이름을** 입력 하 고 **마침** 단추를 클릭 합니다. 

    [![](homekit-images/simulator06.png "서비스 이름 입력")](homekit-images/simulator06.png#lightbox)
6. **특성 추가** 단추를 클릭 하 고 필요한 설정을 구성 하 여 서비스에 대 한 선택적 특성을 제공할 수 있습니다. 

    [![](homekit-images/simulator07.png "필요한 설정 구성")](homekit-images/simulator07.png#lightbox)
7. 위의 단계를 반복 하 여 HomeKit에서 지 원하는 가상 홈 자동화 장치 유형 중 하나를 만듭니다.

몇 가지 샘플 가상 HomeKit 액세서리를 만들고 구성 하면 이제 Xamarin.ios 앱에서 이러한 장치를 사용 하 고 제어할 수 있습니다.

## <a name="configuring-the-infoplist-file"></a>Info.plist 파일 구성

IOS 10 이상에 대 한 새로운 기능으로 개발자는 `NSHomeKitUsageDescription` `Info.plist` 앱의 파일에 키를 추가 하 고 앱이 사용자의 HomeKit 데이터베이스에 액세스 하는 이유를 선언 하는 문자열을 제공 해야 합니다. 이 문자열은 앱을 처음 실행할 때 사용자에 게 표시 됩니다.

[![](homekit-images/info01.png "HomeKit 권한 대화 상자")](homekit-images/info01.png#lightbox)

이 키를 설정 하려면 다음을 수행 합니다.

1. `Info.plist` **솔루션 탐색기** 파일을 두 번 클릭 하 여 편집용으로 엽니다.
2. 화면 아래쪽에서 **원본** 뷰로 전환 합니다.
3. 목록에 새 **항목** 을 추가 합니다.
4. 드롭다운 목록에서 **개인 정보-HomeKit 사용 설명**을 선택 합니다. 

    [![](homekit-images/info02.png "개인 정보-HomeKit 사용 설명 선택")](homekit-images/info02.png#lightbox)
5. 앱이 사용자의 HomeKit 데이터베이스에 액세스 하려는 이유에 대 한 설명을 입력 합니다. 

    [![](homekit-images/info03.png "설명 입력")](homekit-images/info03.png#lightbox)
6. 파일의 변경 내용을 저장합니다.

> [!IMPORTANT]
> 파일에 키를 `NSHomeKitUsageDescription` 설정 하지 않으면 iOS 10 이상에서 실행 될 때 오류 없이 앱이 자동으로 실패 하 게 됩니다 (런타임에 시스템에서 닫힘). `Info.plist`

## <a name="connecting-to-homekit"></a>HomeKit에 연결

HomeKit와 통신 하려면 xamarin.ios 앱이 먼저 `HMHomeManager` 클래스의 인스턴스를 인스턴스화해야 합니다. 홈 관리자는 HomeKit의 중앙 진입점으로, 사용 가능한 홈 목록을 제공 하 고, 해당 목록을 업데이트 하 고 유지 관리 하 고, 사용자의 _기본 홈_을 반환 합니다.

개체 `HMHome` 는 설치 된 모든 홈 automation 액세서리와 함께 포함 될 수 있는 모든 대화방, 그룹 또는 영역을 포함 하 여 홈에 대 한 모든 정보를 포함 합니다. HomeKit에서 작업을 수행 하려면 먼저 하나 `HMHome` 이상의를 만들어 기본 홈으로 할당 해야 합니다.

앱은 기본 홈이 있는지 확인 하 고, 기본 홈이 없는 경우 만들고 할당 하는 일을 담당 합니다.

### <a name="adding-a-home-manager"></a>홈 관리자 추가

Xamarin.ios 앱에 HomeKit 인식 기능을 추가 하려면 **AppDelegate.cs** 파일을 편집 하 여 편집 하 고 다음과 같이 표시 합니다.

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

응용 프로그램을 처음 실행할 때 사용자에 게 HomeKit 정보에 대 한 액세스를 허용할지 묻는 메시지가 표시 됩니다.

[![](homekit-images/home01.png "사용자가 자신의 HomeKit 정보에 액세스 하도록 허용할지 묻는 메시지가 표시 됩니다.")](homekit-images/home01.png#lightbox)

사용자가 **정상**으로 대답 하면 응용 프로그램은 해당 HomeKit 액세서리를 사용할 수 있게 됩니다. 그렇지 않으면 HomeKit에 대 한 모든 호출이 오류로 인해 실패 합니다.

홈 관리자를 사용 하는 경우 다음 응용 프로그램은 기본 홈이 구성 되어 있는지 확인 하 고 그렇지 않은 경우 사용자가이를 만들고 할당할 수 있는 방법을 제공 해야 합니다.

### <a name="accessing-the-primary-home"></a>기본 홈 액세스

위에서 설명한 것 처럼 HomeKit를 사용할 수 있으려면 먼저 기본 홈을 만들고 구성 해야 하며, 사용자가 기본 홈 (있는 경우)을 만들고 할당 하는 방법을 제공 하는 것은 앱의 책임입니다.

앱이 처음 시작 되거나 백그라운드에서 반환 되 면 `DidUpdateHomes` `HMHomeManager` 클래스의 이벤트를 모니터링 하 여 기본 홈이 있는지 확인 해야 합니다. 존재 하지 않는 경우 사용자가 만들 수 있는 인터페이스를 제공 해야 합니다.

다음 코드를 뷰 컨트롤러에 추가 하 여 기본 홈을 확인할 수 있습니다.

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

홈 관리자가 HomeKit `DidUpdateHomes` 에 연결 하면 이벤트가 발생 하 고 모든 기존 홈이 관리자의 홈 컬렉션에 로드 되며, 사용 가능한 경우 기본 홈이 로드 됩니다.

### <a name="adding-a-primary-home"></a>기본 홈 추가

의 속성이 이벤트 `null` 이후 인경우에는계속하기전에사용자가기본홈을만들고할당하는방법을제공해야합니다.`HMHomeManager` `PrimaryHome` `DidUpdateHomes`

일반적으로 앱은 사용자가 새 홈의 이름을 지정한 후 홈 관리자에 게 전달 되어 기본 홈으로 설정 됩니다. **HomeKitIntro** 샘플 앱의 경우, IOS 디자이너에서 모달 보기가 생성 되 고 앱의 주 인터페이스에서 `AddHomeSegue` segue에 의해 호출 됩니다.

사용자가 새 홈의 이름을 입력할 수 있는 텍스트 필드와 홈을 추가 하는 단추를 제공 합니다. 사용자가 **홈 추가** 단추를 누르면 홈 관리자를 호출 하 여 홈을 추가 합니다.

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

메서드 `AddHome` 는 새 홈을 만들어 지정 된 콜백 루틴으로 반환 하려고 합니다. 속성이이 아니면 `null`오류가 발생 하 여 사용자에 게 표시 되어야 합니다. `error` 가장 일반적인 오류는 고유한 홈 이름이 아니거나 홈 관리자가 HomeKit와 통신할 수 없는 경우에 발생 합니다.

Home이 성공적으로 만들어진 경우 `UpdatePrimaryHome` 메서드를 호출 하 여 새 홈을 기본 홈으로 설정 해야 합니다. `error` 속성이이 아닌 `null`경우에도 오류가 발생 하 여 사용자에 게 표시 되어야 합니다.

또한 홈 관리자의 `DidAddHome` 및 `DidRemoveHome` 이벤트를 모니터링 하 고 필요에 따라 앱의 사용자 인터페이스를 업데이트 해야 합니다.

> [!IMPORTANT]
> 위의 샘플 코드에 사용 되는 메서드는HomeKitIntro응용프로그램의도우미클래스로,iOS경고를보다쉽게사용할수있도록합니다.`AlertView.PresentOKAlert`


## <a name="finding-new-accessories"></a>새 액세서리 찾기

Home Manager에서 기본 홈을 정의 하거나 로드 한 후 xamarin.ios 앱은를 `HMAccessoryBrowser` 호출 하 여 새 홈 automation 액세서리를 찾고 홈에 추가할 수 있습니다.

메서드를 호출 하 여 완료 될 때 새 액세서리 `StopSearchingForNewAccessories` 및 메서드를 검색 하기 시작 합니다. `StartSearchingForNewAccessories`

> [!IMPORTANT]
> `StartSearchingForNewAccessories`배터리 수명 및 iOS 장치의 성능에 부정적인 영향을 주기 때문에 오랜 시간 동안 실행 하면 안 됩니다. Apple은 1 `StopSearchingForNewAccessories` 분 후에 호출 하거나, 사용자에 게 액세서리 UI 찾기가 표시 될 때만 검색을 제안 합니다.

이 `DidFindNewAccessory` 이벤트는 새 액세서리를 검색 하면 호출 되며 액세서리 브라우저의 `DiscoveredAccessories` 목록에 추가 됩니다.

이 `DiscoveredAccessories` 목록에는 HomeKit 사용 홈 `HMAccessory` 자동화 장치와 조명 또는 중고품 도어 제어와 같은 사용 가능한 서비스를 정의 하는 개체의 컬렉션이 포함 됩니다.

새 액세서리를 찾았으면 사용자에 게 표시 되 고이를 선택 하 여 홈에 추가할 수 있습니다. 예제:

[![](homekit-images/accessory01.png "새 액세서리 찾기")](homekit-images/accessory01.png#lightbox)

메서드를 `AddAccessory` 호출 하 여 선택한 액세서리를 홈의 컬렉션에 추가 합니다. 예를 들어:

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

속성이이 아니면 `null`오류가 발생 하 여 사용자에 게 표시 되어야 합니다. `err` 그렇지 않으면 사용자에 게 추가할 장치에 대 한 설치 코드를 입력 하 라는 메시지가 표시 됩니다.

[![](homekit-images/accessory02.png "추가할 장치의 설치 코드를 입력 하세요.")](homekit-images/accessory02.png#lightbox)

HomeKit 액세서리 시뮬레이터에서이 숫자는 **설치 코드** 필드 아래에서 찾을 수 있습니다.

[![](homekit-images/accessory03.png "HomeKit 액세서리 시뮬레이터의 설정 코드 필드")](homekit-images/accessory03.png#lightbox)

Real HomeKit 액세서리의 경우 설정 코드는 장치 자체, 제품 상자 또는 액세서리의 사용자 설명서에 있는 레이블에 인쇄 됩니다.

사용자가 홈 컬렉션에 추가한 후 `DidRemoveNewAccessory` 에는 액세서리 브라우저의 이벤트를 모니터링 하 고 사용자 인터페이스를 업데이트 하 여 사용 가능한 목록에서 보조 프로그램을 제거 해야 합니다.

## <a name="working-with-accessories"></a>액세서리 작업

기본 홈이 설정 되 고 보조 프로그램이 추가 되 면 사용할 사용자에 대 한 액세서리 목록 (및 필요에 따라 방)을 제공할 수 있습니다.

`HMRoom` 개체에는 지정 된 대화방 및 여기에 속한 모든 액세서리에 대 한 모든 정보가 포함 됩니다. 필요에 따라 대화방을 하나 이상의 영역으로 구성할 수 있습니다. 는 `HMZone` 지정 된 영역에 대 한 모든 정보와 해당 영역에 속하는 모든 대화방을 포함 합니다.

이 예에서는 작업을 간단 하 게 유지 하 고 홈의 액세서리로 직접 작업 하는 것이 좋습니다.

개체 `HMHome` 는 해당 `Accessories` 속성에서 사용자에 게 제공할 수 있는 할당 된 액세서리 목록을 포함 합니다. 예를 들어:

[![](homekit-images/accessory04.png "예제 액세서리")](homekit-images/accessory04.png#lightbox)

여기에서 사용자는 지정 된 액세서리를 선택 하 고 제공 하는 서비스를 사용할 수 있습니다.

## <a name="working-with-services"></a>서비스 작업

사용자가 지정 된 HomeKit 사용 홈 자동화 장치와 상호 작용 하는 경우 일반적으로 제공 하는 서비스를 통해 제공 됩니다. 클래스의 속성은 `Services` 장치에서 제공 하는 `HMService` 서비스를 정의 하는 개체의 컬렉션을 포함 합니다. `HMAccessory`

서비스는 광원, 온도 조절 장치, 중고품 도어, 스위치 또는 잠금과 같은 작업입니다. 일부 장치 (예: 중고품 도어 열기)는 두 개 이상의 서비스 (예: 조명을 열거나 닫는 기능)를 제공 합니다.

지정 된 액세서리에서 제공 하는 특정 서비스 외에도 각 액세서리에는 `Information Service` 이름, 제조업체, 모델 및 일련 번호와 같은 속성을 정의 하는가 포함 되어 있습니다.

### <a name="accessory-service-types"></a>액세서리 서비스 유형

열거를 `HMServiceType` 통해 사용할 수 있는 서비스 유형은 다음과 같습니다.

- **AccessoryInformation** -지정 된 액세서리 (홈 자동화 장치)에 대 한 정보를 제공 합니다.
- **AirQualitySensor** -공기 품질 센서를 정의 합니다.
- **배터리** -액세서리 배터리의 상태를 정의 합니다.
- **CarbonDioxideSensor** -참조 이산화탄소 센서를 정의 합니다.
- **CarbonMonoxideSensor** -참조 Monoxide 센서를 정의 합니다.
- 연락처 **센서-연락처** 센서 (예: 열거나 닫는 창)를 정의 합니다.
- **도어** -도어 상태 센서 (예: 열림 또는 닫힘)를 정의 합니다.
- **팬** -원격 제어 팬을 정의 합니다.
- **GarageDoorOpener** -중고품 도어 열기를 정의 합니다.
- **HumiditySensor** -습도 센서를 정의 합니다.
- **LeakSensor** -누출 센서 (예: 핫 워터, washing 컴퓨터)를 정의 합니다.
- **전구** -다른 액세서리의 일부인 독립 실행형 조명 또는 광원을 정의 합니다 (예: 중고품 도어 열기).
- **LightSensor** -라이트 센서를 정의 합니다.
- **Lockmanagement** -자동화 된 도어 잠금을 관리 하는 서비스를 정의 합니다.
- **Lockmechanism** -원격 제어 잠금 (예: 도어 잠금)을 정의 합니다.
- **Motionsensor** -동작 센서를 정의 합니다.
- **OccupancySensor** -선점 센서를 정의 합니다.
- **유출** -원격 제어 벽 유출를 정의 합니다.
- **Securitysystem** -홈 보안 시스템을 정의 합니다.
- **StatefulProgrammableSwitch** -트리거된 후 상태를 유지 하는 프로그래밍 가능 스위치를 정의 합니다 (예: flip 스위치).
- **StatelessProgrammableSwitch** -트리거된 후 초기 상태로 반환 되는 프로그래밍 가능 스위치를 정의 합니다 (예: 누름 단추).
- **SmokeSensor** -스모크 센서를 정의 합니다.
- **스위치** -표준 벽 스위치와 같은 on/off 스위치를 정의 합니다.
- **TemperatureSensor** -온도 센서를 정의 합니다.
- **자동 온도 조절기** -HVAC 시스템을 제어 하는 데 사용 되는 스마트 자동 온도 조절기 정의 합니다.
- **Window** -cane 원격으로 열리거나 닫힐 수 있는 자동화 된 창을 정의 합니다.
- **Windowcovering** -열거나 닫을 수 있는 블라인드와 같은 원격 제어 창을 정의 합니다.

### <a name="displaying-service-information"></a>서비스 정보 표시

을 `HMAccessory` 로드 한 후에는 제공 된 개별 `HNService` 개체를 쿼리하여 해당 정보를 사용자에 게 표시할 수 있습니다.

[![](homekit-images/accessory05.png "서비스 정보 표시")](homekit-images/accessory05.png#lightbox)

`Reachable` 에`HMAccessory` 대 한 작업을 시도 하기 전에 항상의 속성을 확인 해야 합니다. 사용자가 장치 범위 내에 없거나 연결 되지 않은 경우 액세서리에 연결할 수 없습니다.

서비스를 선택 하면 사용자가 해당 서비스의 하나 이상의 특성을 보거나 수정 하 여 지정 된 홈 automation 장치를 모니터링 하거나 제어할 수 있습니다.

<a name="Working-with-Characteristics" />

## <a name="working-with-characteristics"></a>특성 작업

각 `HMService` 개체에는 서비스의 상태 `HMCharacteristic` 에 대 한 정보를 제공 하거나 (예: 문 열림 또는 닫힘) 사용자가 상태를 조정할 수 있도록 허용 하는 개체의 컬렉션을 포함할 수 있습니다 (예: 광원 색 설정).

`HMCharacteristic`는 특성 및 해당 상태에 대 한 정보를 제공할 뿐 아니라 _특성 메타 데이터_ (`HMCharacteristisMetadata`)를 통해 상태를 사용 하는 메서드도 제공 합니다. 이 메타 데이터는 사용자에 게 정보를 표시 하거나 상태를 수정할 수 있도록 허용 하는 속성 (예: 최소 및 최대 값 범위)을 제공할 수 있습니다.

열거형 `HMCharacteristicType` 은 다음과 같이 정의 하거나 수정할 수 있는 특성 메타 데이터 값 집합을 제공 합니다.

- AdminOnlyAccess
- AirParticulateDensity
- AirParticulateSize
- 방송 품질
- 오디오 피드백
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

### <a name="working-with-a-characteristics-value"></a>특성 값 사용

앱이 지정 된 특성의 최신 상태를 갖도록 하려면 `ReadValue` `HMCharacteristic` 클래스의 메서드를 호출 합니다. `err` 속성이`null`이 아니면 오류가 발생 하 여 사용자에 게 표시 되거나 표시 되지 않을 수 있습니다.

특성 `Value` 의 속성은 지정 된 특성 `NSObject`의 현재 상태를로 포함 하며,이는에서 C#직접 작업할 수 없습니다.

값을 읽으려면 다음 도우미 클래스가 **HomeKitIntro** 샘플 응용 프로그램에 추가 되었습니다.

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

는 `NSObjectConverter` 응용 프로그램에서 특성의 현재 상태를 읽어야 할 때마다 사용 됩니다. 예를 들면 다음과 같습니다.

```csharp
var value = NSObjectConverter.ToFloat (characteristic.Value);
```

위의 줄은 값 `float` 을 Xamarin C# 코드에서 사용할 수 있는으로 변환 합니다.

를 수정 `HMCharacteristic`하려면 해당 `WriteValue` 메서드를 호출 하 고 `NSObject.FromObject` 호출에서 새 값을 래핑합니다. 예를 들면 다음과 같습니다.

```csharp
Characteristic.WriteValue(NSObject.FromObject(value),(err) =>{
    // Was there an error?
    if (err!=null) {
        // Yes, inform user
        AlertView.PresentOKAlert("Update Error",err.LocalizedDescription,Controller);
    }
});
```

속성이이 아니면 `null`오류가 발생 하 여 사용자에 게 표시 되어야 합니다. `err`

### <a name="testing-characteristic-value-changes"></a>특성 값 변경 테스트

및 시뮬레이션 된 `HMCharacteristics` 액세서리로 작업할 때 `Value` 속성에 대 한 수정은 HomeKit 액세서리 시뮬레이터 내에서 모니터링할 수 있습니다.

실제 iOS 장치 하드웨어에서 실행 되는 **HomeKitIntro** 앱을 사용 하 여 특성 값에 대 한 변경 내용은 HomeKit 액세서리 시뮬레이터에서 거의 즉시 표시 되어야 합니다. 예를 들어 iOS 앱에서 광원의 상태를 변경 합니다.

[![](homekit-images/test01.png "IOS 앱에서 광원의 상태 변경")](homekit-images/test01.png#lightbox)

HomeKit 액세서리 시뮬레이터에서 광원의 상태를 변경 해야 합니다. 값이 변경 되지 않으면 새 특성 값을 쓸 때 오류 메시지의 상태를 확인 하 고 해당 액세서리에 계속 연결할 수 있는지 확인 합니다.

## <a name="advanced-homekit-features"></a>고급 HomeKit 기능

이 문서에서는 Xamarin.ios 앱에서 HomeKit 액세서리를 사용 하는 데 필요한 기본 기능에 대해 설명 했습니다. 그러나이 소개에서 다루지 않는 HomeKit의 몇 가지 고급 기능이 있습니다.

- HomeKit 사용 액세서리는 필요에 따라 최종 사용자가 대화방으로 구성할 수 있습니다. 이를 통해 HomeKit는 사용자가 쉽게 이해 하 고 사용할 수 있는 방식으로 액세서리를 제공할 수 있습니다. 대화방을 만들고 유지 관리 하는 방법에 대 한 자세한 내용은 Apple의 [Hmroom](https://developer.apple.com/library/prerelease/ios/documentation/HomeKit/Reference/HMRoom_Class/index.html#//apple_ref/occ/cl/HMRoom) 설명서를 참조 하세요.
- **영역** -대화방은 최종 사용자가 선택적으로 영역으로 구성할 수 있습니다. 영역은 사용자가 단일 단위로 처리할 수 있는 대화방 컬렉션을 참조 합니다. 예: Upstairs, Downstairs 또는 지하실입니다. 이를 통해 HomeKit는 최종 사용자에 게 적합 한 방식으로 액세서리를 제공 하 고 작업을 수행할 수 있습니다. 영역을 만들고 유지 관리 하는 방법에 대 한 자세한 내용은 Apple의 [Hmzone](https://developer.apple.com/library/prerelease/ios/documentation/HomeKit/Reference/HMZone_Class/index.html#//apple_ref/occ/cl/HMZone) 설명서를 참조 하세요.
- 작업 **및 동작 집합** -작업은 액세서리 서비스 특성을 수정 하 고 집합으로 그룹화 할 수 있습니다. 작업은 프로그램 그룹을 제어 하 고 작업을 조정 하는 스크립트 역할을 합니다. 예를 들어 "TV 시청" 스크립트는 블라인드를 닫고 조명을 희미하게 하 고 텔레비전 및 사운드 시스템을 켤 수 있습니다. 작업 및 작업 집합을 만들고 유지 관리 하는 방법에 대 한 자세한 내용은 Apple의 [Hmaction](https://developer.apple.com/library/prerelease/ios/documentation/HomeKit/Reference/HMAction_Class/index.html#//apple_ref/occ/cl/HMAction) 및 [hmactionset](https://developer.apple.com/library/prerelease/ios/documentation/HomeKit/Reference/HMActionSet_Class/index.html#//apple_ref/occ/cl/HMActionSet) 설명서를 참조 하세요.
- **트리거** -지정 된 조건 집합이 충족 될 때 트리거는 하나 이상의 작업 집합을 활성화할 수 있습니다. 예를 들어, portch 라이트를 켜고 외부 문이 어두운 곳에 있을 때 모든 외부 도어를 잠급니다. 트리거를 만들고 유지 관리 하는 방법에 대 한 자세한 내용은 Apple의 [Hmtrigger](https://developer.apple.com/library/prerelease/ios/documentation/HomeKit/Reference/HMTrigger_Class/index.html#//apple_ref/occ/cl/HMTrigger) 설명서를 참조 하세요.

이러한 기능은 위에 나와 있는 것과 동일한 기법을 사용 하기 때문에 Apple의 [HomeKitDeveloper Guide](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/HomeKitDeveloperGuide/Introduction/Introduction.html), [HomeKit 사용자 인터페이스 지침](https://developer.apple.com/homekit/ui-guidelines/) 및 [HomeKit Framework 참조](https://developer.apple.com/library/ios/home_kit_framework_ref)를 사용 하 여 구현 하는 것이 쉽습니다.

## <a name="homekit-app-review-guidelines"></a>HomeKit 앱 검토 지침

ITunes 앱 스토어에서 HomeKit가 사용 하도록 설정 된 Xamarin.ios 앱을 iTunes Connect에 제출 하기 전에 HomeKit 사용 앱에 대 한 Apple의 지침을 따라야 합니다.

- HomeKit framework를 사용 하는 경우에는 앱의 기본 목적이 home automation 이어야 _합니다_ .
- 앱의 마케팅 텍스트는 HomeKit 사용 중임을 사용자에 게 알리고 개인 정보 취급 방침을 제공 해야 합니다.
- 사용자 정보를 수집 하거나 광고에 대해 HomeKit를 사용 하는 것은 엄격히 금지 됩니다.

전체 검토 지침은 Apple의 [앱 스토어 검토 지침](https://developer.apple.com/app-store/review/guidelines/)을 참조 하세요.

## <a name="whats-new-in-ios-9"></a>IOS 9의 새로운 기능

Apple에서 iOS 9에 대해 다음과 같이 HomeKit를 변경 하 고 추가 했습니다.

- **기존 개체 유지 관리** -기존 액세서리를 수정 하면 홈 관리자 (`HMHomeManager`)는 수정 된 특정 항목을 알려 줍니다.
- **영구 식별자** -모든 관련 HomeKit 클래스에는 HomeKit `UniqueIdentifier` 사용 앱 (또는 동일한 앱의 인스턴스)에서 지정 된 항목을 고유 하 게 식별 하는 속성이 포함 되어 있습니다.
- **사용자 관리** -기본 사용자의 홈에서 HomeKit 장치에 대 한 액세스 권한이 있는 사용자를 위한 사용자 관리를 제공 하는 기본 제공 뷰 컨트롤러를 추가 했습니다.
- **사용자 기능** -HomeKit 사용자는 이제 HomeKit 및 HomeKit enabled 액세서리에서 사용할 수 있는 기능을 제어 하는 권한 집합을 갖게 됩니다. 앱은 현재 사용자 에게만 관련 기능을 표시 해야 합니다. 예를 들어 관리자만 다른 사용자를 유지 관리할 수 있습니다.
- **미리 정의 된 장면** -평균 HomeKit 사용자에 대해 발생 하는 네 가지 일반적인 이벤트에 대해 미리 정의 된 장면을 만들었습니다. 다운로드, 유지, 반환, 평판으로 이동 합니다. 이러한 미리 정의 된 장면을 홈에서 삭제할 수 없습니다.
- **장면 및 siri** -siri는 iOS 9의 장면을 심층적으로 지원 하 고 HomeKit에 정의 된 장면의 이름을 인식할 수 있습니다. 사용자는 자신의 이름을 Siri로 말하기만 장면을 실행할 수 있습니다.
- **액세서리 범주** -미리 정의 된 범주 집합은 모든 액세서리에 추가 되었으며, 홈에 추가 되거나 앱 내에서 작동 하는 액세서리 유형을 식별 하는 데 도움이 됩니다. 이러한 새 범주는 액세서리를 설정 하는 동안 사용할 수 있습니다.
- **Apple Watch 지원** -이제 watchOS에서 HomeKit를 사용할 수 있으며, Apple Watch가 감시 근처에 있는 IPhone 없이 HomeKit 사용 가능 장치를 제어할 수 있습니다. WatchOS에 대 한 HomeKit는 다음과 같은 기능을 지원 합니다. 홈 보기, 보조 프로그램 제어 및 장면 실행
- **새 이벤트 트리거 유형** -ios 8에서 지원 되는 타이머 유형 트리거와 더불어 이제 ios 9에서 액세서리 상태 (예: 센서 데이터) 또는 지리적 위치를 기반으로 하는 이벤트 트리거를 지원 합니다. 이벤트 트리거는 `NSPredicates` 를 사용 하 여 실행 조건을 설정 합니다.
- 원격 **액세스** -원격 액세스를 사용 하면 사용자가 원격 위치에서 집 떨어져 있을 때 HomeKit 사용 홈 Automation 액세서리를 제어할 수 있습니다. IOS 8에서는 사용자가 집에서 3 세대 Apple TV를 사용 하는 경우에만 지원 됩니다. IOS 9에서는이 제한이 리프트 되며 원격 액세스는 iCloud 및 HAP (HomeKit 액세서리 프로토콜)를 통해 지원 됩니다.
- **새로운 블루투스 저 에너지 (HomeKit) 기능** -이제는 더 많은 액세서리 유형을 지원 합니다 .이를 통해 더 많은 액세서리 유형을 지원 합니다. HomeKit 액세서리는 HAP 보안 터널링을 사용 하 여 Wi-fi를 통해 다른 Bluetooth 액세서리를 노출할 수 있습니다 (Bluetooth 범위를 벗어난 경우). IOS 9에서 전체 액세서리는 알림 및 메타 데이터에 대 한 완전 한 지원을 제공 합니다.
- **새 액세서리 범주** -Apple은 iOS 9에서 다음과 같은 새로운 액세서리 범주를 추가 했습니다. 창 Coverings, Motorized 도어 및 Windows, 경보 시스템, 센서 및 프로그래밍 가능 스위치.

IOS 9에서 HomeKit의 새로운 기능에 대 한 자세한 내용은 Apple의 [HomeKit 인덱스](https://developer.apple.com/homekit/) 및 [HomeKit video의 새로운](https://developer.apple.com/videos/wwdc/2015/?id=210) 기능을 참조 하세요.

## <a name="summary"></a>요약

이 문서에는 Apple의 HomeKit home automation 프레임 워크가 도입 되었습니다. HomeKit 액세서리 시뮬레이터를 사용 하 여 테스트 장치를 설정 및 구성 하는 방법 및 HomeKit를 사용 하 여 홈 automation 장치를 검색 하 고, 통신 하 고, 제어 하는 간단한 Xamarin.ios 앱을 만드는 방법을 살펴보았습니다.



## <a name="related-links"></a>관련 링크

- [iOS 9 샘플](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS9)
- [개발자를 위한 iOS 9](https://developer.apple.com/ios/pre-release/)
- [IOS 9.0의 새로운 기능](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [HomeKitDeveloper 가이드](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/HomeKitDeveloperGuide/Introduction/Introduction.html)
- [HomeKit 사용자 인터페이스 지침](https://developer.apple.com/homekit/ui-guidelines/)
- [HomeKit Framework 참조](https://developer.apple.com/library/ios/home_kit_framework_ref)
