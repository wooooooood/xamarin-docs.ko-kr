---
title: Xamarin.iOS 9-문제 해결
description: 이 문서에서는 Xamarin.iOS에서 iOS 9 사용 하 여 작업에 대 한 다양 한 문제 해결 팁을 제공 합니다. XML 구문 분석, 시뮬레이터, 레이아웃 제약 조건, 네트워크 문제 및 다른 여러 주제 팁에 설명합니다.
ms.prod: xamarin
ms.assetid: DCE83E36-CBD9-4D96-8E7F-384CB8A54563
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/20/2017
ms.openlocfilehash: 322bb630194f973d37d7ca27a0ca9fe1b548b240
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50107217"
---
# <a name="xamarinios-9--troubleshooting"></a>Xamarin.iOS 9-문제 해결

_이 문서에서는 Xamarin.iOS 앱에 있는 iOS 9 사용 하 여 작업에 대 한 몇 가지 문제 해결 팁을 제공 합니다._

## <a name="there-was-a-problem-parsing-the-xml"></a>XML을 구문 분석 문제가 발생 했습니다.

Xamarin iOS 디자이너 Xcode 7 기능을 아직 지원 하지 않습니다. 스토리 보드 디자이너에서 로드 되지 것입니다 _"XML을 구문 분석 문제가 발생 했습니다."_ 새 iOS 9 (Xcode 7)를 사용 하려고 하면 StackView 같은 디자이너 요소입니다.

iOS Xcode 7 기능에 대 한 디자이너 지원을 곧 주기 6 기능 출시 대상으로 합니다. 주기 6 미리 보기 버전 알파 채널에서 현재 사용 가능한 및 제한적으로 새 Xcode 7 기능에 대 한 지원.

Mac 용 Visual Studio에 대 한 부분 해결 방법: storyboard를 마우스 오른쪽 단추로 클릭 하 고 선택 **연결** > **Xcode Interface Builder**합니다.

## <a name="where-are-the-ios-8-simulators"></a>여기서? iOS 8 시뮬레이터

Xcode 7 (또는 그 이상)을 자동으로 설치한 경우 모든 iOS 8 시뮬레이터 9 iOS 시뮬레이터를 사용 하 여 기본적으로 바꾸어야 합니다. IOS 8에서 테스트 해야 하는 경우 Xcode를 시작 하 고 다운로드 하 고, iOS 8 시뮬레이터를 설치할 수 있습니다.

Xcode에서 선택 합니다 **Xcode** 한 다음 메뉴 **기본 설정...**   >  **다운로드**:

[![](troubleshooting-images/ios8.png "iOS 8 시뮬레이터 다운로드")](troubleshooting-images/ios8.png#lightbox)

클릭 합니다 **확인 하 고 지금 설치** 단추는 iOS 8 시뮬레이터를 다시 설치 합니다.

## <a name="layout-constraint-with-leftright-attribute-errors"></a>왼쪽/오른쪽 특성 오류를 사용 하 여 레이아웃 제약 조건

IOS 8 (및 이전 버전), 스토리 보드에서 UI 요소 사용 하 여 두 가지 조합 **오른쪽** & **왼쪽** 특성 (`NSLayoutAttributeRight` & `NSLayoutAttributeLeft`) 및  **선행** & **후행** 특성 (`NSLayoutAttributeLeading` & `NSLayoutAttributeTrailing`) 고 같은 레이아웃.

동일한 스토리 보드에서 iOS 9에서 실행 하는 경우 다음 형식의 예외 발생:

> 앱 'NSInvalidArgumentException' 확인할 수 없는 예외로 인해 종료 이유: ' * * * + [NSLayoutConstraint constraintWithItem:attribute:relatedBy:toItem:attribute:multiplier:constant:]: 선행/후행 간의 제약 조건을 만들 수 없습니다 특성 및 오른쪽/왼쪽 특성입니다. 둘 다 또는 둘 다에 대 한 선행/후행 사용 합니다.'

iOS 9를 사용 하 여 레이아웃 적용 **오른쪽** & **왼쪽** _하거나_ **선행**  &   **후행** 특성이 있지만 *하지* 둘 다. 이 문제를 해결 하려면 스토리 보드 파일 내에서 설정 된 동일한 특성을 사용 하는 모든 레이아웃 제약 조건을 변경 합니다.

자세한 내용은 참조 하십시오 합니다 [iOS 9 제약 조건 오류가](http://stackoverflow.com/questions/32692841/ios-9-constraint-error) 스택 오버플로 토론 합니다.

## <a name="error-itms-90535-unexpected-cfbundleexecutable-key"></a>오류 ITMS-90535: 예기치 않은 CFBundleExecutable 키

IOS 9 전환한 후 앱에서 타사 구성 요소 (특히 기존 Google Maps 구성 요소)에 컴파일하고 새 빌드를 iTunes Connect 양식에서 오류가 발생할 수 있습니다를 제출 하려고 할 때 iOS 8 (또는 이전)에서 실행을 사용 합니다.

> 오류 ITMS-90535: 예기치 않은 CFBundleExecutable 키입니다. 'Payload/app-name.app/component.bundle'에서 번들에는 실행 파일을 번들 없습니다...

이 문제 수 일반적으로 프로젝트의 명명 된 번들을 검색 하 여 해결 후 다시 설치 해야-바로 알 수 있듯이 오류 메시지-편집 합니다 `Info.plist` 제거 하 여 번들에는 `CFBundleExecutable` 키입니다. 합니다 `CFBundlePackageType` 키로 설정 해야 `BNDL` 도 합니다.

다음과 같이 변경한 후 정리를 수행 하 고 전체 프로젝트를 다시 빌드하십시오. 이러한 변경을 수행한 후 문제 없이 iTunes Connect에 전송할 수 있어야 합니다.

자세한 내용은 참조 하십시오이 [Stack Overflow](http://stackoverflow.com/questions/32096130/unexpected-cfbundleexecutable-key) 토론 합니다.

## <a name="cfnetwork-sslhandshake-failed--9824-error"></a>CFNetwork SSLHandshake 실패 (-9824) 오류

직접 또는 ios 9 웹 보기에서 인터넷에 연결을 시도할 때 오류가 발생할 수 있습니다는 형식:

```csharp
2015-09-04 14:38:05.757 FormsWebViewiOS[2553:30362] CFNetwork SSLHandshake failed (-9824)
2015-09-04 14:38:05.758 FormsWebViewiOS[2553:30363] NSURLSession/NSURLConnection HTTP load failed (kCFStreamErrorDomainSSL, -9824)
```

또는 형식에서:

```csharp
2015-09-04 14:39:17.881 FormsWebViewiOS[2568:30974] App Transport Security has blocked a cleartext HTTP (http://) resource load since it is insecure.
Temporary exceptions can be configured via your app's Info.plist file.
```

IOS9, 앱 전송 보안 ATS ()는 인터넷 리소스 (예: 앱의 백 엔드 서버)와 앱 간에 보안 연결을 적용합니다. 또한 ATS 필요 사용 하 여 통신을 `HTTPS` 프로토콜 및 전달 완전 보안을 사용 하 여 TLS 버전 1.2 사용 하 여 암호화 될 높은 수준의 API 통신 합니다.

ATS는 모든 연결을 사용 하 여 iOS 9 및 OS X 10.11 (El Capitan) 용으로 빌드된 앱에서 기본적으로 활성화 되어 있으므로 `NSURLConnection`하십시오 `CFURL` 또는 `NSURLSession` ATS 보안 요구 사항이 적용 됩니다. 연결에 이러한 요구 사항에 맞지 않는 경우 예외가 발생 하 여 실패 합니다.

참조 하세요 합니다 [Opting 옵트아웃 ATS](~/ios/app-fundamentals/ats.md) 섹션의 우리의 [앱 전송 보안](~/ios/app-fundamentals/ats.md) 이 문제를 해결 하는 방법에 대 한 정보에 대 한 가이드입니다.

## <a name="my-existing-apps-dont-run-on-ios-9"></a>내 기존 앱은 iOS 9에서 실행 하지 마세요

참조 우리의 [iOS 9 호환성 정보](~/ios/platform/introduction-to-ios9/ios9.md) iOS 9에서 실행을 다시 빌드하고 다시 배포 하는 기존 앱에 대 한 지침에 대 한 합니다.

<a name="UICollectionViewCell.ContentView-is-null-in-constructors" />

## <a name="uicollectionviewcellcontentview-is-null-in-constructors"></a>UICollectionViewCell.ContentView가 Null 생성자

**원인:** Ios 9 `initWithFrame:` 생성자는 이제 iOS 9로의 동작 변경 내용으로 인해 필요한 합니다 [UICollectionView 설명서에 나와](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UICollectionView_class/#//apple_ref/occ/instm/UICollectionView/dequeueReusableCellWithReuseIdentifier:forIndexPath). 셀을 호출 하 여 초기화 됩니다 지정된 된 식별자에 대 한 클래스를 등록 하 고 새 셀을 만들어야 하는 경우 해당 `initWithFrame:` 메서드.

**해결 방법:** 추가 된 `initWithFrame:` 다음과 같은 생성자:

```csharp
[Export ("initWithFrame:")]
public YourCellClassName (CGRect frame) : base (frame)
{
    Initialize (); // refactor initialize code into a method
}
```

관련 샘플: [MotionGraph](https://github.com/xamarin/monotouch-samples/commit/3c1b7a4170c001e7290db9babb2b7a6dddeb8bcb), [TextKitDemo](https://github.com/xamarin/monotouch-samples/commit/23ea01b37326963b5ebf68bbcc1edd51c66a28d6)

<a name="UIView-fails-to-Init-with-Coder-when-Loading-a-View-from-a-Xib/Nib" />

## <a name="uiview-fails-to-init-with-coder-when-loading-a-view-from-a-xibnib"></a>UIView는 Xib/Nib에서 뷰를 로드할 때 코드 작성자를 사용 하 여 초기화 하지 못함

**원인:** 는 `initWithCoder:` 생성자 인터페이스 작성기 Xib 파일에서 뷰를 로드할 때 호출 됩니다. 이 생성자는 내보내지지 않음 경우 비관리 코드는 관리 되는 버전을 호출할 수 없습니다. 이전에 (예: iOS 8)에 `IntPtr` 뷰를 초기화 하려면 생성자 호출 되었습니다.

**해결 방법:** 만들기 및 내보내기는 `initWithCoder:` 다음과 같은 생성자:

```csharp
[Export ("initWithCoder:")]
public YourClassName (NSCoder coder) : base (coder)
{
    Initialize (); // refactor initialize code into a method
}
```

관련된 샘플: [채팅](https://github.com/xamarin/monotouch-samples/commit/7b81138d52e5f3f1aa3769fcb08f46122e9b6a88)

## <a name="dyld-message-no-cache-image-with-name"></a>Dyld 메시지: 캐시 이미지가 없는 이름...

로그에서 다음 정보를 사용 하 여 충돌을 발생할 수 있습니다.

```csharp
Dyld Error Message:
Dyld Message: no cach image with name (/System/Library/PrivateFrameworks/JavaScriptCore.framework/JavaScriptCore)
```

**원인:** 할 개인 프레임 워크를 공개 하는 경우 발생 하는 Apple 네이티브 링커가의 버그입니다 (JavaScriptCore 수행 된 iOS 7에서에서 공개 하기 전에 개인 프레임 워크 였습니다) 앱의 배포 대상은 iOS 버전 이며 경우는 프레임 워크가 개인입니다. 이 경우 Apple의 링커는 공용 버전 대신 프레임 워크의 전용 버전을 사용 하 여 연결 됩니다.

**해결 방법:** iOS 9에 대 한 해결 될 것 이지만 그 동안에 직접 적용할 수 있습니다 문제를 쉽게 해결: 방금 이상 (보십시오 iOS 7이 예제의) 프로젝트에서 iOS 버전을 대상입니다. 다른 프레임 워크와 비슷한 문제가 발생할 수 있습니다, 예를 들어 WebKit framework iOS 8에서에서 공용으로 설정 되었습니다 (및 iOS 앱에서 WebKit를 사용 하는 8을 두어야 이므로이 오류를 발생 하면 iOS 7을 대상으로).

## <a name="untrusted-enterprise-developer"></a>신뢰할 수 없는 엔터프라이즈 개발자

실제 iOS 하드웨어에서 Xamarin.iOS 앱의 iOS 9 버전을 실행 하려고 시도할 때, 개발자 계정의 장치에서 신뢰할 수 있는 메시지가 표시 될 수 있습니다. 예를 들어:

[![](troubleshooting-images/untrusted01.png "신뢰할 수 없는 엔터프라이즈 개발자 경고")](troubleshooting-images/untrusted01.png#lightbox)

이 문제를 해결 하려면 다음을 수행 합니다.

1. Xcode (최신 베타 버전) Mac. 개발 시작
2. 선택 **장치** 에서 합니다 **창** 장치 창을 열려면 메뉴: 

    [![](troubleshooting-images/untrusted02.png "장치 창")](troubleshooting-images/untrusted02.png#lightbox)
3. 아래는 **장치** 패널 쪽에서 사용자 장치를 마우스 오른쪽 단추로 클릭 하 여 선택 **프로 비전 프로필을 표시 하는 중...** : 

    [![](troubleshooting-images/untrusted03.png "SShow 프로 비전 프로필")](troubleshooting-images/untrusted03.png#lightbox)
4. 장치 및 클릭에서 현재 각 프로 비전 프로필을 선택 합니다 **-** 삭제 하는 단추: 

    [![](troubleshooting-images/untrusted04.png "프로 비전 프로필을 삭제 하는 중")](troubleshooting-images/untrusted04.png#lightbox)
5. **Xcode** 메뉴에서 **기본 설정...**  하 고 **계정**: 

    [![](troubleshooting-images/untrusted05.png "Xcode 계정 기본 설정")](troubleshooting-images/untrusted05.png#lightbox)
6. 클릭 된 **세부 정보를 확인 하는 중...**  단추를 클릭 합니다 **모든 다운로드** 단추: 

    [![](troubleshooting-images/untrusted06.png "모든 프로필 다운로드")](troubleshooting-images/untrusted06.png#lightbox)
7. 목록 업데이트 완료 되 면 클릭 합니다 **수행** 단추 및 기본 설정 창을 닫습니다.
8. IOS 장치에서 테스트 하려는 Xamarin.iOS 앱의 기존 버전을 제거 합니다.
9. Mac 용 Visual Studio로 돌아가서, 클린 빌드를 수행 및 장치에서 앱을 다시 실행 하려고 합니다.

중지 하 고 Xcode에서 로드 하는 새 프로 비전 프로필은 표시 하려면 먼저 Mac 용 Visual Studio를 다시 시작 해야 합니다. 조정 해야 할 수도 있습니다는 **iOS 번들 서명** 새 프로 비전 프로필을 선택 하 여 Xamarin.iOS 앱에 대 한 옵션입니다.

## <a name="launch-screen-issues"></a>시작 화면 문제

iOS 9가 다른 인터페이스 방향을 지원 하기 위해 더 이상 동일한 시작 이미지를 다시 사용할 수 있도록에 이제 시작 화면 요구 사항을 적용 합니다. Apple의를 참조 하세요 [UILanchImage 참조](https://developer.apple.com/library/prerelease/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html#//apple_ref/doc/uid/TP40009252-SW28) 자세한 내용은 합니다.

필요에 따라 집합이 아닌 앱의 시작 화면을 표시 하는 스토리 보드 파일을 사용할 수 있습니다 **.png** 이미지 파일입니다. 이제 Apple의 시작 화면을 제공 하는 방식으로 기본 설정입니다. 참조 하십시오 우리의 [통합 스토리 보드 소개](~/ios/user-interface/storyboards/unified-storyboards.md) 자세한 가이드입니다.

마지막으로 앱 시작 화면에 스토리 보드 파일을 사용 하 고 모든 네 가지 인터페이스 방향 (세로, 거꾸로 세로, 가로 왼쪽 및 오른쪽 가로)이 슬라이드를 통해 패널에 또는 분할 보기 모드에서 실행 중인 것으로 간주 지원 해야 합니다. IOS 9의 새로운 멀티태스킹 기능에 대 한 자세한 내용을 알아보려면, 하세요 우리의 [iPad 용 멀티태스킹](~/ios/platform/multitasking.md) 가이드입니다.

## <a name="nsinternalinconsistencyexception-exception"></a>NSInternalInconsistencyException 예외

컴파일 및 iOS 9에 대 한 기존 Xamarin.iOS 앱을 실행 하는 경우 오류가 발생할 수 있습니다는 형식:

> Objective C 예외를 throw 됩니다.  이름: NSInternalInconsistencyException 이유: 응용 프로그램 windows 응용 프로그램 시작 후에 루트 보기 컨트롤러 해야

이 오류는 앱 Windows 응용 프로그램 시작 후에 루트 뷰 컨트롤러를 해야 하 고 기존 앱 하지 때문에 발생 합니다.

이 문제를 해결할 수 있는 최소 두 가지가 있습니다.

1. 대신 스토리 보드 파일을 사용 하도록 앱 업데이트 `xib` 파일을 해당 사용자 인터페이스를 정의 합니다. 이 상당히 많은 시간에 따라 앱의 크기와 iOS 디자이너 (또는 Xcode의 Interface Builder)를 사용 하는 방법에 대 한 지식이 스토리 보드 레이아웃에 필요 합니다. 자세한 내용은 참조 하세요. 당사의 [통합 스토리 보드 소개](~/ios/user-interface/storyboards/unified-storyboards.md) 설명서.
2. 설치 프로그램 `RootViewController` 앱 창의 속성에서 `FinishedLaunching` 에서 메서드 `AppDelegate` 앱 UI에서 뷰 컨트롤러를 가리키도록 클래스입니다.

## <a name="when-to-initialize-views-and-view-controllers"></a>뷰와 뷰 컨트롤러를 초기화 하는 경우

Xamarin.iOS를 사용 하 여 관리 코드에 노출 되는 것 이지만 iOS 디자인 중단 될 때 호출 되는 생성자 내부 보기 또는 뷰 컨트롤러 초기화를 확인 하는 것이 같습니다.

일반적 하지 초기화 해야 호출할 수 있는 다시 Objective-c 코드를 생성자에서 확인할 수 있으므로 모든 호출 하면 됩니다. 또한 즉는 더 나은 위치 (.ctor 다른) 또는 재정의 (Objective-c에 이벤트가 없습니다)에 대 한 호출이이 초기화 해야 합니다.

## <a name="related-links"></a>관련 링크

- [개발자를 위한 iOS 9](https://developer.apple.com/ios/pre-release/)
- [새로운 iOS 9.0에서](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [IOS9 (비디오)를 Xamarin.iOS 앱을 업데이트 하는 중입니다.](https://university.xamarin.com/lightninglectures/Updating-your-XamariniOS-apps-to-iOS9)
