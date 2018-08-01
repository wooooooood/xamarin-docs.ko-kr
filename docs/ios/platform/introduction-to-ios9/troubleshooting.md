---
title: Xamarin.iOS 9-문제 해결
description: 이 문서에서 iOS 9 Xamarin.iOS에 사용 하기 위한 다양 한 문제 해결 팁을 제공 합니다. 팁은 XML 구문 분석, 시뮬레이터, 레이아웃 제약 조건, 네트워크 문제 및 기타 여러 항목을 다룹니다.
ms.prod: xamarin
ms.assetid: DCE83E36-CBD9-4D96-8E7F-384CB8A54563
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: c44d737efcf5092eb4b27d5311271005de65318b
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787665"
---
# <a name="xamarinios-9--troubleshooting"></a>Xamarin.iOS 9-문제 해결

_이 문서에서는 Xamarin.iOS 앱에서 iOS 9 사용 하기 위한 몇 가지 문제 해결 팁을 제공 합니다._

## <a name="there-was-a-problem-parsing-the-xml"></a>XML 구문 분석 문제가 발생 했습니다.

Xamarin iOS 디자이너 아직 Xcode 7 기능을 지원 하지 않습니다. 스토리 보드 디자이너에 로드 되지 않는 _"XML 구문 분석 문제가 발생 했습니다"_ 새 iOS 9 (Xcode 7)를 사용 하려고 할 때 StackView 같은 디자이너 요소입니다.

Xcode 7 기능에 대 한 디자이너 지원을 iOS은 주기 6 기능 예정 된 릴리스에 대 한 대상 합니다. 주기 6의 미리 보기 버전 알파 채널에서 현재 사용할 수 있으며 새 Xcode 7 기능에 대 한 지원이 제한 합니다.

Mac 용 Visual Studio에 대 한 부분 해결: 스토리 보드를 마우스 오른쪽 단추로 클릭 하 고 선택 **프로그램** > **Xcode 인터페이스 작성기**합니다.

## <a name="where-are-the-ios-8-simulators"></a>어디 iOS 8 시뮬레이터 입니까?

Xcode 7 (또는 그 이상)를 설치한 경우 자동으로 모든 iOS 8 시뮬레이터 연결로 바뀝니다 iOS 9 시뮬레이터 기본적으로 합니다. IOS 8에서 테스트를 여전히 필요 하면, Xcode를 시작 하 고 다운로드 하 고, iOS 8 시뮬레이터를 설치할 수 있습니다.

Xcode에서 선택 된 **Xcode** 메뉴 다음 **기본 설정 중...**   >  **다운로드**:

[![](troubleshooting-images/ios8.png "iOS 8 시뮬레이터 다운로드")](troubleshooting-images/ios8.png#lightbox)

클릭는 **확인 하 고 지금 설치** 8 iOS 시뮬레이터를 다시 설치 하는 단추입니다.

## <a name="layout-constraint-with-leftright-attribute-errors"></a>왼쪽/오른쪽 특성 오류로 레이아웃 제약 조건

IOS 8 (및 이전), 스토리 보드의 UI 요소 사용 하 여 모두의 혼합 **오른쪽** & **왼쪽** 특성 (`NSLayoutAttributeRight` & `NSLayoutAttributeLeft`) 및  **선행** & **후행** 특성 (`NSLayoutAttributeLeading` & `NSLayoutAttributeTrailing`) 동일한 레이아웃 합니다.

동일한 스토리 보드 iOS 9 실행 하는 경우는 다음과 같은 형태로 예외가 발생 합니다.

> 'NSInvalidArgumentException' 확인할 수 없는 예외로 인해 응용 프로그램 종료 이유: ' * * * + [NSLayoutConstraint constraintWithItem:attribute:relatedBy:toItem:attribute:multiplier:constant:]: 제약 조건을 선행/후행 간에 만들 수 없습니다 특성 및 왼쪽/오른쪽 특성입니다. 둘 다 있거나 둘 다에 대 한 선행/후행 사용 합니다.'

사용 하 여 레이아웃을 적용 하는 iOS 9 **오른쪽** & **왼쪽** _또는_ **선행**  &   **후행** 특성 하지만 *하지* 둘 다 합니다. 이 문제를 해결 하려면 스토리 보드 파일 내에서 설정 하는 특성과 동일한 특성을 사용 하도록 모든 레이아웃 제약 조건을 변경 합니다.

자세한 내용은 참조 하십시오는 [iOS 9 제약 조건 오류가](http://stackoverflow.com/questions/32692841/ios-9-constraint-error) 토론 스택 오버플로 합니다.

## <a name="error-itms-90535-unexpected-cfbundleexecutable-key"></a>오류 ITMS 90535: 예기치 않은 CFBundleExecutable 키

IOS 9 전환한 후 응용 프로그램에서 3 번째 구성 요소 (특히 기존 Google 맵 구성 요소)에 컴파일하고 iTunes Connect 양식에서 오류가 발생할 수 있습니다에 새 빌드를 제출 하려고 할 때 iOS 8 (또는 이전 버전)에서 실행을 사용 합니다.

> 오류 ITMS 90535: 예기치 않은 CFBundleExecutable 키입니다. 번들 'Payload/app-name.app/component.bundle'에 번들 실행 파일 중...

이 문제 일반적으로 프로젝트의 명명 된 번들을 찾아 해결 한 다음 수 있습니다-바로 알 수 있듯이 오류 메시지-편집할는 `Info.plist` 제거 하 여 번들에 가장는 `CFBundleExecutable` 키입니다. `CFBundlePackageType` 키로 설정 해야 `BNDL` 도 합니다.

다음과 같이 변경한 후 정리를 수행 하 고 전체 프로젝트를 다시 작성 합니다. 이러한 변경을 수행한 후 iTunes Connect에서 문제 없이 제출할 수 있습니다.

자세한 내용은 참조 하십시오이 [스택 오버플로](http://stackoverflow.com/questions/32096130/unexpected-cfbundleexecutable-key) 토론 합니다.

## <a name="cfnetwork-sslhandshake-failed--9824-error"></a>CFNetwork SSLHandshake 실패 (-9824) 오류

인터넷에 직접 또는 iOS 9의에서 웹 보기에서 연결을 시도할 때 형식에서 오류가 발생할 수 있습니다.

```csharp
2015-09-04 14:38:05.757 FormsWebViewiOS[2553:30362] CFNetwork SSLHandshake failed (-9824)
2015-09-04 14:38:05.758 FormsWebViewiOS[2553:30363] NSURLSession/NSURLConnection HTTP load failed (kCFStreamErrorDomainSSL, -9824)
```

폼 또는:

```csharp
2015-09-04 14:39:17.881 FormsWebViewiOS[2568:30974] App Transport Security has blocked a cleartext HTTP (http://) resource load since it is insecure.
Temporary exceptions can be configured via your app's Info.plist file.
```

IOS9, 앱 전송 보안 (AT)는 인터넷 리소스 (예: 응용 프로그램의 백 엔드 서버)와 응용 프로그램 간 보안 연결을 적용합니다. 또한 AT 필요 하므로 사용 하는 통신의 `HTTPS` 프로토콜 및 높은 수준의 API 통신을 암호화 완전 보안 TLS 버전 1.2 사용 합니다.

AT 9 iOS 및 OS X 10.11 (El Capitan)에 대 한 모든 연결을 사용 하 여 빌드한 앱에 기본적으로 활성화 되어 있으므로 `NSURLConnection`, `CFURL` 또는 `NSURLSession` AT 보안 요구 사항이 적용 됩니다. 연결에는 이러한 요구 사항을 충족 하지 않는 경우 예외와 함께 실패 합니다.

참조 하십시오는 [AT Opting 아웃](~/ios/app-fundamentals/ats.md) 섹션 우리의 [앱 전송 보안](~/ios/app-fundamentals/ats.md) 이 문제를 해결 하는 방법에 대 한 가이드입니다.

## <a name="my-existing-apps-dont-run-on-ios-9"></a>IOS 9에서 기존 응용 프로그램 실행 안 함

참조 우리의 [iOS 9 호환성 정보](~/ios/platform/introduction-to-ios9/ios9.md) 에 대 한 지침은 다시 빌드하고 다시 배포 하 여 기존 앱 iOS 9에서 실행 되도록 합니다.

<a name="UICollectionViewCell.ContentView-is-null-in-constructors" />

## <a name="uicollectionviewcellcontentview-is-null-in-constructors"></a>UICollectionViewCell.ContentView은 생성자에 Null

**이유:** iOS 9에에서는 `initWithFrame:` 생성자는 이제 iOS 9와의 동작 변경 내용으로 인해 필요한는 [UICollectionView 설명서에 나와](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UICollectionView_class/#//apple_ref/occ/instm/UICollectionView/dequeueReusableCellWithReuseIdentifier:forIndexPath)합니다. 셀 호출 하 여 초기화 됩니다 지정된 된 식별자에 대 한 클래스를 등록 하 고 새로운 셀을 만들어야 하는 경우 해당 `initWithFrame:` 메서드.

**Fix:** 추가 `initWithFrame:` 다음과 같은 생성자:

```csharp
[Export ("initWithFrame:")]
public YourCellClassName (CGRect frame) : base (frame)
{
    Initialize (); // refactor initialize code into a method
}
```

관련 샘플: [MotionGraph](https://github.com/xamarin/monotouch-samples/commit/3c1b7a4170c001e7290db9babb2b7a6dddeb8bcb), [TextKitDemo](https://github.com/xamarin/monotouch-samples/commit/23ea01b37326963b5ebf68bbcc1edd51c66a28d6)

<a name="UIView-fails-to-Init-with-Coder-when-Loading-a-View-from-a-Xib/Nib" />

## <a name="uiview-fails-to-init-with-coder-when-loading-a-view-from-a-xibnib"></a>Xib/Nib에서 뷰를 로드할 때 UIView 코드 작성자를 초기화 하지

**이유:** 는 `initWithCoder:` 생성자는 인터페이스 작성기 Xib 파일에서 뷰를 로드할 때 호출 합니다. 이 생성자는 내보내지지 않습니다 비관리 코드의 관리 되는 버전 우리의 호출할 수 없습니다. 이전에 않음 (예: iOS 8)에 `IntPtr` 보기를 초기화할 수 있도록 생성자 호출 되었습니다.

**Fix:** 만들기 및 내보내기는 `initWithCoder:` 다음과 같은 생성자:

```csharp
[Export ("initWithCoder:")]
public YourClassName (NSCoder coder) : base (coder)
{
    Initialize (); // refactor initialize code into a method
}
```

관련된 샘플: [채팅](https://github.com/xamarin/monotouch-samples/commit/7b81138d52e5f3f1aa3769fcb08f46122e9b6a88)

## <a name="dyld-message-no-cache-image-with-name"></a>Dyld Message: 이름으로 캐시 이미지가 중...

로그에 다음 정보와 함께 충돌이 발생할 수 있습니다.

```csharp
Dyld Error Message:
Dyld Message: no cach image with name (/System/Library/PrivateFrameworks/JavaScriptCore.framework/JavaScriptCore)
```

**원인:** 개인 프레임 워크 공용 만들 때 발생 합니다. Apple의 네이티브 링커의 버그입니다 (JavaScriptCore 만들어진 iOS 7에서에서 공용 전에 이것은 개인 프레임 워크) 및 앱의 배포 대상 iOS 버전은 때는 프레임 워크 개인 했습니다. 이 경우 Apple의 링커는 공용 버전 대신 프레임 워크의 개인 버전과 연결 됩니다.

**Fix:** iOS 9에 대 한 해결 될 예정 이지만 그 동안에 직접 적용할 수는 쉽게 해결 방법: 방금 (연습할 수 iOS 7이 예제의) 프로젝트에서 나중 iOS 버전을 대상입니다. 다른 프레임 워크는 비슷한 문제를 나타낼 수 있습니다, 예를 들어 WebKit 프레임 워크 iOS 8에서에서 공용으로 설정 되었습니다 (및 없으므로 iOS 7을 대상으로 실행;이 오류가 발생 하면 대상으로 해야 iOS WebKit 응용 프로그램에서 사용 하는 8).

## <a name="untrusted-enterprise-developer"></a>신뢰할 수 없는 엔터프라이즈 개발자

IOS 9 버전 Xamarin.iOS 앱의 실제 iOS 하드웨어에서 실행을 시도할 때 개발자 계정이 장치에서 신뢰 하지 못했다는 메시지가 발생할 수 있습니다. 예를 들어:

[![](troubleshooting-images/untrusted01.png "신뢰할 수 없는 엔터프라이즈 개발자 경고")](troubleshooting-images/untrusted01.png#lightbox)

이 문제를 해결 하려면 다음을 수행 합니다.

1. Mac. 개발에서 Xcode (최신 베타 버전)를 시작 합니다.
2. 선택 **장치** 에서 **창** 메뉴 장치 창을 엽니다. 

    [![](troubleshooting-images/untrusted02.png "장치 창")](troubleshooting-images/untrusted02.png#lightbox)
3. 아래는 **장치** 패널 측면을 사용자의 장치를 마우스 오른쪽 단추 클릭 및 선택 선택 **프로 비전 프로필 보기...** : 

    [![](troubleshooting-images/untrusted03.png "SShow 프로 비전 프로필")](troubleshooting-images/untrusted03.png#lightbox)
4. 각 프로 비전 프로필 클릭 장치에서 현재 선택의 **-** 삭제 하는 단추: 

    [![](troubleshooting-images/untrusted04.png "프로 비전 프로필을 삭제합니다.")](troubleshooting-images/untrusted04.png#lightbox)
5. **Xcode** 메뉴 선택 **기본 설정 중...**  및 **계정**: 

    [![](troubleshooting-images/untrusted05.png "Xcode 계정 기본 설정")](troubleshooting-images/untrusted05.png#lightbox)
6. 클릭는 **세부 정보를 확인 중...**  클릭 한 다음 단추는 **모두 다운로드** 단추: 

    [![](troubleshooting-images/untrusted06.png "모든 프로필 다운로드")](troubleshooting-images/untrusted06.png#lightbox)
7. 목록 업데이트 완료 되 면 클릭는 **수행** 단추 및 기본 설정 창을 닫습니다.
8. IOS 장치에서 테스트 하려는 Xamarin.iOS 앱의 기존 버전을 제거 합니다.
9. Mac 용 Visual Studio로 되돌아가려면 하 고 깨끗 한 빌드를 수행 합니다. 장치에 앱을 다시 실행 하려고 합니다.

중지 하 고는 Xcode에서 로드 하는 새 프로비저닝 프로필을 표시 하기 전에 Mac 용 Visual Studio를 다시 시작 해야 합니다. 조정 해야 할 수 있습니다는 **iOS 번들 서명** 새 프로비저닝 프로필을 선택 Xamarin.iOS 앱에 대 한 옵션입니다.

## <a name="launch-screen-issues"></a>시작 화면 문제

iOS 9에서 시작 하는 이미지 방향을 다른 인터페이스를 지원 하기 위해 다시 더 이상 수 있도록에 이제 시작 화면 요구 사항을 적용 합니다. Apple의 참조 [UILanchImage 참조](https://developer.apple.com/library/prerelease/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html#//apple_ref/doc/uid/TP40009252-SW28) 자세한 정보에 대 한 합니다.

필요에 따라 집합을 사용 하지 않고 응용 프로그램의 시작 화면을 표시 하는 스토리 보드 파일을 사용할 수 있습니다 **.png** 이미지 파일입니다. 이제 Apple의 시작 화면을 표시 하는 방식으로 기본 설정입니다. 참조 하십시오 우리의 [스토리 보드 통합 소개](~/ios/user-interface/storyboards/unified-storyboards.md) 대 한 자세한 내용은 가이드입니다.

마지막으로, 응용 프로그램 시작 화면에 스토리 보드 파일을 사용 하 고 모든 4 개의 인터페이스 방향 (세로, 거꾸로 세로, 가로 왼쪽 및 오른쪽 가로) 패널을 통해 이동 또는 분할 보기 모드에서 실행 하기 위한 것으로 간주 되려면 지원 해야 합니다. IOS 9의 새로운 멀티태스킹 능력에 대 한 자세한 정보를 확인 하려면를 참조 하십시오 우리의 [iPad 용 멀티태스킹](~/ios/platform/multitasking.md) 가이드입니다.

## <a name="nsinternalinconsistencyexception-exception"></a>NSInternalInconsistencyException 예외

컴파일 및 iOS 9에 대 한 기존 Xamarin.iOS 앱을 실행 하는 경우에 형식에서 오류가 발생할 수 있습니다.

> Objective C 예외가 발생 했습니다.  이름: NSInternalInconsistencyException 이유: 응용 프로그램 시작 후에는 루트 보기 컨트롤러가 해야 응용 프로그램 창

이 오류는 응용 프로그램 Windows 응용 프로그램 시작 후에는 루트 보기 컨트롤러가 것으로 예상 되 고 기존 앱 하지 않습니다. 때문에 발생 합니다.

이 문제를 해결할 수 있는 방법은 두 개 이상 있습니다.

1. 업데이트 앱 대신 스토리 보드 파일을 사용 하 여 `xib` 해당 사용자 인터페이스를 정의 하는 파일입니다. 이 중 하나는 많은 양의 시간에 따라 앱의 크기 및 iOS 디자이너 (또는 Xcode의 인터페이스 작성기)를 사용 하는 방법 알고가 레이아웃 스토리 보드에 필요 합니다. 자세한 내용은 참조 우리의 [스토리 보드 통합 소개](~/ios/user-interface/storyboards/unified-storyboards.md) 설명서입니다.
2. 설치 프로그램 `RootViewController` 창 응용 프로그램의 속성에서 `FinishedLaunching` 에서 메서드 `AppDelegate` 클래스 응용 프로그램의 UI에서 뷰 컨트롤러를 가리키도록 합니다.

## <a name="when-to-initialize-views-and-view-controllers"></a>Initialize 뷰 및 컨트롤러 보기

Xamarin.iOS 이므로 iOS 디자인을 중단 하지만 관리 코드에 노출 되는 것 이라고 하는 생성자 내부 보기 또는 뷰-컨트롤러 초기화를 만들 수 있습니다.

일반적 하지 초기화 해야 코드 호출할 수 있는 다시 Objective-c 생성자에서 확인할 수 없습니다 이후 아무 것도 호출 때. 또한은 더 나은 환경 (다른.ctor) 또는 (Objective-c에 이벤트가)으로 재정의 하는 호출이이 초기화 해야 합니다.

## <a name="related-links"></a>관련 링크

- [개발자를 위한 iOS 9](https://developer.apple.com/ios/pre-release/)
- [IOS 9.0에 새로 이란](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Xamarin.iOS 앱 iOS9 (비디오)로 업데이트](https://university.xamarin.com/lightninglectures/Updating-your-XamariniOS-apps-to-iOS9)
