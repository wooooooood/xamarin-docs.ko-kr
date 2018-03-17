---
title: CloudKit
description: "icloud와 사용자의 계정을 통해 자동 동기화를 지 원하는 iCloud에 데이터를 저장할 iOS 8 응용 프로그램 Api를 사용 합니다. ICloud 사용 장치에서 사용자에 게 일관 되 고 완벽 한 경험을 제공 CloudKit를 사용 하 여 합니다. 이 문서에서는 CloudKit 편의 API를 사용 하 여 iOS 8 응용 프로그램에서 사용 하도록 설정 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 66B207F2-FAA0-4551-B43B-3DB9F620C397
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/11/2016
ms.openlocfilehash: e231043b1c4b0fa7ba72f2a371545036ffb21164
ms.sourcegitcommit: 5fc1c4d17cd9c755604092cf7ff038a6358f8646
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/17/2018
---
# <a name="cloudkit"></a>CloudKit

_icloud와 사용자의 계정을 통해 자동 동기화를 지 원하는 iCloud에 데이터를 저장할 iOS 8 응용 프로그램 Api를 사용 합니다. ICloud 사용 장치에서 사용자에 게 일관 되 고 완벽 한 경험을 제공 CloudKit를 사용 하 여 합니다. 이 문서에서는 CloudKit 편의 API를 사용 하 여 iOS 8 응용 프로그램에서 사용 하도록 설정 합니다._

CloudKit 프레임 워크는 해당 액세스 iCloud 응용 프로그램 개발을 간소화합니다. 여기에 응용 프로그램 데이터 및 자산 권한으로 응용 프로그램 정보를 안전 하 게 저장할 수를 검색 합니다. 이 키트 개인 정보를 공유 하지 않고 자신의 iCloud Id와 응용 프로그램에 대 한 액세스를 허용 하 여 사용자에 게 익명의 계층을 제공 합니다.

개발자는 클라이언트 쪽 응용 프로그램에 집중 하 고 서버 쪽 응용 프로그램 논리를 작성할 필요가 없도록 iCloud 수 수 있습니다. CloudKit 인증, 개인 및 공용 데이터베이스 및 구조화 된 데이터 및 자산 저장소 서비스를 제공합니다.

## <a name="requirements"></a>요구 사항

이 문서에 제공 된 단계를 완료 하려면 다음이 요구 됩니다.

-  **Xcode 및 iOS SDK** – Apple의 Xcode 및 iOS 8 Api 해야 설치 하 고 개발자의 컴퓨터에 구성 합니다.
-  **Mac 용 visual Studio** -최신 버전의 Mac 용 Visual Studio를 설치 하 고 사용자 장치에 구성 해야 합니다.
-  **iOS 8 장치** – iOS 장치에서 최신 버전의 테스트를 위해 iOS 8 실행 합니다.

## <a name="what-is-cloudkit"></a>CloudKit 란?

CloudKit 사용 방법은 서버 iCloud에 개발자 액세스할 수 있습니다. 드라이브 iCloud와 사진 라이브러리 iCloud에 대 한 기초를 제공합니다. CloudKit은 Mac OS X와 Apple iOS 장치에서 지원 됩니다.

 [![](intro-to-cloudkit-images/image1.png "Mac OS X와 Apple iOS 장치에서 CloudKit 지 원하는 방법")](intro-to-cloudkit-images/image1.png#lightbox)

CloudKit은 iCloud 계정 인프라를 사용 합니다. iCloud 장치에서 계정에 로그인 한 사용자가을 CloudKit 사용자를 식별 하의 ID를 사용 합니다. 사용할 수 있는 계정이 없는 경우 제한 된 읽기 전용 액세스 제공 됩니다.

CloudKit 공용 및 개인 데이터베이스의 개념을 지원합니다. Public 데이터베이스는 사용자가 액세스할 수 있는 모든 데이터의 "soup"를 제공 합니다. 개인 데이터베이스가 바인딩된 특정 사용자에 게 개인 데이터를 저장 되어야 합니다.

CloudKit 구조적 및 대량 데이터를 지원합니다. 큰 파일 전송을 원활 하 게 처리할 수 있는 경우 CloudKit 담당 하 게 백그라운드에서 iCloud 서버에서에서 대형 파일을 효율적으로 전송 개발자가 다른 작업에 집중할 수를 해제 합니다.

> [!NOTE]
> **참고:** CloudKit는 해야는 _전송 기술_합니다. 모든 지 속성; 제공 되지 않습니다. 만 응용을 프로그램을 보내고 서버에서 정보를 효율적으로 받을 수 있습니다.

이 문서의 작성일 Apple 처음 환경이 CloudKit 무료로 대역폭과 저장 용량이 높은 제한입니다. 더 큰 프로젝트나 대형 사용자 기반으로 응용 프로그램에 대 한 Apple에는 저렴 한 가격 책정 범위로 제공할 것 암시 합니다.


## <a name="enabling-cloudkit-in-a-xamarin-application"></a>CloudKit Xamarin 응용 프로그램에서 사용 하도록 설정

Xamarin 응용 프로그램 CloudKit 프레임 워크를 활용할 수 있습니다, 전에 응용 프로그램이 올바르게 프로 비전 해야에 설명 된 대로 [기능 작업](~/ios/deploy-test/provisioning/capabilities/icloud-capabilities.md) 및 [자격과 작업](~/ios/deploy-test/provisioning/entitlements.md) 가이드

1.  Mac 또는 Visual Studio에 대 한 Visual Studio에서 프로젝트를 엽니다.
2.  에 **솔루션 탐색기**열고는 **Info.plist** 파일을 확인는 **번들 식별자** 에 정의 된 것과 일치 **앱 ID**프로 비전의 일부를 설정 하는 대로 생성:
 
    [![](intro-to-cloudkit-images/image26a.png "번들 Id를 입력 합니다")](intro-to-cloudkit-images/image26a-orig.png#lightbox "Info.plist file displaying Bundle Identifier")

3.  맨 아래로 스크롤하여는 **Info.plist** 파일을 선택 **백그라운드 모드 활성화**, **위치 업데이트** 및 **원격 알림**:

    [![](intro-to-cloudkit-images/image27a.png "사용 가능한 백그라운드 모드, 위치 업데이트 및 원격 알림 선택")](intro-to-cloudkit-images/image27a-orig.png#lightbox "Info.plist file displaying background modes")
4.  선택한 솔루션에 iOS 프로젝트를 마우스 오른쪽 단추로 클릭 **옵션**합니다.
5.  선택 **iOS 번들 서명**, 선택는 **개발자 Identity** 및 **프로 비전 프로필** 위에서 만든 합니다.
6.  확인 된 **Entitlements.plist** 포함 **iCloud를 사용 하도록 설정** , **키-값 저장소** 및 **CloudKit** 합니다.
7.  확인 된 **편 재 컨테이너** (위에서 만든) 하는 동안 응용 프로그램에 대 한 존재 합니다. 예: `iCloud.com.your-company.CloudKitAtlas`
8.  파일의 변경 내용을 저장합니다.


현재 위치를이 설정 하 여 응용 프로그램 CloudKit 프레임 워크 Api에 액세스할 준비가 되었습니다.

## <a name="cloudkit-api-overview"></a>CloudKit API 개요

CloudKit Xamarin iOS 응용 프로그램에서을 구현 하기 전에이 문서는 예정 다음 항목 포함 된 CloudKit 프레임 워크의 기본 사항:

1.  **컨테이너** – 사일로 iCloud 통신의 격리 합니다.
2.  **데이터베이스** -공용 및 전용 응용 프로그램에 사용할 수 있는 합니다.
3.  **레코드** – CloudKit에서 구조화 된 데이터 이동 메커니즘입니다.
4.  **레코드 영역** – 레코드의 그룹입니다.
5.  **레코드 식별자** – 완벽 하 게 정규화 되 고 레코드의 특정 위치를 나타냅니다.
6.  **참조** – 부모-자식 지정된 된 데이터베이스 내에서 관련된 레코드 간의 관계를 제공 합니다.
7.  **자산** – 큰, 구조화 되지 않은 데이터를 iCloud에 업로드 하 고 지정된 된 레코드와 관련 된 수의 파일에 대 한 허용 합니다.


### <a name="containers"></a>컨테이너

지정된 된 응용 프로그램 iOS 장치에서 실행 되는 항상 실행 다른 응용 프로그램과 해당 장치에 서비스를 측면 방향으로 표시 합니다. 클라이언트 장치에서 응용 프로그램 사일로 또는 어떤 방식으로든에서 샌드박스 되도록 하려고 합니다. 경우에 따라이 리터럴 샌드박스 이며, 다른 응용 프로그램은 단순히에 실행 중인 고유 메모리 공간입니다.

클라이언트 응용 프로그램을 수행 하 고 실행 하는 다른 클라이언트에서 신경을 쓰지 않아도 개념은 매우 강력 하며 다음과 같은 이점이 있습니다.

1.  **보안** – 하나의 응용 프로그램 다른 클라이언트 응용 프로그램 또는 OS 자체를 방해할 수 있습니다.
1.  **안정성** -클라이언트 응용 프로그램에는 손상 되는 경우 운영 체제의 다른 앱으로 사용할 수 없습니다.
1.  **개인 정보 보호** – 각 클라이언트 응용 프로그램에 장치에 저장 된 개인 정보에 대 한 액세스를 제한 합니다.


위에 나열 된은 동일한 이점을 제공할 및 클라우드 기반 정보로 작업에 적용할 CloudKit ´ ù.

 [![](intro-to-cloudkit-images/image31.png "컨테이너를 사용 하 여 CloudKit 응용 프로그램이 통신")](intro-to-cloudkit-images/image31.png#lightbox)

되는 응용 프로그램 처럼의 일 대 다는 장치에서 실행 되는 일 대 다의 iCloud와의 통신을 응용 프로그램의 따라서입니다. 이러한 다양 한 통신 사일로의 각 컨테이너 라고 합니다.

컨테이너를 통해 CloudKit 프레임 워크에서 제공 되는 `CKContainer` 클래스입니다. 기본적으로 한 응용 프로그램 토론 하나의 컨테이너 및이 컨테이너에 해당 응용 프로그램에 대 한 데이터를 분리 합니다. 즉, 여러 응용 프로그램을 동일한 iCloud 계정과 정보를 저장 될 수 있지만이 정보를 혼합 하지 것입니다.

Icloud와 데이터의 컨테이너 화에 CloudKit을 사용자 정보를 캡슐화 할 수 있습니다. 이러한 방식으로 응용 프로그램 되므로 iCloud 계정에 대 한 몇 가지 제한 된 액세스 한 개인 정보 및 사용자의 보안 보호 하면서 모든 내 사용자 정보를 저장 합니다.

컨테이너의 WWDR 포털을 통해 응용 프로그램 개발자가 완전히 관리 됩니다. 컨테이너의 네임 스페이스 이므로 모든 Apple 개발자가 전체 전역 컨테이너만 아니어야 주어진된 개발자의 응용 프로그램에 고유 하지만 모든 Apple 개발자와 응용 프로그램에 있습니다.

Apple 응용 프로그램 컨테이너에 대 한 네임 스페이스를 만들 때 역방향 DNS 표기법을 사용 하 여 제안 합니다. 예제:

```csharp
iCloud.com.company-name.application-name
```

컨테이너는, 기본적으로 바인딩된 일대일로 지정된 된 응용 프로그램, 응용 프로그램 간에 공유할 수 있습니다. 따라서 여러 응용 프로그램이 단일 컨테이너에서 조정할 수 있습니다. 단일 응용 프로그램 여러 컨테이너에도 통신할 수 있습니다.

### <a name="databases"></a>Databases

CloudKit의 주요 기능 중 하나 해당 모델 최대 iCloud 서버 응용 프로그램의 데이터 모델 및 복제를 수행 하는 것입니다. 일부 정보를 만든 사용자를 위한, 다른 정보는 공용으로 사용 (예: 음식점 리뷰의 경우)에 대 한 사용자가 만들 수 있는 공용 데이터 또는 정보를 개발자가 응용 프로그램에 대 한 게시 수 있습니다. 두 경우 모두의 대상 형식이 단일 사용자 아니지만 사용자의 커뮤니티입니다.

 [![](intro-to-cloudkit-images/image32.png "CloudKit 컨테이너 다이어그램")](intro-to-cloudkit-images/image32.png#lightbox)

컨테이너 내부 즉, 무엇 보다도 공용 데이터베이스가입니다. 이것이 거주 하 고 모든 공용 정보 및 mingles 배치입니다. 또한 응용 프로그램의 각 사용자에 대 한 개별 여러 개인 데이터베이스 됩니다.

IOS 장치에서 실행 하는 경우 응용 프로그램에만 iCloud 현재 로그인 한 사용자에 대 한 정보에 대 한 액세스를 갖습니다. 따라서 컨테이너의 응용 프로그램의 보기 다음과 같이 됩니다.

 [![](intro-to-cloudkit-images/image33.png "컨테이너의 응용 프로그램 보기")](intro-to-cloudkit-images/image33.png#lightbox)

공용 데이터베이스와 현재 로그온 iCloud 사용자와 관련 된 개인 데이터베이스에만 볼 수 있습니다.

데이터베이스를 통해 CloudKit 프레임 워크에서 제공 되는 `CKDatabase` 클래스입니다. 모든 응용 프로그램에 두 데이터베이스에 대 한 액세스: 공용 데이터베이스 및 개인 것입니다.

컨테이너는 CloudKit에 초기 요소입니다. 응용 프로그램의 기본 컨테이너에서에서 공용 및 개인 데이터베이스에 액세스 하는 다음 코드를 사용할 수 있습니다.

```csharp
using CloudKit;
...

public CKDatabase PublicDatabase { get; set; }
public CKDatabase PrivateDatabase { get; set; }
...

// Get the default public and private databases for
// the application
PublicDatabase = CKContainer.DefaultContainer.PublicCloudDatabase;
PrivateDatabase = CKContainer.DefaultContainer.PrivateCloudDatabase;
```

데이터베이스 형식 간의 차이점은 다음과 같습니다.

||Public 데이터베이스|전용 데이터베이스|
|---|--- |--- |
|**데이터 형식**|공유 데이터|현재 사용자의 데이터|
|**할당량**|개발자의 할당량에서 고려|사용자의 할당량에서 고려|
|**기본 사용 권한**|읽을 수 있는 world|읽을 수 있는 사용자|
|**사용 권한 편집**|iCloud 대시보드 역할을 통해 레코드 클래스 수준|N/A|

### <a name="records"></a>레코드

데이터베이스 및 데이터베이스 내의 레코드가 보유 하는 컨테이너입니다. 레코드는 CloudKit에서 구조화 된 데이터 이동 메커니즘.

 [![](intro-to-cloudkit-images/image34.png "데이터베이스 및 데이터베이스 내의 레코드가 보유 하 고 컨테이너")](intro-to-cloudkit-images/image34.png#lightbox)

레코드를 통해 CloudKit 프레임 워크에서 제공 되는 `CKRecord` 키-값 쌍을 래핑하는 클래스입니다. 응용 프로그램에서 개체의 인스턴스는 해당 하는 `CKRecord` CloudKit에 있습니다. 또한 각 `CKRecord` 개체의 클래스에 해당 하는 레코드 유형을 소유 하 고 있습니다.

레코드는 데이터 처리를 위해 전달 하기 전에 CloudKit에 설명 되어 있으므로 적시에 스키마를 가져야 합니다. 해당 지점에서 CloudKit 정보를 해석 되 고 저장 하 고 검색 레코드의 배치 처리 됩니다.

`CKRecord` 클래스는 다양 한 메타 데이터 지원 합니다. 예를 들어 레코드를 만들 때와 만든 사용자에 대 한 정보를 포함 합니다. 또한 레코드를 마지막으로 수정한 및 수정한 사용자에 대 한 정보를 포함 합니다.

레코드 변경 태그의 개념을 포함 합니다. 지정된 된 레코드의 수정 버전의 이전 버전입니다. 변경 태그는 클라이언트와 서버에 같은 버전이 지정된 된 레코드의 경우를 결정 하는 간단한 방법으로 사용 됩니다.

위의 설명 대로 `CKRecords` 키-값 쌍 시작 되며 따라서 레코드에 다음과 같은 유형의 데이터를 저장할 수 있습니다.

1.   `NSString`
1.   `NSNumber`
1.   `NSData`
1.   `NSDate`
1.   `CLLocation`
1.   `CKReferences`
1.   `CKAssets`


단일 값 형식 외에도 레코드 형식이 같은 배열 이러한 나열 된 형식 중 하나를 포함할 수 있습니다.

다음 코드 새 레코드를 만들고 데이터베이스에 저장할 데 사용할 수 있습니다.

```csharp
using CloudKit;
...

private const string ReferenceItemRecordName = "ReferenceItems";
...

var newRecord = new CKRecord (ReferenceItemRecordName);
newRecord ["name"] = (NSString)nameTextField.Text;
await CloudManager.SaveAsync (newRecord);
```

### <a name="record-zones"></a>레코드 영역입니다.

레코드는 특정된 데이터베이스 내에 단독으로 존재 하지 않으므로-레코드 그룹 레코드 영역 내 함께 존재 합니다. 기존 관계형 데이터베이스의 테이블로 레코드 영역 생각할 수 있습니다.

 [![](intro-to-cloudkit-images/image35.png "레코드 그룹을 존재 레코드 영역 내에 함께")](intro-to-cloudkit-images/image35.png#lightbox)

지정 된 레코드가 영역 내의 여러 레코드와 지정된 된 데이터베이스 내에서 여러 레코드 영역 수 있습니다. 모든 데이터베이스에 기본 레코드 영역이 포함 되어 있습니다.

 [![](intro-to-cloudkit-images/image36.png "기본 레코드 영역 및 사용자 지정 영역을 포함 하는 모든 데이터베이스")](intro-to-cloudkit-images/image36.png#lightbox)

이것이 기본적으로 레코드가 저장 되는 위치입니다. 또한 사용자 지정 레코드 영역을 만들 수 있습니다. 기본 세분성은 원자성 커밋 및 변경 내용 추적에 의해 이루어진다는 영역 나타내며를 기록 합니다.

## <a name="record-identifiers"></a>레코드 식별자

레코드 식별자는 튜플로 서 표시 됩니다, 그리고 둘 다를 포함 하는 클라이언트에 게 제공 레코드 이름 및 레코드 존재 하는 영역입니다. 레코드 식별자에는 다음과 같은 특징이 있습니다.

-  클라이언트 응용 프로그램에 의해 생성 됩니다.
-  이들은 완전히 정규화 된 하며 레코드의 특정 위치를 나타냅니다.
-  레코드 이름으로 외부 데이터베이스에서 레코드의 고유 ID를 할당 하 여 CloudKit 내에서 저장 되지 않은 로컬 데이터베이스 연결 데 사용할 수 있습니다.


개발자가 새 레코드를 만들 때 레코드 식별자에 전달 하도록 선택할 수 있습니다. 레코드 식별자를 지정 하지 않은 경우 UUID 자동으로 생성 되며 레코드에 할당 합니다.

개발자가 새 레코드 Id를 만드는 경우 각 레코드에 속하는 레코드 영역 지정 하도록 선택할 수 있습니다. 지정 하지 않으면 기본 레코드 영역 사용 됩니다.

레코드 식별자를 통해 CloudKit 프레임 워크에서 제공 되는 `CKRecordID` 클래스입니다. 다음 코드는 새 레코드 식별자를 만드는 데 사용할 수 있습니다.

```csharp
var recordID =  new CKRecordID("My Record");
```

### <a name="references"></a>참조

참조는 특정된 데이터베이스 내 관련된 레코드 간의 관계를 제공합니다.

 [![](intro-to-cloudkit-images/image37.png "참조는 특정된 데이터베이스 내 관련된 레코드 간의 관계를 제공합니다.")](intro-to-cloudkit-images/image37.png#lightbox)

위의 예제에서는 자식은 부모 레코드의 자식 레코드를 부모 자식을 소유 합니다. 관계는 부모 레코드에 자식 레코드에서가 이동 하며 라고는 *다시 참조*합니다.

참조를 통해 CloudKit 프레임 워크에서 제공 되는 `CKReference` 클래스입니다. 서로 iCloud 서버 레코드 간의 관계를 이해 하도록 허용 하는 방법입니다.

참조 뒤에 있는 관련 항목 삭제 메커니즘을 제공합니다. 부모 레코드를 데이터베이스에서 삭제 되 면 모든 자식 레코드 (지정 된 대로 관계에서)도 데이터베이스에서 자동으로 삭제 됩니다.

> [!NOTE]
> **참고**: 현 수 포인터 가능성이 사용 하는 경우 CloudKit 합니다. 예를 들어 응용 프로그램 레코드 포인터 목록을 인출, 레코드 선택 있으며 레코드에 대 한 다음 요청 될 때 레코드 더 이상 존재 데이터베이스에 없습니다. 응용 프로그램이이 상황을 정상적으로 처리 하도록 코드를 작성 해야 합니다.

필수 동안 CloudKit Framework와 함께 작업 하는 경우 참조 다시 기본 설정 됩니다. Apple이 참조의 가장 효율적인 유형 작업 수행 하려면 시스템을 미세 했습니다.

참조를 만드는 개발자 이미 메모리에 있는 레코드를 제공 하거나 레코드 식별자에 대 한 참조를 만드십시오. 레코드 식별자 및 지정된 된 참조를 사용 하 여 데이터베이스에 존재 하지 않은 경우 현 수 포인터 만들어집니다.

다음은 알려진된 레코드에 대 한 참조를 만드는 예제입니다.

```csharp
var reference = new CKReference(newRecord, new CKReferenceAction());
```

### <a name="assets"></a>자산

자산에는 큰, 구조화 되지 않은 데이터를 iCloud에 업로드 하 고 지정된 된 레코드와 연결 된 허용:

 [![](intro-to-cloudkit-images/image38.png "자산에는 큰, 구조화 되지 않은 데이터를 iCloud에 업로드 하 고 지정된 된 레코드와 연결 된 허용")](intro-to-cloudkit-images/image38.png#lightbox)

클라이언트에서는 한 `CKRecord` 만들어집니다 iCloud 서버에 업로드 하려는 파일을 설명 하는 합니다. A `CKAsset` 파일을 포함 하기 위해 만들어진 및 것을 설명 하는 레코드에 연결 되어 있습니다.

파일 서버에 업로드 되 면 레코드는 데이터베이스에 배치 되 고 특별 한 대량 저장소 데이터베이스에 파일이 복사 됩니다. 링크는 레코드 포인터와 업로드 한 파일 사이 만들어집니다.

자산 CloudKit 프레임 워크를 통해 노출 되는 `CKAsset` 클래스 및 큰, 구조화 되지 않은 데이터를 저장 하는 데 사용 됩니다. 때문에 개발자 하지 큰, 구조화 되지 않은 데이터가 메모리에 있는, 자산 파일을 디스크에 사용 하 여 구현 됩니다.

자산 자산에서 iCloud 포인터로 레코드를 사용 하 여 검색할 수 있는 레코드에 의해 소유 됩니다. 이러한 방식으로 서버 자산을 담당 하는 레코드를 삭제할 때 가비지 수집 자산 수 있습니다.

때문에 `CKAssets` 사항은 Apple 설계를 효율적으로 업로드 하 고 자산 다운로드 CloudKit, 대용량 데이터 파일을 처리 합니다.

자산 만들기 레코드와 연결 하려면 다음 코드를 사용할 수 있습니다.:

```csharp
var fileUrl = new NSUrl("LargeFile.mov");
var asset = new CKAsset(fileUrl);
newRecord ["name"] = asset;
```

이제 CloudKit 내의 기본 개체의 모든 설명 했습니다. 컨테이너와 연결 된 응용 프로그램 및 데이터베이스를 포함 합니다. 데이터베이스에 레코드를 레코드 영역으로 그룹화 되 고 레코드 식별자가 가리키는 포함 합니다. 참조를 사용 하는 레코드 간의 부모-자식 관계 정의 됩니다. 마지막으로, 큰 파일 업로드 및 자산을 사용 하 여 레코드에 연결 된 수 있습니다.

## <a name="cloudkit-convenience-api"></a>CloudKit Convenience API

Apple는 CloudKit 사용 하기 위한 두 개의 다른 API 집합을 제공 합니다.

-  **Operational API** – CloudKit의 단일 모든 기능을 제공 합니다. 더 복잡 한 응용 프로그램의 경우이 API는 CloudKit 세부적으로 제어를 제공합니다.
-  **Api** – CloudKit 기능의 일반적으로 미리 구성 된 하위 집합을 제공 합니다. IOS 응용 프로그램에서 CloudKit 기능을 포함 하는 데 편리 하 고 쉬운 액세스 솔루션을 제공 합니다.


편의 API은 일반적으로 대부분 iOS 응용 프로그램에 적합 하며, 사과 것으로 시작 합니다. 이 섹션의 나머지 부분에서는 다음과 같은 편리한 API 항목을 설명 합니다.

-  레코드를 저장 합니다.
-  레코드를 인출 합니다.
-  레코드를 업데이트 합니다.


### <a name="common-setup-code"></a>일반적인 설치 코드

CloudKit 편의 API을 시작 하기 전에 필요한 표준 설치 코드 몇 가지 있습니다. 응용 프로그램의 수정 하 여 시작 `AppDelegate.cs` 파일을 다음과 같이 되도록 합니다.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Foundation;
using UIKit;
using CloudKit;

namespace CloudKitAtlas
{
    [Register ("AppDelegate")]
    public partial class AppDelegate : UIApplicationDelegate
    {
        #region Computed Properties
        public override UIWindow Window { get; set;}
        public CKDatabase PublicDatabase { get; set; }
        public CKDatabase PrivateDatabase { get; set; }
        #endregion

        #region Override Methods
        public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
        {
            application.RegisterForRemoteNotifications ();

            // Get the default public and private databases for
            // the application
            PublicDatabase = CKContainer.DefaultContainer.PublicCloudDatabase;
            PrivateDatabase = CKContainer.DefaultContainer.PrivateCloudDatabase;

            return true;
        }

        public override void RegisteredForRemoteNotifications (UIApplication application, NSData deviceToken)
        {
            Console.WriteLine ("Registered for Push notifications with token: {0}", deviceToken);
        }

        public override void FailedToRegisterForRemoteNotifications (UIApplication application, NSError error)
        {
            Console.WriteLine ("Push subscription failed");
        }

        public override void ReceivedRemoteNotification (UIApplication application, NSDictionary userInfo)
        {
            Console.WriteLine ("Push received");
        }
        #endregion
    }
}
```

위의 코드에서는 공용 및 개인 CloudKit 데이터베이스 응용 프로그램의 나머지 부분에서 사용 하기 쉽게 만들의 바로 가기 노출 합니다.

다음으로 뷰나 CloudKit 사용 하는 뷰 컨테이너를 다음 코드를 추가 합니다.

```csharp
using CloudKit;
...

#region Computed Properties
public AppDelegate ThisApp {
    get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
}
#endregion
```

이렇게 하면 추가 대 한 바로 가기는 `AppDelegate` 위에서 만든에서 공용 및 개인 데이터베이스 바로 가기 키에 액세스 하 고 있습니다.

이 코드 위치에서 Xamarin iOS 8 응용 프로그램에서 CloudKit 편의 API 구현를 살펴보면을 보겠습니다.

### <a name="saving-a-record"></a>레코드를 저장

레코드에 논의 하는 경우 위에서 설명한 패턴을 사용 하 여, 다음 코드는 새 레코드를 만들고 편의 API를 사용 하 여 공개 데이터베이스에 저장 하:

```csharp
private const string ReferenceItemRecordName = "ReferenceItems";
...

// Create a new record
var newRecord = new CKRecord (ReferenceItemRecordName);
newRecord ["name"] = (NSString)nameTextField.Text;

// Save it to the database
ThisApp.PublicDatabase.SaveRecord(newRecord, (record, err) => {
    // Was there an error?
    if (err != null) {
        ...
    }
});
```

세 가지 위의 코드에 대 한 참고 사항:

1.  호출 하 여는 `SaveRecord` 의 메서드는 `PublicDatabase`, 개발자는 데이터는 전송 되는 방법을 등에 쓰여지는 어떤 영역 지정 필요가 없습니다. 편의 API 관리의 모든 세부 자체입니다.
1.  호출이 비동기적 하 고 성공 또는 실패를 사용 하 여 호출이 완료 될 때 콜백 루틴을 제공 합니다. 호출이 실패 한 경우 오류 메시지가 제공 됩니다.
1.  CloudKit 로컬 저장소/지 속성이; 제공 하지 않습니다. 전송 미디어만 이며 따라서에 레코드를 저장 하는 요청이 수행 될 때 iCloud 서버로 즉시 전송 됩니다.


> [!NOTE]
> **참고**: 연결 손실 되는 지속적으로 또는 중단 되 면 개발자 CloudKit 작업은 오류 처리 하는 경우 수행 해야 하는 첫 번째 고려 사항 중 하나에 모바일 네트워크 통신의 "손실 허용" 특성으로 인해 합니다.

### <a name="fetching-a-record"></a>레코드를 인출합니다.

만들고 iCloud 서버에 성공적으로 저장 된 레코드와 다음 코드를 사용 하 여 레코드 검색:

```csharp
// Create a record ID and fetch the record from the database
var recordID = new CKRecordID("MyRecordName");
ThisApp.PublicDatabase.FetchRecord(recordID, (record, err) => {
    // Was there an error?
    if (err != null) {
        ...
    }
});
```

레코드를 저장에서 마찬가지로 위의 코드는 비동기, 단순 이루어지며 훌륭한 오류 처리.

### <a name="updating-a-record"></a>레코드 업데이트

ICloud 서버 레코드를 가져온 후 다음 코드 레코드를 수정 하 고 다시 데이터베이스에 변경 내용을 저장할 사용할 수 있습니다.

```csharp
// Create a record ID and fetch the record from the database
var recordID = new CKRecordID("MyRecordName");
ThisApp.PublicDatabase.FetchRecord(recordID, (record, err) => {
    // Was there an error?
    if (err != null) {

    } else {
        // Modify the record
        record["name"] = (NSString)"New Name";

        // Save changes to database
        ThisApp.PublicDatabase.SaveRecord(record, (r, e) => {
            // Was there an error?
            if (e != null) {
                 ...
            }
        });
    }
});
```

`FetchRecord` 의 메서드는 `PublicDatabase` 반환는 `CKRecord` 성공적으로 호출 합니다. 그런 다음 응용 프로그램 수정 기록 및 호출 `SaveRecord` 데이터베이스에 변경 내용을 다시 쓰기를 다시 합니다.

이 섹션에는 응용 프로그램 CloudKit 편의 API 작업할 때 사용 하는 일반적인 주기를 나타났습니다. 응용 프로그램 iCloud에 레코드를 저장, 검색 해당 레코드에서 iCloud, 레코드를 수정 하 고 iCloud에 다시 해당 변경 내용을 저장 합니다.

## <a name="designing-for-scalability"></a>확장성을 위한 디자인

지금까지이 문서에서에서 저장 및 검색 응용 프로그램의 전체 개체 모델 iCloud 서버에 있는 작업을 수행할 때마다 살펴보고 합니다. 이 방법은 적은 양의 데이터와 매우 작은 사용자 기반와 잘 작동 하는 동안 하기 쉽지 않습니다 정보 및/또는 사용자의 양을 증가 기반 하는 경우.

### <a name="big-data-tiny-device"></a>빅 데이터를 작은 장치

응용 프로그램이 더 많이 사용 되 면 데이터베이스에 더 많은 데이터 하며, 가능한 적은 장치에서 전체 데이터의 캐시 합니다. 이 문제를 해결 하기 위해 다음 방법은 사용할 수 있습니다.:

-  **큰 데이터는 클라우드에서 유지** – CloudKit 큰 데이터를 효율적으로 처리 하도록 설계 되었습니다.
-  **클라이언트는 해당 데이터의 조각을 보기만 해야** – 최소한 한 번에 모든 작업을 처리 하는 데 필요한 데이터의 종료 합니다.
-  **클라이언트 뷰를 변경할 수** – 각 사용자가 다른 기본 설정, 표시 되는 데이터 조각을 사용자를 변경할 수 있으며 사용자의 개별 때문에 지정된 된 조각이의 보기 서로 다를 수 있습니다.
-  **클라이언트 쿼리를 사용 하 여 시점에 초점을 맞추려면** – 쿼리인 경우에 사용자가 클라우드 내의 존재 하는 더 큰 데이터 집합의 작은 하위 집합을 볼 수 있습니다.


### <a name="queries"></a>쿼리

위에서 설명한 대로 쿼리를 사용 하면 개발자가 클라우드에 존재 하는 대규모 데이터 집합의 작은 하위 집합을 선택 합니다. 쿼리를 통해 CloudKit 프레임 워크에서 제공 되는 `CKQuery` 클래스입니다.

다음 세 가지 서로 다른 작업을 결합 하는 쿼리: 레코드 종류 ( `RecordType`), 조건자 ( `NSPredicate`) 및 필요에 따라 정렬 설명자 ( `NSSortDescriptors`). CloudKit 대부분의 지원 `NSPredicate`합니다.

#### <a name="supported-predicates"></a>지원 되는 조건자

CloudKit는 다음과 같은 유형의 지원 `NSPredicates` 쿼리로 작업 하는 경우:


1. 일치 레코드를 변수에 저장 된 값과 같습니다.
    
    ```
    NSPredicate.FromFormat(string.Format("name = '{0}'", recordName))
    ```
   
2. 키 컴파일 타임에 알 수 없는 키 값을 동적으로 일치 하는 것을 허용 합니다.
    
    ```
    NSPredicate.FromFormat(string.Format("{0} = '{1}'", key, value))
    ```
    
3. 일치 레코드의 값이 지정된 된 값 보다 크면 레코드:
   
    ```
    NSPredicate.FromFormat(string.Format("start > {0}", (NSDate)date))
    ```

4. 일치 레코드의 위치는 지정된 된 위치 100 미터 내 레코드:
    
    ```
    var location = new CLLocation(37.783,-122.404);
    var predicate = NSPredicate.FromFormat(string.Format("distanceToLocation:fromLocation(Location,{0}) < 100", location));
    ```

5. CloudKit은 토큰화 된 검색을 지원합니다. 이 호출에 대해 하나씩 두 개의 토큰 만들어집니다 `after` 용이고 다른 하나는 `session`합니다. 이러한 두 토큰을 포함 하는 레코드를 반환 합니다.
    
    ```
    NSPredicate.FromFormat(string.Format("ALL tokenize({0}, 'Cdl') IN allTokens", "after session"))
    ```
    
 6. CloudKit 지원 복합 사용 하 여 조인 조건자는 `AND` 연산자입니다.
    
    ```
    NSPredicate.FromFormat(string.Format("start > {0} AND name = '{1}'", (NSDate)date, recordName))
    ```
    


#### <a name="creating-queries"></a>쿼리 작성

다음 코드는 만드는 데 사용할 수는 `CKQuery` Xamarin iOS 8 응용 프로그램에서:

```csharp
var recordName = "MyRec";
var predicate = NSPredicate.FromFormat(string.Format("name = '{0}'", recordName));
var query = new CKQuery("CloudRecords", predicate);
```

첫째,만 지정 된 이름과 일치 하는 레코드를 선택 하는 조건자를 만듭니다. 그런 다음 조건자와 일치 하는 특정된 레코드 종류의 레코드를 선택 하는 쿼리를 만듭니다.

#### <a name="performing-a-query"></a>쿼리를 수행합니다.

쿼리를 만든 후 다음 코드를 사용 하 여 쿼리를 수행 하 고 반환 되는 레코드 처리:

```csharp
var recordName = "MyRec";
var predicate = NSPredicate.FromFormat(string.Format("name = {0}", recordName));
var query = new CKQuery("CloudRecords", predicate);

ThisApp.PublicDatabase.PerformQuery(query, CKRecordZone.DefaultRecordZone().ZoneId, (NSArray results, NSError err) => {
    // Was there an error?
    if (err != null) {
       ...
    } else {
        // Process the returned records
        for(nint i = 0; i < results.Count; ++i) {
            var record = (CKRecord)results[i];
        }
    }
});
```

위의 코드는 위에서 만든 쿼리는 공용 데이터베이스에 대해 실행 합니다. 지정 된 레코드가 영역이 없습니다 이후 모든 영역 검색 됩니다. 오류가 발생 하지 않으면, 배열을 `CKRecords` 쿼리의 매개 변수를 일치 하는 반환 됩니다.

쿼리에 대 한 생각 하는 방법은 되어 있어입니다 여론 조사, 큰 데이터 집합을 통해 조각화에 유용 합니다. 그러나 쿼리, 적합 하지 않습니다 대규모의 대부분 정적 데이터 집합에 대 한 이유:

-  장치가 배터리 수명에 대 한 잘못 된 됩니다.
-  네트워크 트래픽에 대 한 잘못 된 됩니다.
-  이들은 사용자 경험에 대 한 잘못 된 정보를 볼 수는 응용 프로그램 데이터베이스를 폴링하는 빈도 의해 제한 됩니다 하므로 합니다. 사용자를 변경 하면 현재 푸시 알림을 예상 합니다.


### <a name="subscriptions"></a>알림 신청

대규모의 대부분 정적 데이터 집합을 처리할 때는 쿼리 클라이언트 장치에서 수행 하지 않도록, 클라이언트 대신에 서버에서 실행 해야 합니다. 쿼리는 백그라운드에서 실행 해야 하며 현재 장치 또는 동일한 데이터베이스를 변경 하는 다른 장치에서 모든 단일 레코드를 저장 후 실행 해야 합니다.

마지막으로, 서버 쪽 쿼리를 실행할 때 데이터베이스에 연결 된 모든 장치에 푸시 알림을 보내야 합니다.

구독을 통해 CloudKit 프레임 워크에서 제공 되는 `CKSubscription` 클래스입니다. 레코드 종류 결합 ( `RecordType`), 조건자 ( `NSPredicate`) 및 Apple 푸시 알림 ( `Push`).

> [!NOTE]
> **참고**: CloudKit 원인을 발생에 대 한 요구와 같은 특정 정보를 포함 하는 페이로드를 포함 하는 대로 CloudKit 푸시 확대 약간 됩니다.

#### <a name="how-subscriptions-work"></a>구독이 작동 하는 방법

C# 코드에서 구독을 구현 하기 전에 구독 어떻게 작동 하는지에 대 한 간략 한 개요를 보겠습니다.

 [![](intro-to-cloudkit-images/image39.png "구독 어떻게 작동 하는지에 대 한 개요")](intro-to-cloudkit-images/image39.png#lightbox)

위의 그래프는 다음과 같은 일반적인 구독 프로세스를 보여 줍니다.

1.  클라이언트 장치를 트리거하는 구독 및 푸시 알림 트리거가 발생할 때 전송 되는 조건 집합을 포함 하는 새 구독을 만듭니다.
2.  구독은 데이터베이스에 전송 됩니다 기존 구독을 컬렉션에 추가 됩니다.
3.  두 번째 장치 새 레코드를 만들고 데이터베이스에 해당 레코드를 저장 합니다.
4.  데이터베이스를 새 레코드의 조건 중 하나라도 일치 하는 경우에 대 한 구독 목록 검색 합니다.
5.  일치 하는 항목이 구독을 트리거할 수를 일으킨 레코드에 대 한 정보로 등록 장치에 푸시 알림 보내집니다.


위치에이 정보를 살펴 보겠습니다 구독 Xamarin ios에서 8 응용 프로그램 만들기.

#### <a name="creating-subscriptions"></a>구독 만들기

구독을 만들려면 다음 코드를 사용할 수 있습니다.

```csharp
// Create a new subscription
DateTime date;
var predicate = NSPredicate.FromFormat(string.Format("start > {0}", (NSDate)date));
var subscription = new CKSubscription("RecordType", predicate, CKSubscriptionOptions.FiresOnRecordCreation);

// Describe the type of notification
var notificationInfo = new CKNotificationInfo();
notificationInfo.AlertLocalizationKey = "LOCAL_NOTIFICATION_KEY";
notificationInfo.SoundName = "ping.aiff";
notificationInfo.ShouldBadge = true;

// Attach the notification info to the subscription
subscription.NotificationInfo = notificationInfo;
```

첫째, 구독을 트리거하는 조건을 제공 하는 조건자를 만듭니다. 다음으로, 특정 레코드 종류에 대 한 구독을 만들고 트리거 검사 될 때의 옵션을 설정 합니다. 마지막으로 구독 트리거되고 구독을 연결 하는 경우 적용 되는 알림 유형을 정의 합니다.

### <a name="saving-subscriptions"></a>구독 저장

만든 구독에서 다음 코드는 데이터베이스에 저장 된:

```csharp
// Save the subscription to the database
ThisApp.PublicDatabase.SaveSubscription(subscription, (s, err) => {
    // Was there an error?
    if (err != null) {

    }
});
```

호출은 편리 하 게 API를 사용 하 여 비동기, 단순, 이며 쉽게 오류 처리를 제공 합니다.

#### <a name="handling-push-notifications"></a>푸시 알림 처리

개발자가 이전에 Apple 푸시 알림 (APS)을 사용한 CloudKit에 의해 생성 된 알림 처리 하는 프로세스 잘 알고 있어야 합니다.

에 `AppDelegate.cs`, 재정의 `ReceivedRemoteNotification` 클래스를 다음과 같이 합니다.

```csharp
public override void ReceivedRemoteNotification (UIApplication application, NSDictionary userInfo)
{
    // Parse the notification into a CloudKit Notification
    var notification = CKNotification.FromRemoteNotificationDictionary (userInfo);

    // Get the body of the message
    var alertBody = notification.AlertBody;

    // Was this a query?
    if (notification.NotificationType == CKNotificationType.Query) {
        // Yes, convert to a query notification and get the record ID
        var query = notification as CKQueryNotification;
        var recordID = query.RecordId;
    }
}
```

위의 코드 요청 CloudKit CloudKit 알림으로는 사용자 정보를 구문 분석 합니다. 다음으로 경고에 대 한 정보가 추출 됩니다. 마지막으로, 알림 유형을 테스트 하 고 알림을 적절 하 게 처리 됩니다.

이 섹션의 빅 데이터, 쿼리 및 구독을 사용 하 여 위에 작은 장치 문제에 대답 하는 방법으로 나타났습니다. 응용 프로그램이 클라우드에서 큰 데이터를 두고 이러한 기술을 사용 하 여이 데이터 집합에 보기를 제공 하는 합니다.

## <a name="cloudkit-user-accounts"></a>CloudKit 사용자 계정

이 문서의 시작 부분에 설명 했 듯이 CloudKit 기존 iCloud 인프라를 기반으로 작성 됩니다. 다음 섹션에서는 설명, 자세히 계정 CloudKit API를 사용 하 여 개발자에 게 노출 되는 방법.

### <a name="authentication"></a>인증

사용자 계정을 처리할 때 첫 번째 고려 인증입니다. CloudKit 장치에서 iCloud에 현재 로그인 한 사용자를 통해 인증을 지원합니다. 인증 백그라운드에서 발생 걸리며 iOS에 의해 처리 됩니다. 이러한 방식으로 개발자가 않아도 인증 구현의 세부 사항에 대해서는 걱정 됩니다. 참조는 사용자가 로그온 하는 경우에 테스트 합니다.

### <a name="user-account-information"></a>사용자 계정 정보

CloudKit은 개발자에 게 다음과 같은 사용자 정보를 제공합니다.

-  **Identity** – 사용자를 고유 하 게 식별 하는 방법입니다.
-  **메타 데이터** -저장 하 고 사용자에 대 한 정보를 검색할 수 있습니다.
-  **개인 정보 보호** – 모든 정보는 개인 정보 보호 합리적인 manor에서 처리 합니다. 아무 것을 사용자가 동의 하지 않는 한도 노출 합니다.
-  **검색** – 사용자에 게 자신의 친구 동일한 응용 프로그램을 사용 하는 검색 기능을 제공 합니다.


다음으로,이 주제를 자세히 살펴보겠습니다.

#### <a name="identity"></a>클레임

위에서 설명 했 듯이 CloudKit 응용 프로그램을 고유 하 게 지정된 된 사용자를 식별할 수 있는 방법을 제공 합니다.

 [![](intro-to-cloudkit-images/image40.png "고유 하 게 버전별 지정된 된 사용자")](intro-to-cloudkit-images/image40.png#lightbox)

사용자의 장치 및 모든 특정 사용자 개인 데이터베이스 CloudKit 컨테이너 내에서 실행 중인 클라이언트 응용 프로그램이 있습니다. 클라이언트 응용 프로그램이 특정 사용자 중 하나에 연결 되도록 하려고 합니다. 이 설정은 장치에서 로컬로 iCloud에 로그인 된 사용자에 기반 합니다.

이 iCloud에서 온 것인지, 이므로 다양 한 기능의 사용자 정보 저장소를 백업 합니다. 및 사용자 간에 상관 관계 수 있으므로 iCloud 실제로 컨테이너를 호스트 하 고, 합니다. 위 그래픽 사용자의에서 iCloud 계정이 `user@icloud.com` 현재 클라이언트에 연결 됩니다.

컨테이너 별로 고유, 임의로 생성 된 사용자 ID는 만들고 사용자의 iCloud 계정과 (전자 메일 주소)와 관련 된 합니다. 이 사용자 ID는 응용 프로그램에 반환 되 고 개발자에 게 표시 적합 한 방식으로에서 사용할 수 있습니다.

> [!NOTE]
> **참고**: 동일한 iCloud 사용자는 다른 사용자 Id는 서로 다른 CloudKit 컨테이너에 연결 되기 때문에 동일한 장치에서 실행 되는 서로 다른 응용 프로그램입니다.

다음 코드 가져옵니다 CloudKit 사용자 ID에 대 한 현재 로그인 한 장치에서 사용자의 iCloud에:

```csharp
public CKRecordID UserID { get; set; }
...

// Get the CloudKit User ID
CKContainer.DefaultContainer.FetchUserRecordId ((recordID, err) => {
    // Was there an error?
    if (err!=null) {
        Console.WriteLine("Error: {0}", err.LocalizedDescription);
    } else {
        // Save user ID
        UserID = recordID;
    }
});
```

위의 코드는 현재 로그인된 한 사용자의 ID를 제공 하는 CloudKit 컨테이너 요청할 수도 있습니다. 이 정보를 icloud와 서버에서에서 들어오는 이후 호출이 비동기 인지 및 오류 해결이 필요 합니다.

#### <a name="metadata"></a>메타데이터

CloudKit에 각 사용자에 게 이러한 기능을 설명 하는 특정 메타 데이터입니다. 이 메타 데이터는 CloudKit 레코드로 표시 됩니다.

 [![](intro-to-cloudkit-images/image41.png "CloudKit에 각 사용자에 게 이러한 기능을 설명 하는 특정 메타 데이터")](intro-to-cloudkit-images/image41.png#lightbox)

특정 사용자의 컨테이너를 찾고 개인 데이터베이스 내부는 해당 사용자를 정의 하는 하나의 레코드입니다. 데이터베이스 내부에 공용 컨테이너의 각 사용자에 게 하나씩 여러 사용자 레코드 있습니다. 이 중 한 현재 로그온 한 사용자의 레코드 id입니다. 일치 하는 레코드 ID

Public 데이터베이스의 사용자 레코드가 지 세계 읽을 수 있습니다. 대부분, 일반 레코드로 처리 되 고 유형의 `CKRecordTypeUserRecord`합니다. 이러한 레코드는 시스템에서 예약 되며 쿼리에 사용할 수 없습니다.

사용자 레코드에 액세스 하는 다음 코드를 사용 하세요.

```csharp
public CKRecord UserRecord { get; set; }
...

// Get the user's record
PublicDatabase.FetchRecord(UserID, (record ,er) => {
    //was there an error?
    if (er != null) {
        Console.WriteLine("Error: {0}", er.LocalizedDescription);
    } else {
        // Save the user record
        UserRecord = record;
    }
});
```

위의 코드는 공용 ID 위에 액세스 하는 것 인 사용자에 대 한 사용자 레코드를 반환할 데이터베이스 요청할 수도 있습니다. 이 정보를 icloud와 서버에서에서 들어오는 이후 호출이 비동기 인지 및 오류 해결이 필요 합니다.

#### <a name="privacy"></a>개인 정보 보호

CloudKit은 기본적으로 현재 로그온된 한 사용자의 개인 정보를 보호 하기 위해 디자인 되었습니다. 사용자 관련 개인 식별 정보는 기본적으로 노출 됩니다. 응용 프로그램 사용자에 대 한 제한 된 정보를 요구 합니다 경우도 있습니다.

이러한 경우 응용 프로그램은 사용자가이 정보를 공개할 요청할 수 있습니다. 대화 상자에 참여 하는 자신의 계정 정보를 노출 하는 데 묻는 사용자에 게 표시 됩니다.

#### <a name="discovery"></a>검색

가정 하는 사용자에 게 응용 프로그램을 허용 하 게 사용자 계정 정보에 대 한 액세스를 제한, 응용 프로그램의 다른 사용자가 검색 될 수 있습니다.

 [![](intro-to-cloudkit-images/image42.png "사용자는 응용 프로그램의 다른 사용자에 게 검색할 수 있습니다.")](intro-to-cloudkit-images/image42.png#lightbox)

클라이언트 응용 프로그램이 통신 하는 컨테이너 및 컨테이너 사용자 정보에 액세스할 수 iCloud와 통신 합니다. 사용자 전자 메일 주소를 제공할 수 있습니다 및 다시 사용자에 대 한 정보 검색을 사용할 수 있습니다. 필요에 따라 사용자 ID도 사용할 수 사용자에 대 한 정보를 검색 합니다.

CloudKit 현재 로그온 한의 friend 해야 할 수 있는 모든 사용자에 대 한 정보를 검색 하는 방법을 제공 전체 주소록을 쿼리하여 사용자 iCloud에 있습니다. CloudKit 프로세스에서 사용자의 연락처 책 끌어오도록 하 고 전자 메일 주소를 사용 하 여 다른 사용자가 해당 주소와 일치 하는 응용 프로그램을 찾을 수 있는 경우 참조 합니다.

따라서 응용 프로그램을에 대 한 액세스를 제공 하거나 연락처에 대 한 액세스 승인 하도록 사용자를 요청 하지 않고 사용자의 연락처 책을 활용할 수 있습니다. 어떤 연락처 정보를 사용할 수는 응용 프로그램을 CloudKit 프로세스에 액세스 합니다.

재생을 위해는 세 가지 다른 종류의 입력 사용자 검색에 사용할 수 있는

-  **사용자 레코드 ID** -현재 로그온 한 사용자 ID에 대해 검색을 수행할 수 있습니다 CloudKit 사용자 계정입니다.
-  **사용자 전자 메일 주소** – 사용자는 전자 메일 주소를 제공할 수 및 검색에 사용할 수 있습니다.
-  **책 문의** – 해당 연락처에 나열 된 동일한 전자 메일 주소를 포함 하는 응용 프로그램의 사용자를 검색 하는 사용자의 주소록을 사용할 수 있습니다.


사용자 검색에서 다음 정보를 반환 합니다.

-  **사용자 레코드 ID** -공용 데이터베이스에서 사용자의 고유 ID입니다.
-  **첫 번째 이름 및 성** -공용 데이터베이스에 저장 합니다.


이 정보에 선택한 검색 된 사용자에 대만 반환 됩니다.

다음 코드에서 장치에서 iCloud에 현재 로그인 한 사용자에 대 한 정보를 검색 합니다.

```csharp
public CKDiscoveredUserInfo UserInfo { get; set; }
...

// Get the user's metadata
CKContainer.DefaultContainer.DiscoverUserInfo(UserID, (info, e) => {
    // Was there an error?
    if (e != null) {
        Console.WriteLine("Error: {0}", e.LocalizedDescription);
    } else {
        // Save the user info
        UserInfo = info;
    }
});
```

다음 코드를 사용 하 여 연락처 책의 모든 사용자가 쿼리할 수 있습니다:

```csharp
// Ask CloudKit for all of the user's friends information
CKContainer.DefaultContainer.DiscoverAllContactUserInfos((info, er) => {
    // Was there an error
    if (er != null) {
        Console.WriteLine("Error: {0}", er.LocalizedDescription);
    } else {
        // Process all returned records
        for(int i = 0; i < info.Count(); ++i) {
            // Grab a user
            var userInfo = info[i];
        }
    }
});
```

이 섹션에서는 4 개의 주요 영역 CloudKit 응용 프로그램에 제공할 수 있는 사용자 계정에 대 한 액세스 설명 했습니다. 사용자의 Id 및 메타 데이터를 얻지 못하도록 개인 정보 보호 정책에 작성 된 CloudKit 및 마지막으로, 응용 프로그램의 다른 사용자를 검색 하는 기능입니다.

## <a name="the-development-and-production-environments"></a>개발 및 프로덕션 환경

CloudKit 응용 프로그램의 레코드 종류 및 데이터에 대 한 별도 개발 및 프로덕션 환경을 제공합니다. 개발 환경에는 개발 팀의 멤버에만 사용할 수 있는 보다 융통성 있는 환경입니다. 레코드에 새 필드를 추가 하 고 개발 환경에서 해당 레코드를 저장 하는 응용 프로그램, 하는 경우 서버 스키마 정보를 자동으로 업데이트 합니다.

개발자 시간을 절약할 수 있는 개발 하는 동안 스키마를 변경 하려면이 기능을 사용할 수 있습니다. 한 가지 주의할 점은 레코드에 필드를 추가한 후 필드와 연결 된 데이터 형식을 변경할 수 없습니다 프로그래밍 방식으로입니다. 필드의 종류를 변경 하려면 개발자의 필드를 삭제 해야 [CloudKit 대시보드](https://icloud.developer.apple.com/dashboard/) 새 형식으로 다시 추가 합니다.

응용 프로그램을 배포 하기 전에 개발자 마이그레이션할 수의 스키마 및 데이터를 사용 하 여 프로덕션 환경 **CloudKit 대시보드**합니다. 프로덕션 환경에 대해 실행 되 고 서버에 프로그래밍 방식으로 스키마를 변경 하 여 응용 프로그램 수 없습니다. 개발자의 변경 내용을 사용할 수 **CloudKit 대시보드** 하지만 오류를 프로덕션 환경 발생의 레코드에 필드를 추가 하려고 합니다.

> [!NOTE]
> **참고:** 시뮬레이터 에서만 작동 하는 iOS는 **개발 환경**합니다. 개발자는에서 응용 프로그램을 테스트할 준비가 때는 **프로덕션 환경**, 실제 iOS 장치는 필요 합니다.


## <a name="shipping-a-cloudkit-enabled-app"></a>응용 프로그램을 사용 하는 CloudKit 전달

CloudKit를 사용 하는 응용 프로그램을 제공 하기 전에 구성 해야 합니다 대상에는 **프로덕션 CloudKit 환경** 또는 응용 프로그램을 Apple에 의해 거부 됩니다.

다음을 수행합니다.

1. Ma에 대 한 Visual Studio에서 응용 프로그램을 컴파일할 **릴리스** > **iOS 장치**: 

    [![](intro-to-cloudkit-images/shipping01.png "릴리스에 대 한 응용 프로그램 컴파일")](intro-to-cloudkit-images/shipping01.png#lightbox)

2. **빌드** 메뉴 선택 **보관**: 

    [![](intro-to-cloudkit-images/shipping02.png "보관 파일을 선택 합니다.")](intro-to-cloudkit-images/shipping02.png#lightbox)

3. **보관** 만들어지고 Mac 용 Visual Studio에 표시 됩니다. 

    [![](intro-to-cloudkit-images/shipping03.png "보관 파일을 만든 표시")](intro-to-cloudkit-images/shipping03.png#lightbox)

4. **Xcode**를 시작합니다.
5. **창** 메뉴 선택 **구성 도우미**: 

    [![](intro-to-cloudkit-images/shipping04.png "구성 도우미를 선택 합니다.")](intro-to-cloudkit-images/shipping04.png#lightbox)

6. 응용 프로그램의 보관 파일을 선택 하 고 클릭는 **내보내기...**  단추: 

    [![](intro-to-cloudkit-images/shipping05.png "응용 프로그램의 보관 파일")](intro-to-cloudkit-images/shipping05.png#lightbox)
    
7. 내보내기에 대 한 방법을 선택 하 고 클릭 하 고 **다음** 단추: 

    [![](intro-to-cloudkit-images/shipping06.png "내보내기에 대 한 방법 선택")](intro-to-cloudkit-images/shipping06.png#lightbox)

8. 선택 된 **개발팀** 클릭 확인 하 고 드롭다운 목록에서는 **선택** 단추: 

    [![](intro-to-cloudkit-images/shipping07.png "드롭다운 목록에서 개발 팀을 선택 합니다.")](intro-to-cloudkit-images/shipping07.png#lightbox)

9. 선택 **프로덕션** 클릭 확인 하 고 드롭다운 목록에서는 **다음** 단추: 

    [![](intro-to-cloudkit-images/shipping08.png "드롭다운 목록에서 프로덕션을 선택 합니다.")](intro-to-cloudkit-images/shipping08.png#lightbox)

10. 설정 및 클릭 검토는 **내보내기** 단추: 

    [![](intro-to-cloudkit-images/shipping09.png "설정을 검토합니다")](intro-to-cloudkit-images/shipping09.png#lightbox)

11. 완성 된 응용 프로그램을 생성 하는 위치를 선택 `.ipa` 파일입니다.

이 프로세스는 iTunes Connect에 직접 응용 프로그램 제출을 위해 비슷합니다 클릭에 **전송 중...**  대신 구성 도우미 창에서 보관 파일을 선택한 후... 내보내기 단추가 있습니다.

## <a name="when-to-use-cloudkit"></a>CloudKit를 사용 하는 경우

이 문서에서 살펴본 대로 CloudKit 쉽게 저장 하 고 iCloud 서버에서 정보를 검색할 응용 프로그램을 제공 합니다. CloudKit 사용 되지 않는 않거나 기존 도구 또는 프레임 워크 중 하나를 사용 안 함.

### <a name="use-cases"></a>사용 사례

다음 사용 사례에는 특정 iCloud 프레임 워크 또는 기술 사용 시기를 결정 하는 개발자 데 도움이 됩니다.

-  **키-값 저장소 iCloud** -비동기적으로 적은 양의 데이터를 최신 상태로 유지 되었으며 응용 프로그램의 기본 작업에 매우 유용 합니다. 그러나 매우 적은 양의 정보에 대 한 제한 됩니다.
-  **icloud와 드라이브** – 기존 icloud와 문서 Api 기반 및 파일 시스템에서 구조화 되지 않은 데이터를 동기화 하는 단순 API를 제공 합니다. Mac OS x 전체 오프 라인 캐시를 제공 하 고 문서 중심 응용 프로그램에 대 한 훌륭한 됩니다.
-  **icloud와 핵심 데이터** – 데이터를 모든 사용자의 장치 간에 복제할 수 있습니다. 에 데이터는 단일 사용자 이며 keeping 개인, 구조화 된 데이터를 동기화에 매우 유용 합니다.
-  **CloudKit** – 공용 데이터 모두 구조를 제공 하 고 대량 및 크기가 큰 데이터 집합 및 큰 구조화 되지 않은 파일을 모두 처리할 수 있습니다. 제한은 사용자의 iCloud 계정과 하 고 제공 클라이언트 데이터 전송을 제어 합니다.


이러한 유지 사용 경우까지 이렇게 염두에서, 개발자는 현재 필요한 응용 프로그램 기능을 제공 하 고 이후 성장을 확장성을 제공 하는 올바른 icloud와 기술을 선택 해야 합니다.

## <a name="summary"></a>요약

이 문서는 CloudKit API에 대해 간략하게 소개를 검사가 수행 됩니다. 이 프로 비전 및 CloudKit를 사용 하는 Xamarin iOS 응용 프로그램을 구성 하는 방법으로 나타났습니다. CloudKit 편의 API의 기능 검사가 합니다. 표시에는 CloudKit를 디자인 하는 방법을 쿼리 및 구독을 사용 하 여 확장성을 위해 응용 프로그램을 사용 하도록 설정 합니다. 및 CloudKit 하 여 응용 프로그램에 노출 되는 사용자 계정 정보에 마지막 표시 된 합니다.

## <a name="related-links"></a>관련 링크

- [CloudKitAtlas (샘플)](https://developer.xamarin.com/samples/monotouch/ios8/CloudKitAtlas/)
- [iOS 8 소개](~/ios/platform/introduction-to-ios8.md)
- [프로비저닝 프로필을 만들려면](~/ios/get-started/installation/device-provisioning/index.md)
