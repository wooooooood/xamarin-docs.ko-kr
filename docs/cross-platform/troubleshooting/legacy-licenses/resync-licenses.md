---
title: 어떻게 수행 하나요를 수동으로 다시 동기화 Xamarin 라이선스?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: D0BD93E9-3A1F-4E5B-8EE8-36ADC33DCFE4
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: b06a1a7d525c91d7c3973b2b02d3d2835ce482f9
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="how-do-i-manually-resynchronize-xamarin-licenses"></a>어떻게 수행 하나요를 수동으로 다시 동기화 Xamarin 라이선스?

> [!IMPORTANT]
> 이 가이드를 소유 하거나 사용 하지 않는 Xamarin 계정에 로그인 하지 않아도 됩니다 때문에 대부분 MSDN 사용자에 게 적용 되지 않습니다는 [Xamarin 구성 요소를 저장](https://components.xamarin.com/) 또는 [Mac (Mac)에 대 한 Visual Studio](~/cross-platform/get-started/requirements.md)합니다.




## <a name="overview"></a>개요

일반적으로 라이선스 정보 동기화 됩니다 Xamarin 라이선스 서버와 자동으로 IDE를 시작 합니다. 예를 들어 라이선스 업그레이드, 다운 그레이드, 및 갱신 IDE를 다시 시작한 후 일반적으로 자동으로 표시 됩니다. IDE에서 표시 되는 라이선스 정보에 표시 되는 정보를 일치 해야 프로그램 [계정 페이지](https://store.xamarin.com/account/my/subscription/computers)합니다. 정보를 여전히 일치 하지 않는 경우 저장소 계정 정보를 정확히 표시 하 고 다음 로그 아웃 하 고 디스크에 라이선스 파일을 삭제 한 다음 다시 로그인 하 여 라이선스를 수동으로 동기화를 먼저 다시 확인 수 있습니다.

### <a name="note-for-ios-developers-on-windows"></a>Windows에서 iOS 개발자를 위한 참고 사항

Windows에서 iOS 개발자 Mac;에 대 한 Visual Studio에 대 한 이러한 단계를 수행 해야 할 수도 경우에 쌍을 이루는 Mac에서 빌드 호스트에서 직접 개발 하지 마십시오.

## <a name="quick-manual-refresh-steps"></a>빠른 수동 새로 고침 단계

이 빠른 프로세스에서는 문서의 일부 경우에는 충분 한 디스크에 라이선스 파일을 삭제 하는 단계를 건너뜁니다. 

1.  로그 아웃 IDE 통해 Xamarin 계정:
    -   Visual Studio for Mac
        -   시작 화면 오른쪽 위 모서리
        -   **Mac 용 visual Studio > 계정** (Mac)
        -   **도구 > 계정** (Windows)
    -   Visual Studio
        -   **도구 > Xamarin 계정**
2.  IDE에서 Xamarin 계정에 다시 로그인 합니다.

## <a name="extended-manual-license-refresh-steps"></a>수동 라이선스 새로 고침 단계를 확장합니다.

1.  IDE 통해 Xamarin 계정 로그입니다. 
2.  IDE를 종료 합니다.
3.  저장소 계정 정보 제대로 표시 되는지 확인 합니다. 특히에 중복 된 컴퓨터 이름에 대 한 확인은 [컴퓨터 페이지](https://store.xamarin.com/account/my/subscription/computers)합니다.

4.  한 쌍의 컴퓨터 이름이 중복 표시를 사용 하 여는 **비활성화** 제거할 드롭 다운 메뉴 항목 _둘 다_ 쌍의 구성원:
    
    ![라이선스에 Deactivate를-> https://store.xamarin.com/account/my/subscription/computers ] (resync-licenses-images/deactivate.png "Deactivate 드롭 다운 메뉴 항목을 사용 하 여 쌍의 두 구성원을 제거 하려면")

5.  디스크에 남아 있는 라이선스 파일의 나머지 복사본을 모두 삭제 합니다.
    -   Windows

        다음 폴더를 모두 삭제 합니다.
        -   `%PROGRAMDATA%\Mono for Android`
        -   `%PROGRAMDATA%\MonoTouch`
        -   `%LOCALAPPDATA%\VirtualStore\ProgramData\Mono for Android`
        -   `%LOCALAPPDATA%\VirtualStore\ProgramData\MonoTouch`

        Windows 기본 설치에서는:
        -   `%PROGRAMDATA%` 다음과 같이 확장 됩니다. `C:\ProgramData`
        -   `%LOCALAPPDATA%` 다음과 같이 확장 됩니다. `C:\Users\%USERNAME%\AppData\Local`

        `VirtualStore` 라이선스의 복사본이 자동으로 생성 됩니다 Windows에서 특정 시나리오에서 합니다. VirtualStore 복사본 있으면 읽힙니다 _대신_ 에 라이선스 `%PROGRAMDATA%`합니다.

    -   Mac

        다음 폴더를 모두 삭제 합니다.

        -   `~/Library/MonoAndroid`
        -   `~/Library/MonoTouch`
        -   `~/Library/Xamarin.Mac`

        이러한 경로에 액세스할 수 있습니다 **Finder** 선택 하 여는 **이동 > 폴더로 이동** 메뉴 및 대화 상자 창에 붙여 넣는 방법입니다. Finder 자동 대체는 ~ 사용자 폴더와 물결표 문자입니다.

6.  Mac 용 Visual Studio 또는 Visual Studio 열고 로그 Xamarin 계정에 백업합니다.

## <a name="supplementary-information-individual-license-file-locations"></a>추가 정보: 개별 라이선스 파일 위치

### <a name="windows"></a>Windows

Windows에서 개별 라이선스 파일은 다음 위치에 저장 됩니다.

-   Xamarin.Android:  
     `%PROGRAMDATA%\Mono for Android\License\monoandroid.licx`
-   Xamarin.iOS:  
     `%PROGRAMDATA%\MonoTouch\License\monotouch.licx`

에 대 한 라이선스 *평가판* 구독 다르게 명명 된:

-   Xamarin.Android:  
     `%PROGRAMDATA%\Mono for Android\License\monoandroid.trial.licx`
-   Xamarin.iOS:  
     `%PROGRAMDATA%\MonoTouch\License\monotouch.trial.licx`

### <a name="mac"></a>Mac

Mac에서 개별 라이선스 파일은 다음 위치에 저장 됩니다.

-   Xamarin.Android:  
     `~/Library/MonoAndroid/License`
-   Xamarin.iOS:  
     `~/Library/MonoTouch/License.v2`
-   Xamarin.Mac:  
     `~/Library/Xamarin.Mac/License`

에 대 한 라이선스 *평가판* 구독 다르게 명명 된:

-   Xamarin.Android:  
     `~/Library/MonoAndroid/License.trial`
-   Xamarin.iOS:  
     `~/Library/MonoTouch/License.trial`
-   Xamarin.Mac:  
     `~/Library/Xamarin.Mac/License.trial`

## <a name="additional-troubleshooting-steps"></a>추가 문제 해결 단계

-   사용자 이름, 컴퓨터 이름, 또는에서 완전히 확장 된 라이선스 파일의 경로 있는 모든 비 ASCII 문자가 있는지 확인 합니다.

-   [Xamarin 개발자 지원에 문의](http://xamarin.com/support)
