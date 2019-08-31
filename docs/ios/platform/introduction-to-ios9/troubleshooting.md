---
title: Xamarin. iOS 9 – 문제 해결
description: 이 문서에서는 Xamarin.ios에서 iOS 9를 사용 하기 위한 다양 한 문제 해결 팁을 제공 합니다. 팁에는 XML 구문 분석, 시뮬레이터, 레이아웃 제약 조건, 네트워크 문제 및 기타 여러 항목이 포함 되어 있습니다.
ms.prod: xamarin
ms.assetid: DCE83E36-CBD9-4D96-8E7F-384CB8A54563
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/20/2017
ms.openlocfilehash: e6e264d9f1cd959c95a27597649d2ec23d832b1c
ms.sourcegitcommit: 1e3a0d853669dcc57d5dee0894d325d40c7d8009
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2019
ms.locfileid: "70200071"
---
# <a name="xamarinios-9--troubleshooting"></a>Xamarin. iOS 9 – 문제 해결

_이 문서에서는 Xamarin.ios 앱에서 iOS 9로 작업 하기 위한 몇 가지 문제 해결 팁을 제공 합니다._

## <a name="there-was-a-problem-parsing-the-xml"></a>XML을 구문 분석 하는 동안 문제가 발생 했습니다.

Xamarin iOS 디자이너는 Xcode 7 기능을 아직 지원 하지 않습니다. 스토리 보드는 StackView와 같은 새 iOS 9 (Xcode 7) 디자이너 요소를 사용 하려고 할 때 디자이너에서 _"XML을 구문 분석 하는 동안 문제가 발생 했습니다."_ 를 사용 하 여 로드 되지 않습니다.

iOS Designer Xcode 7 기능에 대 한 지원은 예정 된 주기 6 기능 릴리스를 대상으로 합니다. 주기 6의 미리 보기 버전은 현재 알파 채널에서 사용할 수 있으며 새로운 Xcode 7 기능을 제한적으로 지원 합니다.

Mac용 Visual Studio에 대 한 부분 해결 방법: 스토리 보드를 마우스 오른쪽 단추로 클릭 하 고**Xcode Interface Builder** **를 사용 하 여** > 열기를 선택 합니다.

## <a name="where-are-the-ios-8-simulators"></a>IOS 8 시뮬레이터는 어디에 있나요?

Xcode 7 이상을 설치한 경우 기본적으로 모든 iOS 8 시뮬레이터를 iOS 9 시뮬레이터로 자동으로 대체 합니다. IOS 8에서 테스트 해야 하는 경우 Xcode를 시작한 다음 iOS 8 시뮬레이터를 다운로드 하 여 설치할 수 있습니다.

Xcode에서 **Xcode** 메뉴를 선택 하 고 **기본 설정 ...** 을 선택 합니다.  >  **다운로드**:

[![](troubleshooting-images/ios8.png "iOS 8 시뮬레이터 다운로드")](troubleshooting-images/ios8.png#lightbox)

**확인 및 지금 설치** 단추를 클릭 하 여 iOS 8 시뮬레이터를 다시 설치 합니다.

## <a name="layout-constraint-with-leftright-attribute-errors"></a>왼쪽/오른쪽 특성 오류가 있는 레이아웃 제약 조건

IOS 8 (및 이전 버전)에서 storyboard의 UI 요소는 **오른쪽** & **왼쪽** `NSLayoutAttributeRight`특성 (`NSLayoutAttributeLeft` & )과 **선행** & **후행** 특성 ( )를동일한레이아웃`NSLayoutAttributeLeading`에배치합니다.  &  `NSLayoutAttributeTrailing`

에서 동일한 Storyboard가 iOS 9에서 실행 되는 경우 다음과 같은 형식으로 예외가 발생 합니다.

> Catch 되지 않은 예외 ' NSInvalidArgumentException '로 인해 앱을 종료 하는 중입니다. 이유: ' * * * + [NSLayoutConstraint constraintWithItem: attribute: relatedBy: toItem: attribute: 승수: constant:]: 선행/후행 특성과 오른쪽/왼쪽 특성 사이에는 제약 조건을 적용할 수 없습니다. 또는 둘 다에 대해 선행/후행을 사용 합니다. '

iOS 9는 레이아웃을 적용 하 여 **오른쪽** & **왼쪽** _또는_ **선행** & **후행** 특성 중 하나만 사용 합니다. 이 문제를 해결 하려면 스토리 보드 파일 내에 설정 된 것과 동일한 특성을 사용 하도록 모든 레이아웃 제약 조건을 변경 합니다.

자세한 내용은 [iOS 9 제약 조건 오류](https://stackoverflow.com/questions/32692841/ios-9-constraint-error) Stack Overflow 토론을 참조 하세요.

## <a name="error-itms-90535-unexpected-cfbundleexecutable-key"></a>ERROR ITMS-90535: 예기치 않은 CFBundleExecutable 키

IOS 9로 전환한 후 앱에서 iOS 8 (또는 이전 버전)에서 컴파일되고 실행 된 타사 구성 요소 (특히 기존 Google Maps 구성 요소)를 사용 하 여 새 빌드를 iTunes Connect에 제출 하려고 할 때 다음과 같은 형식으로 오류를 가져올 수 있습니다.

> ERROR ITMS-90535: 예기치 않은 CFBundleExecutable 키입니다. ' 페이로드/app-name/구성 요소. 번들 '의 번들에 번들 실행 파일이 포함 되어 있지 않습니다.

이 문제는 일반적으로 오류 메시지에서 키를 `Info.plist` `CFBundleExecutable` 제거 하 여 번들에 있는를 편집 하는 것과 같이 프로젝트에서 명명 된 번들을 찾아서 해결할 수 있습니다. 키도로 `BNDL` 설정 해야 합니다. `CFBundlePackageType`

이러한 변경을 수행한 후에는 전체 프로젝트를 정리 하 고 다시 빌드합니다. 이러한 변경을 수행한 후에는 문제 없이 iTunes Connect에 제출할 수 있습니다.

자세한 내용은이 [Stack Overflow](https://stackoverflow.com/questions/32096130/unexpected-cfbundleexecutable-key) 토론을 참조 하세요.

## <a name="cfnetwork-sslhandshake-failed--9824-error"></a>CFNetwork SSLHandshake 실패 (-9824) 오류

직접 또는 iOS 9의 웹 보기에서 인터넷에 연결 하려고 할 때 다음과 같은 형식으로 오류가 발생할 수 있습니다.

```csharp
2015-09-04 14:38:05.757 FormsWebViewiOS[2553:30362] CFNetwork SSLHandshake failed (-9824)
2015-09-04 14:38:05.758 FormsWebViewiOS[2553:30363] NSURLSession/NSURLConnection HTTP load failed (kCFStreamErrorDomainSSL, -9824)
```

또는 다음과 같은 형식으로 되어 있습니다.

```csharp
2015-09-04 14:39:17.881 FormsWebViewiOS[2568:30974] App Transport Security has blocked a cleartext HTTP (http://) resource load since it is insecure.
Temporary exceptions can be configured via your app's Info.plist file.
```

IOS9에서 ATS (App Transport Security)는 인터넷 리소스 (예: 앱의 백 엔드 서버)와 앱 간 보안 연결을 적용 합니다. 또한 ATS를 사용 하려면 프로토콜 및 `HTTPS` 높은 수준의 API 통신을 사용 하는 통신이 TLS 버전 1.2을 사용 하 여 암호화 되어야 합니다.

ATS는 iOS 9 및 OS X 10.11 (El Capitan) 용으로 빌드된 앱에서 기본적으로 사용 하도록 설정 되므로 또는 `NSURLConnection` `NSURLSession` 를 `CFURL` 사용 하는 모든 연결에는 ATS 보안 요구 사항이 적용 됩니다. 연결이 이러한 요구 사항을 충족 하지 않는 경우에는 예외와 함께 실패 합니다.

이 문제를 해결 하는 방법에 대 한 자세한 내용은 [앱 전송 보안](~/ios/app-fundamentals/ats.md) 가이드의 [ATS의 옵트아웃 (옵트아웃)](~/ios/app-fundamentals/ats.md) 섹션을 참조 하세요.

## <a name="my-existing-apps-dont-run-on-ios-9"></a>IOS 9에서 기존 앱이 실행 되지 않음

IOS 9에서 실행 되도록 기존 앱을 다시 빌드하고 다시 배포 하는 방법에 대 한 지침은 [ios 9 호환성 정보](~/ios/platform/introduction-to-ios9/ios9.md) 를 참조 하세요.

<a name="UICollectionViewCell.ContentView-is-null-in-constructors" />

## <a name="uicollectionviewcellcontentview-is-null-in-constructors"></a>UICollectionViewCell는 생성자에서 Null입니다.

**문서화** Ios 9 `initWithFrame:` 에서는 [UICollectionView 설명서 상태로](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UICollectionView_class/#//apple_ref/occ/instm/UICollectionView/dequeueReusableCellWithReuseIdentifier:forIndexPath)ios 9의 동작 변경으로 인해 생성자가 필요 합니다. 지정 된 식별자에 대 한 클래스를 등록 하 고 새 셀을 만들어야 하는 경우 이제 해당 `initWithFrame:` 메서드를 호출 하 여 셀을 초기화 합니다.

**방법을** 다음과 같이 `initWithFrame:` 생성자를 추가 합니다.

```csharp
[Export ("initWithFrame:")]
public YourCellClassName (CGRect frame) : base (frame)
{
    Initialize (); // refactor initialize code into a method
}
```

관련 샘플: [MotionGraph](https://github.com/xamarin/monotouch-samples/commit/3c1b7a4170c001e7290db9babb2b7a6dddeb8bcb), [TextKitDemo](https://github.com/xamarin/monotouch-samples/commit/23ea01b37326963b5ebf68bbcc1edd51c66a28d6)

<a name="UIView-fails-to-Init-with-Coder-when-Loading-a-View-from-a-Xib/Nib" />

## <a name="uiview-fails-to-init-with-coder-when-loading-a-view-from-a-xibnib"></a>Xib/Nib에서 뷰를 로드 하는 경우 UIView가 코드 작성자 있는지를 사용 하 여 초기화 되지 않습니다.

**문서화** 생성자 `initWithCoder:` 는 Interface Builder Xib 파일에서 뷰를 로드할 때 호출 되는 생성자입니다. 이 생성자가 내보내지 않는 경우 관리 되지 않는 코드는 관리 되는 버전을 호출할 수 없습니다. 이전 (예: iOS 8) `IntPtr` 에서 뷰를 초기화 하기 위해 생성자가 호출 되었습니다.

**방법을** 다음과 같이 생성자를 `initWithCoder:` 만들고 내보냅니다.

```csharp
[Export ("initWithCoder:")]
public YourClassName (NSCoder coder) : base (coder)
{
    Initialize (); // refactor initialize code into a method
}
```

관련 샘플: [채팅](https://github.com/xamarin/monotouch-samples/commit/7b81138d52e5f3f1aa3769fcb08f46122e9b6a88)

## <a name="dyld-message-no-cache-image-with-name"></a>Dyld 메시지: 이름이 인 캐시 이미지가 없습니다 ...

로그에서 다음 정보를 사용 하 여 충돌이 발생할 수 있습니다.

```csharp
Dyld Error Message:
Dyld Message: no cach image with name (/System/Library/PrivateFrameworks/JavaScriptCore.framework/JavaScriptCore)
```

**문서화** 이는 개인 프레임 워크를 공용으로 만들 때 (JavaScriptCore가 전용 프레임 워크 였던 이전에는 iOS 7에서 공용으로 설정 됨), 프레임 워크가 전용 인 경우 앱의 배포 대상이 iOS 버전용입니다. 이 경우 Apple의 링커는 공용 버전 대신 프레임 워크의 전용 버전과 연결 됩니다.

**방법을** 이는 iOS 9에 대 한 것 이지만 사용자에 게 직접 적용할 수 있는 해결 방법이 있습니다. 프로젝트에서 이후 iOS 버전을 대상으로 합니다 .이 경우 iOS 7을 사용해 볼 수 있습니다. 다른 프레임 워크는 비슷한 문제를 나타낼 수 있습니다. 예를 들어 ios 8에서 WebKit framework가 공용으로 설정 되었으므로 iOS 7을 대상으로 지정 하면이 오류가 발생 합니다. 앱에서 WebKit를 사용 하려면 iOS 8을 대상으로 해야 합니다.

## <a name="untrusted-enterprise-developer"></a>신뢰할 수 없는 엔터프라이즈 개발자

실제 iOS 하드웨어에서 iOS 9 버전의 Xamarin.ios 앱을 실행 하려고 할 때 개발자 계정이 장치에서 신뢰 되지 않았다는 메시지가 표시 될 수 있습니다. 예를 들어:

[![](troubleshooting-images/untrusted01.png "신뢰할 수 없는 엔터프라이즈 개발자 경고")](troubleshooting-images/untrusted01.png#lightbox)

이 문제를 해결 하려면 다음을 수행 합니다.

1. 개발 Mac에서 Xcode (최신 베타 버전)을 시작 합니다.
2. **창** 메뉴에서 **장치** 를 선택 하 여 장치 창을 엽니다. 

    [![](troubleshooting-images/untrusted02.png "장치 창")](troubleshooting-images/untrusted02.png#lightbox)
3. 장치 측면 패널에서 장치를 선택 하 고 마우스 오른쪽 단추를 클릭 한 다음 **프로 비전 프로필 표시**...를 선택 합니다. 

    [![](troubleshooting-images/untrusted03.png "프로 비전 프로필 표시")](troubleshooting-images/untrusted03.png#lightbox)
4. 현재 장치에서 각 프로 비전 프로필을 선택 하 고 **-** 단추를 클릭 하 여 삭제 합니다. 

    [![](troubleshooting-images/untrusted04.png "프로 비전 프로필 삭제")](troubleshooting-images/untrusted04.png#lightbox)
5. **Xcode** 메뉴에서 **기본 설정 ...** 및 **계정**을 선택 합니다. 

    [![](troubleshooting-images/untrusted05.png "Xcode 계정 기본 설정")](troubleshooting-images/untrusted05.png#lightbox)
6. **자세히 보기** ... 단추를 클릭 한 다음 **모두 다운로드** 단추를 클릭 합니다. 

    [![](troubleshooting-images/untrusted06.png "모든 프로필 다운로드")](troubleshooting-images/untrusted06.png#lightbox)
7. 목록의 업데이트가 완료 되 면 **완료** 단추를 클릭 하 고 기본 설정 창을 닫습니다.
8. IOS 장치에서 테스트 하려는 기존 버전의 Xamarin.ios 앱을 제거 합니다.
9. Mac용 Visual Studio으로 돌아가서 클린 빌드를 수행 하 고 장치에서 앱을 다시 실행 해 봅니다.

Xcode에 의해 로드 된 새 프로 비전 프로필이 표시 되기 전에 Mac용 Visual Studio를 중지 하 고 다시 시작 해야 할 수 있습니다. 새 프로 비전 프로필을 선택 하려면 Xamarin.ios 앱에 대 한 **Ios 번들 서명** 옵션을 조정 해야 할 수도 있습니다.

## <a name="launch-screen-issues"></a>시작 화면 문제

이제 iOS 9는 동일한 시작 이미지를 다른 인터페이스 방향을 지원 하기 위해 더 이상 재사용할 수 없도록 시작 화면 요구 사항을 강제로 적용 합니다. 자세한 내용은 Apple의 [UILanchImage 참조](https://developer.apple.com/library/prerelease/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html#//apple_ref/doc/uid/TP40009252-SW28) 를 참조 하세요.

필요에 따라 storyboard 파일을 사용 하 여 **.png** 이미지 파일 집합을 사용 하는 것과는 반대로, 앱의 시작 화면을 표시할 수 있습니다. 이제 시작 화면을 표시 하는 Apple의 선호 되는 방법입니다. 자세한 내용은 [통합 된 스토리 보드 소개 가이드를](~/ios/user-interface/storyboards/unified-storyboards.md) 참조 하세요.

마지막으로 앱은 시작 화면에 스토리 보드 파일을 사용 해야 하며 패널 또는 분할 보기 모드에서 실행 되는 것으로 간주 되는 네 가지 인터페이스 방향 (세로, 세로, 세로, 세로, 가로 왼쪽 및 가로 오른쪽)을 모두 지원 합니다. IOS 9의 새로운 멀티태스킹 기능에 대해 자세히 알아보려면 [iPad 용 멀티태스킹](~/ios/platform/multitasking.md) 가이드를 참조 하세요.

## <a name="nsinternalinconsistencyexception-exception"></a>NSInternalInconsistencyException 예외

IOS 9 용 기존 Xamarin.ios 앱을 컴파일하고 실행 하는 경우 다음과 같은 형식으로 오류가 발생할 수 있습니다.

> 목표-C 예외가 throw 되었습니다.  이름: NSInternalInconsistencyException 이유: 응용 프로그램 시작의 끝에는 응용 프로그램 창에 루트 뷰 컨트롤러가 있어야 합니다.

응용 프로그램 시작이 종료 될 때 응용 프로그램 창에 루트 뷰 컨트롤러가 있어야 하 고 기존 앱이 발생 하지 않기 때문에이 오류가 발생 합니다.

이 문제에 대 한 두 가지 이상의 해결 방법이 있습니다.

1. 응용 프로그램을 업데이트 하 여 `xib` 파일 대신 스토리 보드 파일을 사용 하 여 해당 사용자 인터페이스를 정의 합니다. 이 기능을 사용 하려면 앱 크기와 iOS Designer (또는 Xcode의 Interface Builder)를 사용 하 여 스토리 보드를 레이아웃 하는 방법에 따라 상당한 시간이 필요 합니다. 자세한 내용은 [통합 Storyboard 소개 설명서를](~/ios/user-interface/storyboards/unified-storyboards.md) 참조 하세요.
2. 응용 `RootViewController` 프로그램 UI의 뷰 컨트롤러 `FinishedLaunching` 를 가리키도록 `AppDelegate` 클래스의 메서드에서 앱 창의 설정 속성입니다.

## <a name="when-to-initialize-views-and-view-controllers"></a>뷰를 초기화 하 고 컨트롤러를 확인 하는 경우

Xamarin.ios를 사용 하면 관리 코드에 항목이 노출 될 때 호출 되는 생성자 내에서 뷰 또는 뷰 컨트롤러 초기화를 수행할 수 있지만 iOS 디자인은 중단 됩니다.

일반적으로 호출 되는 시기를 알 수 없으므로 생성자에서 백 목표-C 코드를 호출할 수 있는 모든 항목을 초기화 해서는 안 됩니다. 이는이 초기화를 수행 해야 하는 위치 (다른 .ctor) 또는 재정의 (목적-C에 이벤트 없음)가 더 적합 함을 의미 하기도 합니다.

## <a name="related-links"></a>관련 링크

- [개발자를 위한 iOS 9](https://developer.apple.com/ios/pre-release/)
- [IOS 9.0의 새로운 기능](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [IOS9에 Xamarin.ios 앱 업데이트 (비디오)](https://university.xamarin.com/lightninglectures/Updating-your-XamariniOS-apps-to-iOS9)
