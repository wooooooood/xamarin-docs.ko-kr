---
title: Xamarin.iOS에서 HealthKit
description: 이 문서에서는 HealthKit, 상태 관련 정보에 대 한 중앙 집중화 하 고 조정 된 보안 데이터 저장소를 제공 하는 iOS 8에에서 도입 된 프레임 워크를 설명 합니다. HealthKit 앱 프로 비전 하는 방법 및 HealthKit 프레임 워크를 사용 하는 코드를 작성 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: E3927A21-507C-43BA-A2AD-957716BA9B52
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/19/2017
ms.openlocfilehash: df0b0e8dd57129917f2d8dab07115551ca675acf
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50109226"
---
# <a name="healthkit-in-xamarinios"></a>Xamarin.iOS에서 HealthKit

상태 키트는 사용자의 상태 관련 정보에 대 한 보안 데이터 저장소를 제공합니다. 상태 키트 앱 수 사용자의 명시적 사용 권한으로이 데이터 저장소에 쓸 읽고 관련 데이터가 추가 되 면 알림을 받습니다. 앱 데이터를 제공할 수 있습니다 또는 사용자의 모든 데이터의 대시보드 보기 Apple의 제공 된 상태 앱을 사용할 수 있습니다.

있기 때문에 상태와 관련 된 데이터 이므로 민감하고 중요 상태 키트 강력한 측정값과 정보 기록 되 고 (예를 들어 blood 혈당 수준 또는 심장 박동수) 형식의 명시적으로 연결의 단위를 사용 하 여 또한 상태 키트 앱 명시적 권한 부여를 사용 해야 합니다, 그리고 특정 종류의 정보 및 사용자 액세스를 요청 해야 명시적으로 부여 해야 해당 유형의 데이터에 대 한 앱 액세스 합니다.

이 문서를 소개 합니다.

- 응용 프로그램 프로 비전 하 고 사용자 상태 키트 데이터베이스 액세스 권한을 요청을 포함 하 여 상태 키트의 보안 요구 사항
- 잘못 적용 하거나 데이터를 잘못 가능성을 최소화 하는 상태 키트의 형식 시스템
- 공유 되는 시스템 차원의 상태 키트 데이터 저장소에 작성 합니다.

이 문서는 데이터베이스를 쿼리, 측정 단위 간의 변환, 새 데이터에 대 한 알림을 받는 등의 고급 항목을 다루지 않습니다.

이 문서에서는 우리가 만들 사용자의 심 박수를 기록 하려면 샘플 응용 프로그램:

[![](healthkit-images/image01.png "사용자 심 박수를 기록 하려면 샘플 응용 프로그램")](healthkit-images/image01.png#lightbox)

## <a name="requirements"></a>요구 사항

다음은이 기사에서 설명한 단계를 완료 하려면 필요 합니다.

- **Xcode 7 및 iOS 8 이상** – Apple의 최신 Xcode 및 iOS Api 설치 하 고 개발자의 컴퓨터에서 구성 해야 합니다.
- **Visual Studio 또는 Mac 용 visual Studio** – 최신 버전의 Mac 용 Visual Studio를 설치 하 고 개발자의 컴퓨터에 구성 해야 합니다.
- **iOS 8 (이상) 장치** – 8 이상이 테스트에 대 한 최신 버전의 iOS 실행 하는 iOS 장치.

> [!IMPORTANT]
> 상태 키트 iOS 8에서에서 도입 되었습니다. 현재 상태 키트에서 사용할 수 없는 iOS 시뮬레이터와 디버깅 하려면 실제 iOS 장치에 연결 해야 합니다.




## <a name="creating-and-provisioning-a-health-kit-app"></a>만들기 및 상태 키트 앱을 프로 비전
Xamarin iOS 8 응용 프로그램을 HealthKit API를 사용 하려면 먼저이 제대로 구성 하 고 프로 비전 합니다. 이 섹션에서는 제대로 Xamarin 응용 프로그램을 설치 하는 데 필요한 단계를 설명 합니다.

상태 키트 앱 필요합니다.

- 명시적인 **앱 ID**합니다.
- A **프로 비전 프로필** 관련 된 명시적 **앱 ID** 이며 **상태 키트** 권한.
- `Entitlements.plist` 사용 하 여는 `com.apple.developer.healthkit` 형식의 속성 `Boolean` 로 `Yes`합니다.
- `Info.plist` 해당 `UIRequiredDeviceCapabilities` 사용 하 여 항목을 포함 하는 키를 `String` 값 `healthkit`합니다.
- `Info.plist` 있어야 적절 한 개인 정보 보호 설명 항목:를 `String` 키에 대 한 설명을 `NSHealthUpdateUsageDescription` 앱 데이터를 작성 하려는 경우와 `String` 키에 대 한 설명을 `NSHealthShareUsageDescription` 앱 상태 키트 읽기 하려는 경우 데이터입니다.

IOS 앱 프로 비전 하는 방법에 대 한 자세한 내용을 알아보려면를 [Device Provisioning](~/ios/get-started/installation/device-provisioning/index.md) Xamarin의 문서 **Getting Started** 시리즈 개발자 인증서 앱 Id 간의 관계를 설명 합니다. 프로 비전 프로필 및 앱 자격입니다.

<a name="explicit-appid" />

### <a name="explicit-app-id-and-provisioning-profile"></a>명시적 앱 ID 및 프로 비전 프로필

명시적인 폭넓은 **앱 ID** 및 적절 한 **프로 비전 프로필** Apple 내에서 수행 됩니다 [iOS 개발자 센터](https://developer.apple.com/devcenter/ios/index.action)합니다. 

현재 **앱 Id** 내에 나열 된는 [인증서, 식별자 및 프로필](https://developer.apple.com/account/ios/identifiers/bundle/bundleList.action) 개발자 센터의 섹션입니다. 종종이 목록에 표시 됩니다 **ID** 의 값 `*`나타내는 합니다 **앱 ID** - **이름** 접미사를 개수에 관계 없이 사용할 수 있습니다. 이러한 *와일드 카드 앱 Id* 상태 키트를 사용 하 여 사용할 수 없습니다.
 
명시적인 만들려면 **앱 ID**를 클릭 합니다 **+** 로 이동 하려면 오른쪽 위에 있는 단추를 **iOS 앱 ID 등록** 페이지:


[![](healthkit-images/image02.png "Apple Developer 포털에서 앱 등록")](healthkit-images/image02.png#lightbox)

앱에 대 한 설명을 만든 후 위의 이미지에 표시 된 대로 사용 합니다 **명시적 앱 ID** 응용 프로그램에 대 한 ID를 만들려면 섹션입니다. 에 **App Services** 섹션 **상태 키트** 에 **서비스 사용** 섹션입니다.

완료 되 면 키를 눌러 합니다 **계속** 등록 단추를 **앱 ID** 계정의. 다시 이동 됩니다 합니다 **인증서, 식별자 및 프로필** 페이지입니다. 클릭 **프로 비전 프로필** 클릭 하 여 현재 프로 비전 프로필을 목록으로 이동 합니다 **+** 이동 하려면 오른쪽 위 모서리에 있는 단추는 **Add iOS 프로 비전 프로필** 페이지입니다. 선택 합니다 **iOS App Development** 옵션을 클릭 **계속** 페이지로 이동 합니다 **앱 ID 선택** 페이지입니다. 여기에서 명시적 선택 **앱 ID** 이전에 지정 합니다.


[![](healthkit-images/image03.png "명시적 앱 ID 선택")](healthkit-images/image03.png#lightbox)

클릭 **계속** 지정 하는 나머지 화면을 통해 회사 및 사용자 **개발자 인증서**, **장치**, 및 **이름** 이 대 한 **프로 비전 프로필**:

[![](healthkit-images/image04.png "프로 비전 프로필 생성")](healthkit-images/image04.png#lightbox)

클릭 **생성** 하 고 프로필의 생성을 기다립니다. 파일을 다운로드 하 고 Xcode에서 설치 하려면 두 번 클릭 합니다. 설치를 확인할 수 있습니다 **Xcode > 기본 설정 > 계정 > 자세히 보기...** 방금 설치 된 프로 비전 프로필을 표시 하 고 상태 키트 및 기타 특수 서비스에 대 한 아이콘이 있어야 해당 **자격** 행:

[![](healthkit-images/image05.png "Xcode에서 프로필 보기")](healthkit-images/image05.png#lightbox)

<a name="associating-appid" />

### <a name="associating-the-app-id-and-provisioning-profile-with-your-xamarinios-app"></a>앱 ID를 연결 하 고 Xamarin.iOS 앱을 사용 하 여 프로필을 프로 비전

생성 하 고 적절 한 설치 했으면 **프로 비전 프로필** 일반적으로 솔루션을 만들려면 Visual Studio에서 Mac 또는 Visual Studio에 대 한 시간에서 설명한 대로 것입니다. 키트에 대 한 액세스 상태는 모든 iOS에 사용할 수 있는 C# 또는 F# 프로젝트.

손으로 Xamarin iOS 8 프로젝트를 만드는 과정을 안내 하는 대신이 문서를 포함 하는 미리 작성 된 스토리 보드 및 코드에 연결 된 샘플 앱을 엽니다. 사용 상태 키트를 사용 하 여 샘플 앱을 연결할 **프로 비전 프로필**를 **Solution Pad**프로젝트를 마우스 오른쪽 단추로 클릭 하 고 표시 해당 **옵션** 대화 합니다. 으로 전환 합니다 **iOS 응용 프로그램** 패널 enter 명시적 **앱 ID** 앱의 이전에 만든 **번들 식별자**:

[![](healthkit-images/image06.png "명시적 앱 ID를 입력 합니다.")](healthkit-images/image06.png#lightbox)

이제 전환할 합니다 **iOS 번들 서명** 패널입니다. 에 최근에 설치 **프로 비전 프로필**를 명시적으로 해당 연결을 사용 하 여 **앱 ID**, 이제으로 사용할 수는 **프로 비전 프로필**:

[![](healthkit-images/image07.png "프로 비전 프로필 선택")](healthkit-images/image07.png#lightbox)

경우는 **프로 비전 프로필** 를 사용할 수 없는 다시 확인 합니다 **번들 식별자** 에 **iOS 응용 프로그램** 패널에 지정 된 비교를 **iOS 개발자 센터** 올바르며 합니다 **프로 비전 프로필** 되어 (**Xcode > 기본 설정 > 계정 > 자세히 보기...** ).

때 상태 키트 지원 **프로 비전 프로필** 는 선택한 클릭 **확인** 프로젝트 옵션 대화 상자를 닫습니다.

### <a name="entitlementsplist-and-infoplist-values"></a>Entitlements.plist 및 Info.plist 값

샘플 앱 포함을 `Entitlements.plist` 합니다 (필요한 상태 키트를 사용 하도록 앱 설정에 대 한) 파일을 모든 프로젝트 템플릿에 포함 되지 않은 합니다. 프로젝트 자격 포함 되어 있지 않으면, 프로젝트를 마우스 오른쪽 단추로 클릭, 선택 **파일 > 새 파일... > iOS > Entitlements.plist** 하나를 수동으로 추가 합니다.

궁극적으로 프로그램 `Entitlements.plist` 다음 키와 값 쌍이 있어야 합니다.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>com.apple.developer.HealthKit</key>
    <true/>
</dict>
</plist>

```

마찬가지로 `Info.plist` 앱의 값이 있어야 합니다.에 대 한 `healthkit` 연관 된 `UIRequiredDeviceCapabilities` 키:

```xml
<key>UIRequiredDeviceCapabilities</key>
<array>
<string>armv7</string>
    <string>healthkit</string>
</array>

```

이 문서를 사용 하 여 제공 된 샘플 응용 프로그램에는 미리 구성 된 `Entitlements.plist` 필요한 키를 모두 포함 합니다.

<a name="programming" />

## <a name="programming-health-kit"></a>프로그래밍 상태 키트

상태 키트 데이터 저장소는 앱 간에 공유 되는 개인, 사용자 고유의 데이터 저장소입니다. 따라서 중요 한 상태 정보 이기 때문에 사용자 데이터 액세스를 허용 하는 양의 단계를 수행 해야 합니다. 이 액세스 partial 일 수 있습니다 (쓰기 읽기를 제외한 다른 것을 제외한 데이터의 일부 형식에 대 한 액세스 등) 및 언제 든 지 취소할 수 있습니다. 상태 키트 응용 프로그램은 많은 사용자가 해당 상태 관련 정보를 저장 하는 방법에 대 한 망설 되도록 이해 방어적에 작성 되어야 합니다.

상태 키트 데이터가 제한 사과 유형을 지정 합니다. 이러한 형식이 엄격 하 게 정의 된: 혈 형식과 같은 일부 경우에 제공 되는 Apple 열거형의 특정 값으로 제한 (예: 그램, 칼로리, 리터)는 측정 단위는 크기가 결합할 다른 합니다. 호환 되는 측정 단위를 공유 하는 데이터도 구분 됩니다 해당 `HKObjectType`; 예를 들어, 형식 시스템 스토어로 잘못 된 시도 catch 합니다는 `HKQuantityTypeIdentifier.NumberOfTimesFallen` 예상 필드 값을 `HKQuantityTypeIdentifier.FlightsClimbed` 둘 다 사용 하는 경우에는` HKUnit.Count` 측정 단위입니다.

상태 키트 데이터 저장소에 저장할 유형은의 모든 서브 `HKObjectType`합니다. `HKCharacteristicType` 개체는 생물학적 성별, 혈 형식 및 날짜의 생년월일을 저장합니다. 보다 일반적 그러나는 `HKSampleType` 을 특정 시간이 나 시간 동안의 샘플링 되는 데이터를 나타내는 개체입니다. 

[![](healthkit-images/image08.png "HKSampleType 개체 차트")](healthkit-images/image08.png#lightbox)

`HKSampleType` 추상적 이므로 네 가지 구체적인 서브 클래스입니다. 현재 유형이 하나만 있는지 `HKCategoryType` 절전 분석 데이터. 상태 키트의 데이터 대부분은 형식의 `HKQuantityType` 에 해당 데이터를 저장 하 고 `HKQuantitySample` 친숙 한 팩터리 디자인 패턴을 사용 하 여 생성 된 개체:

[![](healthkit-images/image09.png "상태 키트의 데이터 대부분은 HKQuantityType 유형이 고 HKQuantitySample 개체에 데이터를 저장")](healthkit-images/image09.png#lightbox)

`HKQuantityType` 형식에서 범위 `HKQuantityTypeIdentifier.ActiveEnergyBurned` 에 `HKQuantityTypeIdentifier.StepCount`입니다. 

<a name="requesting-permission" />

### <a name="requesting-permission-from-the-user"></a>사용자 로부터 권한을 요청

최종 사용자가 앱 상태 키트 데이터를 쓰거나 읽을 수 있도록 양의 단계를 수행 해야 합니다. IOS 8 장치에 미리 설치 되는 상태 앱을 통해 수행 됩니다. 처음 상태 키트 앱이 실행 되는 시스템 제어 방식의 사용 하 여 사용자가 표시 됩니다 **상태 액세스** 대화 상자:

[![](healthkit-images/image10.png "사용자가 시스템 제어 방식의 상태 액세스 대화 상자가 표시 됩니다.")](healthkit-images/image10.png#lightbox)

사용자가 앱의 상태를 사용 하 여 권한을 변경할 수 나중 **원본** 대화 상자:

[![](healthkit-images/image11.png "사용자 상태 앱에 대 한 원본 대화 상자를 사용 하 여 권한을 변경할 수 있습니다.")](healthkit-images/image11.png#lightbox)

매우 중요 한 상태 정보 이므로 앱 개발자가 작성 해야 해당 프로그램 방어적, 사용 권한 거부 및 앱을 실행 하는 동안에 변경할 수는 예상 합니다. 가장 일반적인 관용구의 권한을 요청 하는 것은 `UIApplicationDelegate.OnActivated` 메서드 다음 적절 하 게 사용자 인터페이스를 수정 합니다.

### <a name="permissions-walkthrough"></a>사용 권한 연습

상태 키트 프로 비전 된 프로젝트를 열고는 `AppDelegate.cs` 파일입니다. 문 사용 `HealthKit`; 파일의 맨 위에 있는 합니다.


다음 코드는 관련 상태 키트 사용 권한:

```csharp
private HKHealthStore healthKitStore = new HKHealthStore ();

public override void OnActivated (UIApplication application)
{
        ValidateAuthorization ();
}

private void ValidateAuthorization ()
{
        var heartRateId = HKQuantityTypeIdentifierKey.HeartRate;
        var heartRateType = HKObjectType.GetQuantityType (heartRateId);
        var typesToWrite = new NSSet (new [] { heartRateType });
        var typesToRead = new NSSet ();
        healthKitStore.RequestAuthorizationToShare (
                typesToWrite, 
                typesToRead, 
                ReactToHealthCarePermissions);
}

void ReactToHealthCarePermissions (bool success, NSError error)
{
        var access = healthKitStore.GetAuthorizationStatus (HKObjectType.GetQuantityType (HKQuantityTypeIdentifierKey.HeartRate));
        if (access.HasFlag (HKAuthorizationStatus.SharingAuthorized)) {
                HeartRateModel.Instance.Enabled = true;
        } else {
                HeartRateModel.Instance.Enabled = false;
        }
}

```

이러한 메서드의 코드를 모두 수행할 수 있습니다 인라인 `OnActivated`, 샘플 앱은 별도 메서드를 사용 하 여 의도 명확 하 게 확인 합니다: `ValidateAuthorization()` 의 특정 쓰고 형식 (및 읽기, 앱을 원하는 경우)에 액세스를 요청 하는 데 필요한 단계는 및 `ReactToHealthCarePermissions()` 는 Health.app의 사용 권한 대화 상자를 사용 하 여 사용자가 상호 작용한 후 활성화 되는 콜백 합니다.

작업을 `ValidateAuthorization()` 집합을 만드는 방법은 `HKObjectTypes` 앱을 작성 하 고 해당 데이터를 업데이트 하려면 권한 부여를 요청 하 합니다. 샘플 앱에는 `HKObjectType` 키가 `KHQuantityTypeIdentifierKey.HeartRate`합니다. 이 형식 집합에 추가 됩니다 `typesToWrite`를 설정 하는 동안 `typesToRead` 비어 있습니다. 이러한 설정에 대 한 참조를 `ReactToHealthCarePermissions()` 콜백에 전달 됩니다 `HKHealthStore.RequestAuthorizationToShare()`합니다.

합니다 `ReactToHealthCarePermissions()` 사용자가 권한 대화 상자 상호 작용 하 고 두 가지 정보를 전달 하는 콜백이 호출 됩니다:는 `bool` 값을 `true` 사용자가 권한 대화 상자와는 상호작용하는경우`NSError`, null이 아닌 나타내는 몇 가지 종류의 사용 권한 대화 상자를 제공 하는 데 관련 된 오류입니다.

> [!IMPORTANT]
> 이 함수에 인수를 명확히 하기: 합니다 _성공_ 하 고 _오류_ 매개 변수를 사용자에 게 부여 상태 키트 데이터를 액세스할 수 있는 권한이 있는지 여부를 나타내지 않습니다! 사용자 데이터에 대 한 액세스를 허용 하는 기회가 제공 된만 나타냅니다.

앱에서 데이터에 액세스할 수 있는지 여부를 확인 하는 `HKHealthStore.GetAuthorizationStatus()` 사용, 전달 `HKQuantityTypeIdentifierKey.HeartRate`합니다. 반환 된 상태에 따라 앱을 사용 하거나 데이터를 입력 하는 기능을 사용 하지 않도록 설정 합니다. 액세스 거부를 처리 하기 위한 표준 사용자 환경은 없습니다 되며 옵션이 많이 있습니다. 예제 앱 상태에 설정 됩니다는 `HeartRateModel` 차례로 관련 이벤트를 발생 하는 단일 개체입니다.

## <a name="model-view-and-controller"></a>모델, 뷰 및 컨트롤러

검토 합니다 `HeartRateModel` singleton 개체를 열기를 `HeartRateModel.cs` 파일:

```csharp
using System;
using HealthKit;
using Foundation;

namespace HKWork
{
        public class GenericEventArgs<T> : EventArgs
        {
                public T Value { get; protected set; }
                public DateTime Time { get; protected set; }

                public GenericEventArgs (T value)
                {
                        this.Value = value;
                        Time = DateTime.Now;
                }
        }

        public delegate void GenericEventHandler<T> (object sender,GenericEventArgs<T> args);

        public sealed class HeartRateModel : NSObject
        {
                private static volatile HeartRateModel singleton;
                private static object syncRoot = new Object ();

                private HeartRateModel ()
                {
                }

                public static HeartRateModel Instance {
                        get {
                                //Double-check lazy initialization
                                if (singleton == null) {
                                        lock (syncRoot) {
                                                if (singleton == null) {
                                                        singleton = new HeartRateModel ();
                                                }
                                        }
                                }

                                return singleton;
                        }
                }

                private bool enabled = false;

                public event GenericEventHandler<bool> EnabledChanged;
                public event GenericEventHandler<String> ErrorMessageChanged;
                public event GenericEventHandler<Double> HeartRateStored;

                public bool Enabled { 
                        get { return enabled; }
                        set {
                                if (enabled != value) {
                                        enabled = value;
                                        InvokeOnMainThread(() => EnabledChanged (this, new GenericEventArgs<bool>(value)));
                                }
                        }
                }

                public void PermissionsError(string msg)
                {
                        Enabled = false;
                        InvokeOnMainThread(() => ErrorMessageChanged (this, new GenericEventArgs<string>(msg)));
                }

                //Converts its argument into a strongly-typed quantity representing the value in beats-per-minute
                public HKQuantity HeartRateInBeatsPerMinute(ushort beatsPerMinute)
                {
                        var heartRateUnitType = HKUnit.Count.UnitDividedBy (HKUnit.Minute);
                        var quantity = HKQuantity.FromQuantity (heartRateUnitType, beatsPerMinute);

                        return quantity;
                }
                        
                public void StoreHeartRate(HKQuantity quantity)
                {
                        var bpm = HKUnit.Count.UnitDividedBy (HKUnit.Minute);
                        //Confirm that the value passed in is of a valid type (can be converted to beats-per-minute)
                        if (! quantity.IsCompatible(bpm))
                        {
                                InvokeOnMainThread(() => ErrorMessageChanged(this, new GenericEventArgs<string> ("Units must be compatible with BPM")));
                        }

                        var heartRateId = HKQuantityTypeIdentifierKey.HeartRate;
                        var heartRateQuantityType = HKQuantityType.GetQuantityType (heartRateId);
                        var heartRateSample = HKQuantitySample.FromType (heartRateQuantityType, quantity, new NSDate (), new NSDate (), new HKMetadata());

                        using (var healthKitStore = new HKHealthStore ()) {
                                healthKitStore.SaveObject (heartRateSample, (success, error) => {
                                        InvokeOnMainThread (() => {
                                                if (success) {
                                                        HeartRateStored(this, new GenericEventArgs<Double>(quantity.GetDoubleValue(bpm)));
                                                } else {
                                                        ErrorMessageChanged(this, new GenericEventArgs<string>("Save failed"));
                                                }
                                                if (error != null) {
                                                        //If there's some kind of error, disable 
                                                        Enabled = false;
                                                        ErrorMessageChanged (this, new GenericEventArgs<string>(error.ToString()));
                                                }
                                        });
                                });
                        }
                }
        }
}

```

첫 번째 섹션에는 일반 이벤트 및 처리기를 만들기 위한 상용구 코드입니다. 처음 부분을 `HeartRateModel` 클래스는 스레드로부터 안전한 singleton 개체를 만들기 위한 상용구 이기도 합니다.

그런 다음 `HeartRateModel` 3 이벤트를 노출 합니다. 

- `EnabledChanged` -나타냅니다 심장 박동수 저장소를 사용 하거나 사용 하지 않도록 설정 (저장소 처음에 비활성화 되어 참고). 
- `ErrorMessageChanged` -이 샘플 앱에 대 한 매우 간단한 오류 처리 모델을 했습니다: 마지막 오류를 사용 하 여 문자열입니다. 
- `HeartRateStored` -심장 박동수 상태 키트 데이터베이스에 저장 될 때 발생 합니다.

통해 이루어지는 이러한 이벤트를 발생 될 때마다 `NSObject.InvokeOnMainThread()`, UI를 업데이트 하는 구독자를 허용 하는 합니다. 또는 백그라운드 스레드에서 발생 되는 이벤트 문서화 수 하 고 호환성을 보장 해야 해당 처리기에 남을 수 있습니다. 스레드 고려 사항은 권한 요청 등의 함수를 많은 비동기적 이며 해당 콜백을 주 스레드에서 실행 때문에 상태 키트 응용 프로그램에서 중요 합니다.

Heath 키트 특정 코드 `HeartRateModel` 두 함수는 `HeartRateInBeatsPerMinute()` 고 `StoreHeartRate()`입니다. 

`HeartRateInBeatsPerMinute()` 강력한 형식의 상태 키트 인수로 변환 `HKQuantity`합니다. 수량의 형식이 지정 된 `HKQuantityTypeIdentifierKey.HeartRate` 수량 단위 이며 `HKUnit.Count` 나눈 `HKUnit.Minute` (단위는 즉, *분 당 비트 수*). 

`StoreHeartRate()` 함수는 `HKQuantity` (샘플 앱에서 하나 만들어 `HeartRateInBeatsPerMinute()` ). 사용 데이터의 유효성을 검사 하는 `HKQuantity.IsCompatible()` 메서드를 반환 하는 `true` 개체의 단위를 인수에 대 한 단위로 변환할 수 있는 경우. 수량으로 만든 경우 `HeartRateInBeatsPerMinute()` 분명히 돌아갑니다 `true`, 것도 반환 `true` 수량으로 예를 들어, 생성 된 경우 *시간당 차로*합니다. 일반적으로 더 `HKQuantity.IsCompatible()` 질량 등을 유효성 검사에 사용할 수 거리 및 사용자 또는 장치 입력 수도 있고 하나 측정 시스템의 (예: 야드/파운드법 단위)를 표시 하지만 다른 시스템 (예: 메트릭 단위)에 저장 될 수 있는 에너지입니다. 

수량의 호환성을 검증 된 일단의 `HKQuantitySample.FromType()` 팩터리 메서드는 강력한 형식의 만드는 데 `heartRateSample` 개체입니다. `HKSample` 개체에는 시작 및 종료 날짜 즉각적인 정보에 대 한 이러한 값 동일 해야, 예와에서 같습니다. 또한이 샘플 모든 키-값 데이터를 설정 하지 않습니다 해당 `HKMetadata` 인수 이지만 센서 위치를 지정 하려면 다음 코드와 같은 코드를 사용할 수:

```csharp
var hkm = new HKMetadata();
hkm.HeartRateSensorLocation = HKHeartRateSensorLocation.Chest;

```

한 번의 `heartRateSample` 되었습니다 만들어지면 코드 새 연결을 만들 데이터베이스를 사용 하 여 블록입니다. 해당 블록 내에서 `HKHealthStore.SaveObject()` 메서드는 데이터베이스에 비동기 쓰기를 시도 합니다. 람다 식에 대 한 결과 호출 하거나 관련 이벤트를 트리거합니다 `HeartRateStored` 또는 `ErrorMessageChanged`합니다.

이제 모델 프로그래밍 된, 컨트롤러에서 모델의 상태를 반영 하는 방법을 확인 하려면 시간입니다. 엽니다는 `HKWorkViewController.cs` 파일입니다. 연결 하기만 하면 생성자는 `HeartRateModel` 단일 이벤트 처리 메서드를 (다시 인라인 람다 식 사용 하 여 수행할 수 있습니다 하지만 별도 메서드가 의도 좀 더 명확 하 게 만들):

```csharp
public HKWorkViewController (IntPtr handle) : base (handle)
{
     HeartRateModel.Instance.EnabledChanged += OnEnabledChanged;
     HeartRateModel.Instance.ErrorMessageChanged += OnErrorMessageChanged;
     HeartRateModel.Instance.HeartRateStored += OnHeartBeatStored;
}

```

관련 처리기는 다음과 같습니다.

```csharp
void OnEnabledChanged (object sender, GenericEventArgs<bool> args)
{
        StoreData.Enabled = args.Value;
        PermissionsLabel.Text = args.Value ? "Ready to record" : "Not authorized to store data.";
        PermissionsLabel.SizeToFit ();
}

void OnErrorMessageChanged (object sender, GenericEventArgs<string> args)
{
        PermissionsLabel.Text = args.Value;
}

void OnHeartBeatStored (object sender, GenericEventArgs<double> args)
{
        PermissionsLabel.Text = String.Format ("Stored {0} BPM", args.Value);
}

```

물론 단일 컨트롤러를 사용 하 여 응용 프로그램에서는 별도 모델 개체의 생성 및 제어 흐름에 대 한 이벤트의 사용 하지 않도록 할 것 이지만 모델 개체 사용 실제 응용 프로그램에 더 적합 합니다.

## <a name="running-the-sample-app"></a>샘플 앱 실행

IOS 시뮬레이터 상태 키트를 지원 하지 않습니다. 디버깅 하는 iOS 8을 실행 하는 물리적 장치에서 수행 되어야 합니다.

시스템에 제대로 프로 비전 iOS 8 개발 장치를 연결 합니다. Mac 용 Visual Studio에서 배포 대상으로 선택 하 고 메뉴에서 **실행 > 디버깅**합니다.

> [!IMPORTANT]
> 프로 비전과 관련 된 실수는이 시점에서 노출 됩니다. 오류를 해결 하려면 만들기 및 프로 비전 위의 상태 키트 앱 섹션을 검토 합니다. 구성 요소는: 
>
> - **iOS 개발자 센터** -명시적 앱 ID 및 상태 키트 프로 비전 프로필을 사용 하도록 설정 합니다. 
> - **프로젝트 옵션** -번들 식별자 (명시적 앱 ID) 및 프로 비전 프로필입니다.
> - **소스 코드** -Entitlements.plist & Info.plist

프로 비전을 제대로 설정 되어 있는지를 가정 응용 프로그램이 시작 됩니다. 에 도달 하면 해당 `OnActivated` 메서드 상태 키트 권한 부여를 요청 합니다. 처음으로 운영 체제에서 발견 한이 사용자는 다음 대화 상자를 사용 하 여 표시 됩니다.


[![](healthkit-images/image12.png "사용자가이 대화 상자를 사용 하 여 표시")](healthkit-images/image12.png#lightbox)

핵심 처리 속도 데이터를 업데이트 하려면 앱을 사용 하도록 설정 하 고 앱이 다시 나타납니다. `ReactToHealthCarePermissions` 콜백 비동기적으로 활성화 됩니다. 이렇게 하면를 `HeartRateModel’s` `Enabled` 에서 발생 하는 속성을 변경 하려면를 `EnabledChanged` 이벤트로 인해 발생 합니다 `HKPermissionsViewController.OnEnabledChanged()` 이벤트 처리기를 실행 하 고 수 있도록 하려면는 `StoreData` 단추. 다음 다이어그램은 시퀀스를 나타냅니다.


[![](healthkit-images/image13.png "이 다이어그램의 이벤트 순서를 보여 줍니다.")](healthkit-images/image13.png#lightbox)

키를 눌러 합니다 **레코드** 단추입니다. 이렇게 하면를 `StoreData_TouchUpInside()` 의 값을 구문 분석 하려고 하는 처리기를 실행 하려면를 `heartRate` 텍스트 필드에는 convert `HKQuantity` 앞서 설명한 것을 통해 `HeartRateModel.HeartRateInBeatsPerMinute()` 함수 및 해당 수량을 전달 `HeartRateModel.StoreHeartRate()`. 이전에 설명한 대로이 데이터를 저장 하려고 및 중 하나에서 발생 한 `HeartRateStored` 또는 `ErrorMessageChanged` 이벤트입니다.

두 번 클릭 합니다 **홈** 장치에서 단추 및 상태 앱을 엽니다. 클릭 합니다 **원본** 탭을 나열 하는 샘플 앱에 표시 됩니다. 선택 하 고 핵심 처리 속도 데이터를 업데이트할 수 있는 권한을 허용 하지 않습니다. 두 번 클릭 합니다 **홈** 단추 다시 전환 앱. 다시 한 번 `ReactToHealthCarePermissions()` 호출 될 하지만이 이번에 액세스가 거부 되어 서 합니다 **StoreData** 단추를 사용할 수 없게 됩니다 (참고 비동기적으로 발생 하 고 사용자 인터페이스 변경 최종 사용자에 게 표시 될 수 있습니다).

## <a name="advanced-topics"></a>고급 항목

상태 키트에서 데이터를 읽을 데이터베이스는 매우 비슷합니다 데이터 쓰기: 다른 사람이 요청 권한 부여에 액세스 하려고 하는 데이터 형식을 하나 지정 하 고 데이터의 호환 장치를 자동 변환 하 여 사용할 수는 부여 하는 경우 측정값입니다.

조건자 기반 쿼리와 관련 된 데이터가 업데이트 될 때 업데이트를 수행 하는 쿼리를 허용 하는 보다 복잡 한 쿼리 함수는 여러 가지가 있습니다. 

상태 키트 응용 프로그램 개발자는 Apple의 상태 키트 섹션을 검토 해야 [지침을 검토 하는 앱](https://developer.apple.com/app-store/review/guidelines/#healthkit)합니다.

보안 및 형식 시스템 모델을 파악 되 면 저장 하 고 공유 하는 상태 키트 데이터베이스에 데이터를 읽는 간단 합니다. 다양 한 상태 키트 내에서 함수를 비동기적으로 작동 하 고 응용 프로그램 개발자는 해당 프로그램을 적절 하 게 작성 해야 합니다.

이 문서를 작성할 당시 현재 있는지 상태 키트 Android 또는 Windows Phone와 동일한 기능이 없습니다.

## <a name="summary"></a>요약

이 문서의 상태 키트 응용 프로그램 저장을 허용 하는 방법을 살펴본, 검색 및 공유 상태 관련 정보를, 사용자 액세스 및이 데이터를 제어할 수 있는 표준 상태 앱을 제공 하는 동안. 

보았습니다 개인 정보 보호, 보안 및 데이터 무결성 상태 관련 정보에 대 한 문제를 재정의 하는 방법 및 응용 프로그램 관리 측면 (프로 비전), (상태 키트의 종류를 코딩 하는 복잡성에서 증가 상태 키트를 사용 하 여 앱 처리 해야 합니다. 시스템의 경우) 및 사용자 (사용 권한 시스템 대화 상자 및 상태 앱을 통해 사용자 컨트롤) 발생 합니다. 

마지막으로 상태 키트 스토어로 하트 비트 데이터를 쓰고 인식 비동기 디자인을 가진 포함 된 샘플 앱을 사용 하 여 상태 키트의 간단한 구현을 살펴보겠습니다 했습니다.

## <a name="related-links"></a>관련 링크

- [HKWork (샘플)](https://developer.xamarin.com/samples/monotouch/ios8/IntroToHealthKit/)
- [iOS 8 소개](~/ios/platform/introduction-to-ios8.md)
