---
title: HealthKit
description: "HealthKit 상태 관련 정보에 대 한 중앙 집중화 하 고, 통합, 안전한 데이터 저장소를 제공 하는 iOS 8에에서 도입 된 프레임 워크입니다. 운영 체제는 개인 정보 및 상태 정보 및 보안, 상태 앱을 사용자에 대 한 대시보드를 확인합니다. 사용자의 권한이 있는 응용 프로그램 읽고 다양 한 상태 정보를 쓸 수 있습니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: E3927A21-507C-43BA-A2AD-957716BA9B52
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: e7075b67db94b6bf603bd96c637c9f7724ae1519
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/22/2018
---
# <a name="healthkit"></a>HealthKit

_HealthKit 상태 관련 정보에 대 한 중앙 집중화 하 고, 통합, 안전한 데이터 저장소를 제공 하는 iOS 8에에서 도입 된 프레임 워크입니다. 운영 체제는 개인 정보 및 상태 정보 및 보안, 상태 앱을 사용자에 대 한 대시보드를 확인합니다. 사용자의 권한이 있는 응용 프로그램 읽고 다양 한 상태 정보를 쓸 수 있습니다._

상태 키트는 사용자의 상태 관련 정보에 대 한 보안 데이터 저장소를 제공합니다. 상태 키트 앱을 사용자의 명시적 사용 권한을 가진이 데이터 저장소에 선택한 관련 데이터를 추가할 때 알림이 제공 합니다. 앱에서 데이터를 제공할 수 또는 사용자의 사용 상태 응용 프로그램을 제공된 하는 Apple의 모든 데이터의 대시보드를 볼 수 있습니다.

상태 관련 데이터는 매우 중요 한 고 중요 하며 상태 키트는 강력한 형식 측정값과 명시적으로 연결 되 고 기록 되는 정보 (예를 들어, 헌 혈당 수준 또는 심 박수)의 형식과 단위입니다. 또한 상태 키트 앱 명시적 자격을 사용 해야 합니다, 그리고 특정 종류의 정보 및 사용자 액세스 권한을 요청 해야 명시적으로 부여 해야 해당 유형의 데이터에 대 한 앱 액세스 합니다.

이 문서를 소개 합니다.

- 응용 프로그램 프로 비전 하 고 상태 키트 데이터베이스;에 액세스할 수 있는 권한을 사용자 요청을 포함 하 여 상태 키트의 보안 요구 사항
- 잘못 적용 하거나 데이터를 잘못 해석의 가능성을 최소화 하는 상태 키트의 형식 시스템
- 공유, 시스템 수준 상태 키트 데이터 저장소에 기록 됩니다.

이 문서는 데이터베이스를 쿼리, 측정 단위 간의 변환 또는 새 데이터에 대 한 알림을 받는 같은 더 고급 항목을 설명 합니다.

이 문서에서는 사용자의 심 박수 기록 하는 예제 응용 프로그램 만드는데:

[![](healthkit-images/image01.png "사용자가 심 박수 기록 하는 예제 응용 프로그램")](healthkit-images/image01.png#lightbox)

## <a name="requirements"></a>요구 사항

다음은이 문서에 제공 된 단계를 완료 하려면 필요 합니다.

- **Xcode 7 및 iOS 8 (또는 그 이상)** – Apple의 최신 Xcode 및 iOS Api 설치 하 고 개발자의 컴퓨터에 구성 해야 합니다.
- **Mac 또는 Visual Studio 용 visual Studio** -최신 버전의 Mac 용 Visual Studio를 설치 하 고 개발자의 컴퓨터에 구성 해야 합니다.
- **iOS 8 (또는 그 이상) 장치** – iOS 장치 테스트를 위해 8 이상에서 최신 버전의 iOS 실행 합니다.

> [!IMPORTANT]
> 상태 키트 iOS 8에에서 도입 되었습니다. 현재, 상태 키트에서 사용할 수 없으면 iOS 시뮬레이터와 연결 된 실제 iOS 장치에 필요한 디버깅 합니다.




## <a name="creating-and-provisioning-a-health-kit-app"></a>만들고 상태 키트 응용 프로그램을 프로 비전
Xamarin iOS 8 응용 프로그램 HealthKit API를 사용 하려면 먼저 그 해야 올바르게 구성 하 고 사용자를 프로 비전 합니다. 이 섹션은 올바른 Xamarin 응용 프로그램을 설정 하는 데 필요한 단계를 설명 합니다.

상태 키트 앱이 필요 합니다.

- 명시적 **앱 ID**합니다.
- A **프로 비전 프로필** 와 연결 된 명시적 **앱 ID** 와 **상태 키트** 사용 권한.
- `Entitlements.plist` 와 `com.apple.developer.healthkit` 형식의 속성이 `Boolean` 로 설정 `Yes`합니다.
- `Info.plist` 인 `UIRequiredDeviceCapabilities` 된 항목을 포함 하는 키의 `String` 값 `healthkit`합니다.
- `Info.plist` 있어야 적절 한 개인 정보 보호 설명 항목:는 `String` 키에 대 한 설명은 `NSHealthUpdateUsageDescription` 응용 프로그램 데이터를 작성 하려는 경우와 `String` 키에 대 한 설명은 `NSHealthShareUsageDescription` 응용 프로그램 상태 키트 읽을 예정인 경우 데이터입니다.

IOS 앱을 프로 비전 하는 방법에 대 한 자세한 내용을 [장치 프로 비전](~/ios/get-started/installation/device-provisioning/index.md) Xamarin의 문서를 **시작** 시리즈 개발자 인증서를 App Id 간의 관계를 설명 합니다. 프로비저닝 프로필 및 앱 자격입니다.

<a name="explicit-appid" />

### <a name="explicit-app-id-and-provisioning-profile"></a>명시적 앱 ID 및 프로비저닝 프로필

명시적 만들기 **앱 ID** 및 적절 한 **프로 비전 프로필** Apple의 내에서 [iOS Dev Center](https://developer.apple.com/devcenter/ios/index.action)합니다. 

현재 **의 앱 Id** 내에 나열 된는 [인증서, 식별자 및 프로필](https://developer.apple.com/account/ios/identifiers/bundle/bundleList.action) 개발자 센터의 섹션입니다. 종종이 목록에 표시 됩니다 **ID** 값 `*`한다는 표시 이므로 하는 **앱 ID** - **이름** 접미사를 개수에 관계 없이 함께 사용할 수 있습니다. 이러한 *와일드 카드의 앱 Id* 키트 상태와 함께 사용할 수 없습니다.
 
만들려면 명시적 **앱 ID**, 클릭는  **+**  를 이동 하는 데 오른쪽 위 단추는 **iOS 앱 ID를 등록** 페이지:


[![](healthkit-images/image02.png "Apple 개발자 포털에 앱 등록")](healthkit-images/image02.png#lightbox)

사용 하 여 응용 프로그램 설명을 만든 후 위의 그림에 표시 된 것 처럼는 **명시적 앱 ID** 응용 프로그램에 대 한 ID를 만들려면 섹션. 에 **응용 프로그램 서비스** 확인 섹션 **상태 키트** 에 **서비스 사용** 섹션.

완료 되 면 키를 눌러는 **계속** 등록 하는 단추는 **앱 ID** 계정에서 합니다. 다시 이동 됩니다는 **인증서, 식별자 및 프로필** 페이지. 클릭 **프로 비전 프로필** 현재 프로 비전 프로필을 목록으로 이동 하 고 클릭 하 고  **+**  를 이동 하는 데 오른쪽 위 모퉁이의 단추는 **iOS 추가 프로비저닝 프로필** 페이지. 선택의 **iOS 응용 프로그램 개발** 옵션 **계속** 를 시작 하는 **응용 프로그램 ID 선택** 페이지. 여기에서 명시적 선택 **앱 ID** 이전에 지정 합니다.


[![](healthkit-images/image03.png "명시적 응용 프로그램 ID 선택")](healthkit-images/image03.png#lightbox)

클릭 **계속** 지정 하 게 됩니다 나머지 화면을 통해 회사 및 사용자 **개발자 인증서**, **장치**, 및 **이름** 이 **프로비저닝 프로필**:

[![](healthkit-images/image04.png "프로비저닝 프로필을 생성합니다.")](healthkit-images/image04.png#lightbox)

클릭 **생성** 하 고 프로필의 생성을 기다립니다. 파일을 다운로드 하 고 설치 Xcode에서 두 번 클릭 합니다. 설치를 확인할 수 있습니다 **Xcode > 기본 설정 > 계정 > 자세히 보기...** 방금 설치 된 프로비저닝 프로필을 표시 되어야 하 고 상태 키트 및의 특별 한 다른 서비스에 대 한 아이콘이 있어야 해당 **자격** 행:

[![](healthkit-images/image05.png "Xcode에서 프로필 보기")](healthkit-images/image05.png#lightbox)

<a name="associating-appid" />

### <a name="associating-the-app-id-and-provisioning-profile-with-your-xamarinios-app"></a>앱 ID를 연결 하 고 프로 비전 프로필 Xamarin.iOS 앱과 함께

생성 하 고 적절 한 설치 후 **프로 비전 프로필** 설명한 것 처럼는 일반적으로 것 시간 Mac 또는 Visual Studio 용 Visual Studio에서 솔루션을 만들 수 있습니다. 상태 키트 액세스는 모든 iOS C# 또는 F # 프로젝트에 사용할 수 있습니다.

Xamarin iOS 8 프로젝트를 직접 만드는 과정을 안내 하는 대신 (미리 작성 된 스토리 보드 및 코드 포함)이 표시 된이 문서에 연결 된 샘플 앱을 엽니다. 샘플 응용 프로그램을 사용 하 여 상태 키트 연관 시킬 **프로 비전 프로필**에 **솔루션 패드**에서 프로젝트를 마우스 오른쪽 단추로 클릭 (를) 실행의 **옵션** 대화 상자. 전환 하는 **iOS 응용 프로그램** 패널로 끌어다 명시적 입력 **앱 ID** 으로 응용 프로그램의 이전에 만든 **번들 식별자**:

[![](healthkit-images/image06.png "명시적 앱 ID를 입력 합니다.")](healthkit-images/image06.png#lightbox)

이제 전환할는 **iOS 번들 서명** 패널입니다. 하면 최근에 설치 **프로 비전 프로필**, 연결을 명시적으로 **앱 ID**, 이제으로 사용할 수는 **프로 비전 프로필**:

[![](healthkit-images/image07.png "프로비저닝 프로필을 선택 합니다.")](healthkit-images/image07.png#lightbox)

경우는 **프로 비전 프로필** 사용할 수 없거나, 한 번 더 확인는 **번들 식별자** 에 **iOS 응용 프로그램** 패널에 지정 된 비교는 **iOS 개발자 센터** 하 고는 **프로 비전 프로필** 설치 (**Xcode > 기본 설정 > 계정 > 정보 보기...** ).

때 상태 키트 사용 **프로 비전 프로필** 가 선택한 상태에서 **확인** 프로젝트 옵션 대화 상자를 닫습니다.

### <a name="entitlementsplist-and-infoplist-values"></a>Entitlements.plist 및 Info.plist 값

샘플 응용 프로그램에 포함 되어는 `Entitlements.plist` 파일 (필요한 상태 키트 앱에 대 한), 모든 프로젝트 서식 파일에 포함 되지 않은 합니다. 프로젝트를 마우스 오른쪽 단추로 클릭 프로젝트 자격을 포함 하지 않는 경우, 선택 **파일 > 새 파일... > iOS > Entitlements.plist** 을 수동으로 추가 합니다.

궁극적으로, 프로그램 `Entitlements.plist` 다음 키와 값 쌍이 있어야 합니다.

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

마찬가지로,는 `Info.plist` 응용 프로그램의 값을가지고 있어야 `healthkit` 연관 된 `UIRequiredDeviceCapabilities` 키:

```xml
<key>UIRequiredDeviceCapabilities</key>
<array>
<string>armv7</string>
    <string>healthkit</string>
</array>

```

이 문서와 함께 제공 되는 샘플 응용 프로그램에는 미리 구성 된 `Entitlements.plist` 필요한 키를 모두 포함 하는 합니다.

<a name="programming" />

## <a name="programming-health-kit"></a>프로그래밍 상태 키트

상태 키트 데이터 저장소 앱 간에 공유 되는 개인, 사용자 고유의 데이터 저장소가입니다. 상태 정보가 너무 중요 하기 때문에 사용자 데이터 액세스를 허용 하도록 양의 단계를 수행 해야 합니다. 이 액세스 partial 일 수 있습니다 (쓰기 읽기를 제외한 다른 노드를 제외한 데이터의 일부 형식에 대 한 액세스 등) 하 고 언제 든 지 취소 될 수 있습니다. 많은 사용자가 자신의 상태 관련 정보를 저장 하는 방법에 대 한 망설 됩니다 이해를 바탕으로 상태 키트 응용 프로그램을 작성할, 작성 되어야 합니다.

상태 키트 데이터로 제한 됩니다. Apple 유형을 지정 합니다. 이러한 형식은 엄격 하 게 정의 되어: 다른 사용 권한과 결합할 크기 (예: 그램, 칼로리, 리터)는 측정 단위의 중 피 형식과 같은 일부는 제공 된 Apple 열거형의 특정 값으로 제한 합니다. 호환 되는 측정 단위를 공유 하는 데이터에 의해 구별 됩니다 자신의 `HKObjectType`; 예를 들어, 형식 시스템 잘못된 저장 하려고 catch는 `HKQuantityTypeIdentifier.NumberOfTimesFallen` 를 예상 하는 필드 값은 `HKQuantityTypeIdentifier.FlightsClimbed` 둘 다 사용 하는 경우에는` HKUnit.Count` 측정 단위입니다.

상태 키트 데이터 저장소에 저장 가능한 종류의 모든 하위 클래스는 `HKObjectType`합니다. `HKCharacteristicType` 개체는 생물학적 성별, 피 유형 및 Birth Date를 저장합니다. 가장 일반적인 모두 이지만, `HKSampleType` 개체를 특정 시간이 나 시간 동안 샘플링 되는 데이터를 나타냅니다. 

[![](healthkit-images/image08.png "HKSampleType 개체 차트")](healthkit-images/image08.png#lightbox)

`HKSampleType` 추상 이며 4 개의 구체적인 하위 클래스가 있습니다. 한 가지 유형의 현재는 `HKCategoryType` 절전 분석 된 데이터입니다. 큰 대부분 상태 키트에는 데이터의 경우 형식의 `HKQuantityType` 에 데이터 저장 및 `HKQuantitySample` 친숙 한 팩터리 디자인 패턴을 사용 하 여 생성 되는 개체:

[![](healthkit-images/image09.png "대다수 상태 키트에는 데이터의가 HKQuantityType 형식이 고 HKQuantitySample 개체에서 데이터 저장")](healthkit-images/image09.png#lightbox)

`HKQuantityType` 형식에서 범위 `HKQuantityTypeIdentifier.ActiveEnergyBurned` 를 `HKQuantityTypeIdentifier.StepCount`합니다. 

<a name="requesting-permission" />

### <a name="requesting-permission-from-the-user"></a>사용자 로부터 권한을 요청 하 고

최종 사용자가 앱을 읽거나 쓰는 상태 키트 데이터를 양의 단계를 수행 해야 합니다. 이 iOS 8 장치에 미리 설치 되어 나오는 상태 앱을 통해 수행 됩니다. 상태 키트 앱이 실행 되 면 처음으로 사용자가 표시 시스템 제어 **상태 액세스** 대화 상자:

[![](healthkit-images/image10.png "사용자가 시스템 제어 상태 액세스 대화 상자 표시")](healthkit-images/image10.png#lightbox)

사용자 상태 응용 프로그램을 사용 하 여 권한을 변경할 수 나중 **소스** 대화 상자:

[![](healthkit-images/image11.png "사용자가 앱 원본 대화 상자를 사용 하 여 권한을 변경할 수 있습니다.")](healthkit-images/image11.png#lightbox)

상태 정보는 매우 민감하며에 응용 프로그램 개발자 써야 프로그램 방어적, 사용 권한 거부 및는 응용 프로그램을 실행 하는 동안에 변경할 수는 예상 합니다. 가장 일반적인 요소에 대 한 사용 권한을 요청 하는 것은 `UIApplicationDelegate.OnActivated` 메서드 다음 적절 하 게 사용자 인터페이스를 수정 합니다.

### <a name="permissions-walkthrough"></a>사용 권한 연습

상태 키트 구성 된 프로젝트를 열고는 `AppDelegate.cs` 파일입니다. 문을 사용 하면 `HealthKit`; 파일의 맨 위쪽에 있습니다.


다음 코드와 상태 키트 사용 권한 관련:

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

이러한 메서드에 코드를 모두 수행할 수 있었습니다 인라인 `OnActivated`, 샘플 응용 프로그램은 별도 메서드로 더 명확 하 게 의도를 사용 하 여 하지만: `ValidateAuthorization()` 의 특정 쓰여지는 유형 (및 읽기, 앱을 원하는 경우)에 액세스 권한을 요청 하는 데 필요한 단계는 및 `ReactToHealthCarePermissions()` 는 사용자가는 Health.app의 사용 권한 대화 상자와 상호 작용한 후 활성화 되는 콜백입니다.

작업 `ValidateAuthorization()` 의 집합을 작성 하는 것 `HKObjectTypes` 앱 작성 되며 해당 데이터를 업데이트 하는 권한 부여를 요청 합니다. 샘플 앱에는 `HKObjectType` 은 키 용 `KHQuantityTypeIdentifierKey.HeartRate`합니다. 이 형식은 집합에 추가 됩니다 `typesToWrite`, 집합 동안 `typesToRead` 은 비어 있습니다. 이러한 집합 및에 대 한 참조는 `ReactToHealthCarePermissions()` 콜백에 전달 됩니다 `HKHealthStore.RequestAuthorizationToShare()`합니다.

`ReactToHealthCarePermissions()` 사용자 사용 권한 대화 상자와 상호 작용에 하 고 두 가지 정보를 전달 된 후 콜백이 호출 되:는 `bool` 될 값 `true` 사용자가 사용 권한 대화 상자는 와상호작용할경우`NSError`, null이 아닌 나타내는 몇 가지 종류의 사용 권한 대화 상자를 제공 하는 데 관련 된 오류입니다.

> [!IMPORTANT]
> 이 함수에 대 한 인수에 대 한 명확 하 게:는 _성공_ 및 _오류_ 매개 변수는 사용자에 게 부여 상태 키트 데이터에 액세스할 수 있는 권한이 있는지 여부를 나타내지 않습니다! 사용자 데이터에 대 한 액세스를 허용 하기 위해 기회 부여 된만 나타냅니다.

응용 프로그램에는 데이터에 액세스할 수 있는지 여부를 확인 하는 `HKHealthStore.GetAuthorizationStatus()` 을 사용 하는 전달 `HKQuantityTypeIdentifierKey.HeartRate`합니다. 반환 된 상태에 따라, 응용 프로그램 사용 하거나 데이터를 입력 하는 기능을 해제 합니다. 액세스 거부를 처리 하기 위한 표준 사용자 본 경험이 없는 이며 여러 가지 가능한 옵션이 있습니다. 예제 응용 프로그램 상태가에 설정 되는 `HeartRateModel` 차례로 관련 이벤트를 발생 시키는 singleton 개체입니다.

## <a name="model-view-and-controller"></a>모델, 뷰 및 컨트롤러

검토 하는 `HeartRateModel` singleton 개체를 열기 고 `HeartRateModel.cs` 파일:

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

첫 번째 섹션은 제네릭 이벤트 처리기를 만들기 위한 상용구 코드입니다. 초기 부분은 `HeartRateModel` 클래스는 스레드로부터 안전한 singleton 개체를 만들기 위한 상용구도 합니다.

그런 다음 `HeartRateModel` 3 이벤트를 노출 합니다. 

- `EnabledChanged` -심 박수 저장소 사용 또는 사용 하지 않도록 설정 되어 있는지 나타냅니다 (저장소를 처음 사용할 수 없다는 점에 유의). 
- `ErrorMessageChanged` -이 샘플 응용 프로그램에 대 한 매우 간단한 오류 처리 모델 했으므로: 마지막 오류가 있는 문자열입니다. 
- `HeartRateStored` -심 박수 상태 키트 데이터베이스에 저장 될 때 발생 합니다.

이러한 이벤트는 발생 때마다 통해 수행 됩니다 `NSObject.InvokeOnMainThread()`, UI를 업데이트 하는 구독자 수 있습니다. 또는 백그라운드 스레드에서 발생 하는 이벤트 문서화 수 하 고 호환성을 보장 해야 해당 처리기에 둘 수 없습니다. 스레드 고려 사항은 대부분의 권한 요청과 같은 함수는 비동기 이며 해당 콜백을 주 스레드에서 실행 하기 때문에 상태 키트는 응용 프로그램에서 중요 합니다.

특정 코드에 Heath 키트 `HeartRateModel` 두 함수에 `HeartRateInBeatsPerMinute()` 및 `StoreHeartRate()`합니다. 

`HeartRateInBeatsPerMinute()` 강력한 형식의 상태 키트를 해당 인수를 변환 `HKQuantity`합니다. 수량의 형식이 변수로 지정는 `HKQuantityTypeIdentifierKey.HeartRate` 고 수량 단위는 `HKUnit.Count` 나눈 `HKUnit.Minute` (즉, 단위는 *아닙니다*). 

`StoreHeartRate()` 함수는 `HKQuantity` (하 여 만든 샘플 응용 프로그램에서 `HeartRateInBeatsPerMinute()` ). 해당 데이터를 유효성 검사를 사용 하 여는 `HKQuantity.IsCompatible()` 반환 하는 메서드 `true` 인수에 단위로 개체의 단위를 변환할 수 있는 경우. 수량으로 만든 경우 `HeartRateInBeatsPerMinute()` 이 분명 반환 됩니다 `true`, 것도 반환 `true` 수량으로, 예를 들어, 생성 된 경우 *시간 보다 더 나은*합니다. 보다 일반적으로, `HKQuantity.IsCompatible()` mass, 유효성 검사를 사용할 수 거리 및 에너지 사용자 또는 장치 입력 또는 하나의 측정 시스템의 (예: 제국 단위)를 표시 하지만 다른 시스템 (예: 미터 단위)에 저장 될 수 있습니다. 

수량 호환성 확인 되 면는 `HKQuantitySample.FromType()` 팩터리 메서드는 강력한 형식의 만드는 데 `heartRateSample` 개체입니다. `HKSample` 개체는 시작 및 종료 날짜입니다. 즉시 판독값에 대 한 이러한 값 동일 해야, 예제에 있는 것 처럼 합니다. 또한이 샘플 모든 키-값 데이터를 설정 하지 않습니다 해당 `HKMetadata` 인수 이지만 센서 위치를 지정 하려면 다음 코드와 같은 코드를 사용할 수 있습니다.

```csharp
var hkm = new HKMetadata();
hkm.HeartRateSensorLocation = HKHeartRateSensorLocation.Chest;

```

한 번의 `heartRateSample` 되었습니다 만들어지면 코드 새 연결을 만듭니다는 데이터베이스에 사용 하 여 블록입니다. 해당 블록에서의 `HKHealthStore.SaveObject()` 메서드는 데이터베이스에 비동기 쓰기를 시도 합니다. 람다 식에 대 한 결과 호출 하거나 관련 된 이벤트를 트리거합니다 `HeartRateStored` 또는 `ErrorMessageChanged`합니다.

이제 모델 프로그래밍 컨트롤러 모델의 상태를 반영 하는 방법을 보려면 시간입니다. 열립니다는 `HKWorkViewController.cs` 파일입니다. 생성자를 단순히 연결는 `HeartRateModel` 이벤트 처리 메서드를 단일 항목 (인라인 람다 식 사용 하 여 제한을 적용할 수 있지만 별도 메서드로 의도 좀 더 명확 하 게 다시):

```csharp
public HKWorkViewController (IntPtr handle) : base (handle)
{
     HeartRateModel.Instance.EnabledChanged += OnEnabledChanged;
     HeartRateModel.Instance.ErrorMessageChanged += OnErrorMessageChanged;
     HeartRateModel.Instance.HeartRateStored += OnHeartBeatStored;
}

```

다음은 관련 처리기입니다.

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

물론, 응용 프로그램에서 단일 컨트롤러를 별도 모델 개체의 생성 및 제어 흐름에 대 한 이벤트의 사용을 방지 하기 위해 수 없지만 모델 개체의 사용은 실제 응용 프로그램에 더 적합 합니다.

## <a name="running-the-sample-app"></a>샘플 응용 프로그램 실행

IOS 시뮬레이터 상태 키트를 지원 하지 않습니다. 디버깅 하는 iOS 8을 실행 하는 실제 장치에서 수행 되어야 합니다.

시스템에 제대로 프로 비전 iOS 8 개발 장치를 연결 합니다. Mac 용 Visual Studio의 배포 대상으로 선택 하 고 메뉴에서 선택 **실행 > 디버그**합니다.

> [!IMPORTANT]
> 이 시점에서 실수와 관련 된 프로 비전 노출할 합니다. 오류를 해결 하려면 만들기 및 프로비저닝 위의 상태 키트 응용 프로그램 섹션을 검토 합니다. 구성 요소입니다. 
>
> - **iOS Dev Center** -명시적 응용 프로그램 ID 및 상태 키트 프로 비전 프로필을 사용 하도록 설정 합니다. 
> - **프로젝트 옵션** -번들 식별자 (명시적 응용 프로그램 ID) 및 프로 비전 프로필입니다.
> - **소스 코드** -Entitlements.plist & Info.plist

프로 비전 제대로 설정 되어 있다고 가정 하면 응용 프로그램이 시작 됩니다. 에 도착할 때 해당 `OnActivated` 메서드를 상태 키트 권한 부여를 요청 합니다. 운영 체제에서 발견 한이 처음으로 다음과 같은 대화 상자가 표시 되면서 사용자가 표시 됩니다.


[![](healthkit-images/image12.png "사용자가이 대화 상자 표시")](healthkit-images/image12.png#lightbox)

심 박수 데이터를 업데이트 하려면 앱을 사용 하도록 설정 하 고 응용 프로그램은 더 이상 표시 합니다. `ReactToHealthCarePermissions` 콜백 비동기적으로 활성화 됩니다. 이렇게 하면는 `HeartRateModel’s` `Enabled` 에서 발생 하는 속성을 변경 하려면는 `EnabledChanged` 발생할 수 있는 이벤트는 `HKPermissionsViewController.OnEnabledChanged()` 통해를 실행 하려면 이벤트 처리기는 `StoreData` 단추입니다. 다음 다이어그램은 시퀀스를 나타냅니다.


[![](healthkit-images/image13.png "이 다이어그램의 이벤트 순서를 보여 줍니다.")](healthkit-images/image13.png#lightbox)

키를 눌러는 **레코드** 단추입니다. 이렇게 하면는 `StoreData_TouchUpInside()` 의 값을 구문 분석 하려고 하는 처리기를 실행 하려면는 `heartRate` 텍스트 필드를 변환에는 `HKQuantity` 앞에서 설명한 통해 `HeartRateModel.HeartRateInBeatsPerMinute()` 작동 시키고 해당 수량을 `HeartRateModel.StoreHeartRate()`합니다. 이 데이터를 저장 하려고 시도 하며에서 발생 하거나 이전에 설명한 대로 `HeartRateStored` 또는 `ErrorMessageChanged` 이벤트입니다.

두 번 클릭 하 여 **홈** 장치에서 단추를 선택한 상태 앱을 엽니다. 클릭는 **소스** 탭 하 고 나열 된 샘플 응용 프로그램에 표시 됩니다. 항목을 선택 하 고 심 박수 데이터를 업데이트할 수를 허용 하지 않습니다. 두 번 클릭 하 고 **홈** 앱에 백업 하는 단추와 스위치입니다. 다시 한 번 `ReactToHealthCarePermissions()` 호출 됩니다. 하지만이 이번에 액세스가 거부 되어 서는 **StoreData** 단추가 비활성화 됩니다 (참고가 비동기적으로 발생 하 고 사용자 인터페이스의 변경 내용을 확인 최종 사용자에 게 표시 될 수 있습니다).

## <a name="advanced-topics"></a>고급 항목

상태 관리 키트에서 데이터를 읽는 데이터베이스는와 매우 유사 데이터를 쓰는: 다른 사람이 요청 권한 부여에 액세스 하려고 하는 데이터 형식을 하나 지정 하 고 데이터 자동 변환 단위의 호환 함께 사용할 수는 해당 부여 하는 경우 측정값입니다.

조건자 기반 쿼리 및 관련 데이터가 업데이트 될 때 업데이트를 수행 하는 쿼리 수 있는 보다 복잡 한 쿼리 함수는 여러 가지가 있습니다. 

상태 키트 응용 프로그램 개발자는 Apple의 상태 키트 섹션을 검토 해야 [앱 검토 지침](https://developer.apple.com/app-store/review/guidelines/#healthkit)합니다.

보안 및 형식 시스템 모델 파악 되 면 저장 및 공유 상태 키트 데이터베이스의 데이터 읽기는 간단 합니다. 비동기적으로 작동 많은 상태 키트 내에서 기능 및 응용 프로그램 개발자가 해당 프로그램을 적절 하 게 작성 해야 합니다.

이 문서를 작성할 당시 ľ ř ˝ Android 또는 Windows Phone 상태 Kit에 해당 키 없음.

## <a name="summary"></a>요약

이 문서의 상태 키트를 저장할 응용 프로그램을 사용 하는 방법을 살펴보았습니다 검색, 및 공유 상태 관련 정보를, 사용자 액세스 및이 데이터를 제어할 수 있도록 하는 표준 상태 응용 프로그램을 제공 합니다. 

복잡 한 응용 프로그램 관리 측면 (프로 비전), 코딩 (상태 키트의 형식에에서 상태 키트를 사용 하 여 앱의 증가 처리 해야 하 고 보았습니다 개인 정보 보호, 보안 및 데이터 무결성 상태 관련 정보에 대 한 문제를 재정의 하는 방법 시스템) 및 사용자 환경을 (시스템 대화 상자 및 상태 응용 프로그램을 통해 사용 권한 사용자 정의 컨트롤). 

마지막으로 상태 키트 저장소 하트 비트 데이터를 기록 하 고 비동기 인식 디자인 하는 포함 되어 있는 예제 응용 프로그램을 사용 하 여 상태 키트의 간단한 구현을 살펴보겠습니다 했습니다.

## <a name="related-links"></a>관련 링크

- [HKWork (샘플)](https://developer.xamarin.com/samples/monotouch/ios8/IntroToHealthKit/)
- [iOS 8 소개](~/ios/platform/introduction-to-ios8.md)
