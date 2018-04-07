---
title: 일부 특정 라이선스 오류
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: D26BDF2D-923B-4BC1-841A-74583155DB71
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: 8592fe5381c974e999477d0ef6ca6ebdd8b38cc4
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="some-specific-licensing-errors"></a>일부 특정 라이선스 오류

> [!IMPORTANT]
> 관련 문제에 레거시 Xamarin 라이선스를 되기 때문에 아래 정보를 MSDN 사용자에 게 적용 되지 않습니다. MSDN 사용자 이며 것과 유사한 오류 아래를 참조 하는 경우 하세요 [Xamarin의 최신 버전으로 업데이트](https://developer.xamarin.com/recipes/cross-platform/ide/change_updates_channel/) &이 참조 하 여 [라이선스 옵션을 안내](~/cross-platform/get-started/requirements.md)합니다.



## <a name="invalid-license"></a>"잘못 된 라이선스"

> 잘못 된 라이선스입니다. Xamarin.Android (XA9999) 없습니다 라이선스 버전을 확인 하려면를 다시 활성화 하십시오. (XA9010)

이러한 메시지는 몇 가지 다른 상황에서 나타날 수 있습니다.

-   디스크에서 현재 라이선스 동기화 아웃 될 수 있습니다는 컴퓨터에서 현재 사용자 정보로 합니다. [라이선스 파일을 새로 고치](~/cross-platform/troubleshooting/legacy-licenses/resync-licenses.md) 대부분의 경우에서이 문제가 해결 됩니다.

-   컴퓨터에는 2 개의 충돌 하는 중복 된 활성화 수 있을 수 있습니다. 이러한 유형의 라이선스는 Xamarin에서 더 이상 사용 됩니다. 하십시오이 오류를 하 나와 관련 되어야 나타나는 문제가 발생 하면 [전자 메일 지원](https://www.xamarin.com/support)합니다.

-   Xamarin 계정 Xamarin 라이선스에 아직 초대할 수 있습니다. 참고 항목: [라이선스 관리 팀](~/cross-platform/troubleshooting/legacy-licenses/team-management.md)합니다.

## <a name="failed-to-load-android-entitlements"></a>"Android 자격을 로드 하지 못했습니다."

> Mono.VisualStudio.Shell.ShellPackage 오류: 0: Android 자격을 로드 하지 못했습니다: 잘못 된 라이선스입니다. Xamarin.Android (XA9999) Mono.VisualStudio.Shell.ShellPackage 오류 다시 활성화 하십시오: 0: 라이선스 업데이트 실패: 잘못 된 라이선스입니다. Xamarin.Android (XA9999) 다시 활성화 하십시오.

이 이전 버전의 위에서 "잘못 된 라이선스" 문제입니다. 동일한 단계 적용 됩니다.

## <a name="this-version-was-released-after-your-subscription-expired"></a>"이 출시 된이 버전은 구독 만료 되 면"

> 오류 XA9000: 구독 만료 된 후에이 버전은 출시 되었습니다 (2014/11/11/5: 오후 11:41). (XA9000) (Mobile.Android.Utilities) 오류 XA9010: 라이선스 버전을 확인할 수 없습니다. (XA9010) (Mobile.Android.Utilities)

이러한 메시지는 일반적으로 두 가지 경우에 표시 됩니다.

-   디스크에 라이선스는 오래 된입니다. [라이선스 파일을 새로 고치](~/cross-platform/troubleshooting/legacy-licenses/resync-licenses.md) 대부분의 경우에서이 문제가 해결 됩니다.

-   Xamarin 구독이 더 이상 활성화 되며, Xamarin의 현재 설치 된 버전은 구독 만료 날짜 보다 더 최신입니다. 올바른 수정 하는 것이 경우 [다운 그레이드](http://kb.xamarin.com/customer/portal/articles/1699777) xamarin 구독 만료 전에 출시 된 버전입니다. 전자 메일을 보내 주시기 [ hello@xamarin.com ](mailto:hello@xamarin.com) 를 구독에 대 한 유효한 최신 버전을 요청 합니다. 다운 그레이드 한 후 오류가 계속 나타나면 반드시 너무 라이선스 파일을 새로 고쳐 보세요.

## <a name="additional-references"></a>추가 참조

-   [라이선스를 새로 고치는 방법](~/cross-platform/troubleshooting/legacy-licenses/resync-licenses.md)
-   [Xamarin을 다운 그레이드 하는 방법](http://kb.xamarin.com/customer/portal/articles/1699777-downgrading)
-   [Xamarin.Android 오류 코드 목록](~/android/troubleshooting/errors.md)
-   [MonoTouch (Xamarin.iOS) 오류 코드 목록](~/ios/troubleshooting/mtouch-errors.md)

### <a name="next-steps"></a>다음 단계
추가 지원을 문의 또는 위의 정보를 활용 한 후에이 문제가 여전히 발생 하는 경우를 참조 하십시오 [지원 옵션에 대해 Xamarin에 대 한 사용할 수 있는?](~/cross-platform/troubleshooting/support-options.md) 연락처 옵션, 제안 사항에 대 한 내용은 하는 방법 필요한 경우 새 버그를 파일입니다.
